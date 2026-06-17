# 🛠️ RL-FMU Integrations-Bugs: Die 5 kritischen Implementierungsfallen

Diese Notiz dokumentiert die fünf kritischen Software-Defekte, die bei der Kopplung von Reinforcement Learning Frameworks (wie **Stable-Baselines3**) mit physikalischen Digital Twins (**Functional Mock-up Units / FMUs** via `FMPy`/`Gymnasium`) auftreten. Diese Fehler sind rein struktureller Natur, erzeugen keine harten Fehlermeldungen, sondern sabotieren das Training lautlos (*Silent Failures*).

---

## 📌 Bug 1: VecNormalize verliert seine gleitenden Statistiken (`VecNormalize Stats Loss`)

### 🔍 Fehlerbeschreibung
Der `VecNormalize`-Wrapper von Stable-Baselines3 (SB3) trackt den gleitenden Mittelwert ($\mu$) und die Varianz ($\sigma^2$) aller Beobachtungen (States), um sie auf $(s - \mu)/\sigma$ zu skalieren. Beim Speichern von Checkpoints wurde jedoch unbeabsichtigt nur ein leerer Platzhalter gespeichert. 

Beim Fortsetzen des Trainings (*Resume*) initialisierte sich `VecNormalize` neu mit Standardwerten ($\mu=0, \sigma=1$). Das vortrainierte neuronale Netz erhielt dadurch völlig anders skalierte Eingabewerte (z.B. rohe Drücke/Temperaturen statt normalisierter Werte).

> [!danger] Auswirkung
> Der Agent empfängt "Garbage-Inputs". Die Performance bricht sofort nach dem Laden des Checkpoints ein (z.B. Reward-Absturz von ~130 auf ~6). Das Training stagniert dauerhaft in Phase 0.

### 💻 Code-Vergleich

**Fehlerhafte Implementierung (Checkpoint-Code):**
```python
# Der Bug: Es wird nur ein Platzhalter ohne die echten Werte gesichert
checkpoint_metadata = {
    "policy_weights": model.policy.state_dict(),
    "vecnormalize_stats": {"obs_rms": None}  # Unbeabsichtigter Null-Placeholder!
}
torch.save(checkpoint_metadata, "checkpoint.pt")
```

**Korrekte Lösung (First-Class Artifact):**
```python
# Aufwand beim Speichern (Save):
env.save(checkpoint_dir / "vecnorm_stats.pkl")
model.save(checkpoint_dir / "model_weights.zip")

# Aufwand beim Fortsetzen (Resume):
from stable_baselines3.common.vec_env import VecNormalize

loaded_env = VecNormalize.load(checkpoint_dir / "vecnorm_stats.pkl", venv)
model = PPO.load(checkpoint_dir / "model_weights.zip", env=loaded_env)
```

---

## 📌 Bug 2: Asynchrones Episoden-Timing (`Rollout vs. Episode Alignment`)

### 🔍 Fehlerbeschreibung
Der Fortschritt der Schwierigkeitsstufen (*Curriculum Advancement*) wurde in der Methode `on_rollout_end` überprüft. Ein SB3-Rollout feuert fest alle `n_steps` (z. B. 2048 Umgebungsschritte). Wenn 8 Umgebungen parallel laufen und eine Episode nur 120 Schritte dauert, finden innerhalb eines Rollouts etwa 136 Episodenabschlüsse statt. 

`on_rollout_end` prüft aber standardmäßig nur den *letzten* Zustand des Vektors. Die Wahrscheinlichkeit, dass eine Episode exakt im Schritt 2048 endet, liegt statistisch bei unter 1 %. Fast alle Episodenabschlüsse wurden somit ignoriert.

> [!danger] Auswirkung
> Der Trigger für den Wechsel in die nächste Lernphase sieht niemals genug abgeschlossene Episoden. Das Curriculum verharrt unendlich lange auf der aktuellen Stufe, obwohl der Agent die Kriterien längst erfüllt hat.

### 💻 Code-Vergleich

**Fehlerhafte Implementierung (Callback-Ebene):**
```python
class CurriculumCallback(BaseCallback):
    def on_rollout_end(self) -> bool:
        # PRÜFT NUR ALLE 2048 STEPS: Die meisten Episodenenden werden übersehen!
        if self.training_env.get_attr("episode_corrupted")[0]:
            self.track_metrics()
        return True
```

**Korrekte Lösung:**
```python
class CurriculumCallback(BaseCallback):
    def _on_step(self) -> bool:
        # PRÜFT JEDEN EINZELNEN SCHRITT: Erkennt Abbrüche (dones) sofort in allen Subprozessen
        for env_idx, done in enumerate(self.locals["dones"]):
            if done:
                episode_info = self.locals["infos"][env_idx].get("episode")
                self.register_episode_completion(episode_info)
        return True
```

---

## 📌 Bug 3: Doppelte physikalische Skalierung (`Double Unit Scaling`)

### 🔍 Fehlerbeschreibung
Das physikalische Basismodell (FMU) berechnete die Verdichter- bzw. Heizleistung in der SI-Einheit **Watt [W]**. Der Python-Schnittstellen-Adapter (`FMPyAdapter`) transformierte diesen Wert richtigerweise mittels eines Faktors von $10^{-6}$ in **Megawatt [MW]**, um dem Netz kleinere Zahlenwerte zu übergeben. In der Konfigurationsdatei der Gym-Umgebung war jedoch fälschlicherweise ein zweiter Skalierungsfaktor hinterlegt (`w_net_unit_scale: 1.0e-6`).

> [!danger] Auswirkung
> Die Leistungskomponente innerhalb der Belohnungsfunktion (Reward) kam im Bereich von $10^{-12}$ an (**Mikrowatt**). Damit wurde der physikalische Leistungs-Reward de facto zu mathematischem Rauschen reduziert. Der Agent lernte nur die Temperaturregelung, ignorierte den Stromverbrauch aber komplett.

### 💻 Code-Vergleich

**Fehlerhafte Implementierung (Reward-Berechnung):**
```python
# FMU liefert 5000 W -> Adapter macht 0.005 MW
power_mw = fmu.get("power") * 1e-6 

# Der Bug: Erneute Skalierung in der Reward-Funktion
reward_power = power_mw * config["w_net_unit_scale"]  # 0.005 * 1e-6 = 5e-9 W!
reward = target_reward - reward_power
```

**Korrekte Lösung:**
```python
# Eindeutige Kompetenztrennung: Der Adapter besitzt die Einheitenhoheit
df_config["w_net_unit_scale"] = 1.0  

# Belohnung arbeitet strikt im standardisierten MW-Bereich
reward_power = power_mw * 1.0
```

---

## 📌 Bug 4: Datenruinen bei Phasenübergängen (`Stale Profile KeyErrors`)

### 🔍 Fehlerbeschreibung
Beim automatischen Aufstieg in eine neue Curriculum-Phase (z.B. von *Steady State* zu *Transienter Störung*) änderte sich die Konfigurationsstruktur. Die Umgebung versuchte, transiente Störparameter (z.B. Amplitudenwerte für Wetteränderungen) abzufragen. Da das Störprofil-Dictionary jedoch nur einmalig bei Methodenaufruf von `env.reset()` initialisiert wurde, enthielt es diese Keys nicht.

> [!danger] Auswirkung
> Es kam zu einem unvorhergesehenen `KeyError` im Hintergrund. Da Gymnasium-Vektorumgebungen (`SubprocVecEnv`) in isolierten Python-Subprozessen laufen, stürzte der Thread lautlos ab (*Silent Worker Crash*). Das Hauptskript fror ohne Fehlermeldung unendlich ein.

### 💻 Code-Vergleich

**Fehlerhafte Implementierung (Phase-Wechsel):**
```python
def update_curriculum_phase(self, new_phase):
    self.current_phase = new_phase
    # Das alte Störprofil bleibt unverändert im Speicher stehen!
```

**Korrekte Lösung:**
```python
def update_curriculum_phase(self, new_phase):
    self.current_phase = new_phase
    # Atomares (vollständiges) Überschreiben und Neuaufbauen des Profils
    self.disturbance_profile = self.generate_profile_for_phase(new_phase)
    self.fmu.reset() # Notwendiger Re-Init der Solver-Zustände
```

---

## 📌 Bug 5: Blockiertes Lernen durch Null-Toleranz-Regel (`The Zero-Violation Trap`)

### 🔍 Fehlerbeschreibung
Um maximale Betriebssicherheit zu garantieren, wurde die Bedingung für den Aufstieg im Curriculum extrem restriktiv gesetzt: `require_zero_constraint_violations: True`. Sobald der Agent im Verlauf einer Phase auch nur ein einziges Mal eine kritische Systemgrenze verletzte (z.B. Absinken der Verdampfungstemperatur unter das Limit), wurde die gesamte Phase als "nicht bestanden" gewertet.

Da stochastische RL-Algorithmen (wie [[PPO (Proximal Policy Optimization)|PPO]] oder [[QR-DQN (Quantile Regression DQN)|QR-DQN]] während der Explorationsphase zwingend den Zustandsraum explorieren *müssen*, berühren sie diese Grenzen mathematisch unweigerlich.

> [!danger] Auswirkung
> Dem Agenten wurde jegliche Möglichkeit genommen, zu lernen, *wo* die Grenze überhaupt liegt und wie man sie vermeidet. Der Aufstieg im Curriculum wurde strukturell unmöglich gemacht, das System blockierte sich selbst.

### 💻 Code-Vergleich

**Fehlerhafte Implementierung (Logik-Prüfung):**
```python
# Null-Toleranz-Infrastruktur verhindert Konvergenz
if phase_violations > 0:
    allow_curriculum_advance = False
```

**Korrekte Lösung (Lagrangian Relaxation & Separation):**
1. **Im Training:** Erlaube bis zu 10 % Verletzungen während der stochastischen Exploration. Bestrafe diese über adaptive Lagrange-Multiplikatoren ($\lambda$).
2. **Im Betrieb (Deployment):** Nutze einen mathematischen Sicherheitsfilter (*Constraint Projection Quadratic Program / QP*), der die Actions des Netzes im Millisekundentakt auf zulässige physikalische Grenzen projiziert.

```python
# Erlaubt kontrolliertes Lernen am Limit im Training:
if (phase_violations / total_steps) <= 0.10 and mean_reward >= target_reward:
    allow_curriculum_advance = True
```

---

## 💡 Key Takeaways für Dymola / TIL-HeatPump

1. **Einheiten-Checkliste:** Schreibe dir für deine Arbeit eine explizite Tabelle mit den Einheiten der FMU (TIL nutzt meist standardmäßig SI: Kelvin, Watt, kg/s) und den Variablen in Python.
2. **Smoke-Test nach Resume:** Programmiere einen kurzen Testlauf. Sichere das Modell bei Schritt X, lade es sofort wieder und vergleiche, ob der allererste berechnete Reward nach dem Laden exakt identisch mit dem Wert vor dem Speichern ist. Weicht er ab, verlierst du `VecNormalize`-Statistiken.
3. **Subprozess-Debugging:** Nutze beim Entwickeln der Gym-Umgebung anfangs immer den `DummyVecEnv` (sequenziell). Erst wenn alles fehlerfrei läuft, schalte um auf `SubprocVecEnv` (parallel). `SubprocVecEnv` schluckt und maskiert Python-Fehlermeldungen!

---
#fmu #reinforcement-learning #stable-baselines #modelica #dymola #debugging