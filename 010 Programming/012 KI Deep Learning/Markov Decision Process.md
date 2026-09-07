---
title: "Markov Decision Process"
type: konzept
status: fertig
tags:
  - reinforcement-learning
  - deep-learning
  - ai
  - agents
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 19.1 (Markov decision processes, returns, policies)

---

## 1. Wozu das Ganze?
[[Reinforcement Learning]] braucht eine formale Beschreibung der Aufgabe, sonst kann man nichts optimieren. Der **Markov Decision Process (MDP)** ist genau diese Beschreibung: er sagt, *welche Zustände es gibt*, *was der Agent tun darf*, *wie die Welt reagiert* und *was belohnt wird*.

Alles was danach kommt ([[Bellman Gleichung und Dynamic Programming]], [[Temporal Difference Learning und Q-Learning]], [[Policy Gradient und Actor Critic]]) setzt einen MDP voraus.

## 2. Aufbau: der Markov-Baukasten
Prince baut das in drei Stufen auf:

| Stufe | Bestandteile | Was fehlt noch? |
| :--- | :--- | :--- |
| **Markov-Prozess** | Zustände $s$, Übergänge $Pr(s_{t+1}\|s_t)$ | keine Belohnung, keine Aktionen |
| **Markov Reward Process** | + Reward $r_t$ | keine Aktionen — man schaut nur zu |
| **Markov Decision Process** | + Aktionen $a$ | vollständig |

### Markov-Eigenschaft
$$Pr(s_{t+1} \mid s_t, a_t, s_{t-1}, a_{t-1}, \dots) = Pr(s_{t+1} \mid s_t, a_t)$$

Der nächste Zustand hängt **nur** vom aktuellen Zustand und der aktuellen Aktion ab — nicht von der Vorgeschichte. Der Zustand muss also alles enthalten, was für die Zukunft relevant ist.

> **Praxisfalle:** Genau hier scheitern viele reale Umgebungen. Wenn im Zustand eine relevante Größe fehlt (z. B. bei einer Abtauung die *Dauer* seit dem letzten Abtauvorgang oder die Reifschicht-Historie), ist der Prozess nicht mehr Markov'sch und der Agent lernt schlecht. Abhilfe: Zustand erweitern (Frame-Stacking, gleitende Mittelwerte, Zeit seit Ereignis X) oder rekurrente Policies.

### MDP formal
Ein MDP ist das Tupel $\langle \mathcal S, \mathcal A, Pr(s_{t+1}|s_t,a_t), r(s_t,a_t), \gamma \rangle$:

* $\mathcal S$ — Zustandsraum (diskret oder kontinuierlich)
* $\mathcal A$ — Aktionsraum (diskret: An/Aus, Ventilstufe; kontinuierlich: Drehzahl, Ventilöffnung)
* **Transition** $Pr(s_{t+1}|s_t,a_t)$ — Dynamik der Umgebung (bei dir: das Dymola-/FMU-Modell)
* **Reward** $r(s_t,a_t)$ — skalares Feedback pro Schritt
* **Discount** $\gamma \in [0,1]$

### Partially Observable MDP (POMDP)
Sieht der Agent nicht den echten Zustand $s_t$, sondern nur eine Beobachtung $o_t$, spricht man von einem POMDP. Der Agent muss dann aus der Beobachtungshistorie einen internen Zustand ("belief state") konstruieren.

## 3. Trajektorie / Rollout
Die Interaktion erzeugt eine Folge

$$\tau = [s_1, a_1, r_1, s_2, a_2, r_2, \dots]$$

genannt **Trajektorie**, **Rollout** oder **Episode**. Man unterscheidet:

* **Episodisch:** endet in einem Terminalzustand (Spiel gewonnen, Simulation zu Ende).
* **Fortlaufend (continuing):** läuft prinzipiell ewig — hier ist $\gamma < 1$ zwingend, sonst divergiert die Summe.

## 4. Return und Discount-Faktor
Nicht der einzelne Reward zählt, sondern die **abdiskontierte Summe der zukünftigen Rewards** ab Zeitpunkt $t$ — der **Return**:

$$G_t = \sum_{k=0}^{\infty} \gamma^{k}\, r_{t+k+1} = r_{t+1} + \gamma\, r_{t+2} + \gamma^{2} r_{t+3} + \dots$$

Rekursiv (die Grundlage aller Bellman-Gleichungen):

$$G_t = r_{t+1} + \gamma\, G_{t+1}$$

**Warum diskontieren?**
1. Mathematisch: macht die Summe bei unendlichem Horizont endlich.
2. Modellierung: Unsicherheit über die ferne Zukunft; sofortige Belohnung ist mehr wert.
3. Praktisch: $\gamma$ steuert die **Weitsicht** des Agenten.

| $\gamma$ | Verhalten | effektiver Horizont $\approx \frac{1}{1-\gamma}$ |
| :--- | :--- | :--- |
| $0$ | rein gierig, nur der nächste Reward zählt | 1 Schritt |
| $0.9$ | mittelfristig | ~10 Schritte |
| $0.99$ | weitsichtig (SB3-Default) | ~100 Schritte |
| $0.999$ | sehr weitsichtig, träges Lernen | ~1000 Schritte |

> **Wichtig für Simulationen:** $\gamma$ muss zur **Zeitschrittweite** passen. Bei 1-Sekunden-Schritten sieht $\gamma = 0.99$ nur ~100 s weit — für einen Abtauzyklus über Stunden viel zu kurzsichtig. Entweder größere Zeitschritte (Frame-Skipping) oder $\gamma$ nach oben.

## 5. Policy
Die **Policy** $\pi$ ist die Strategie des Agenten:

* **Deterministisch:** $a_t = \pi(s_t)$
* **Stochastisch:** $a_t \sim \pi(a_t \mid s_t)$ — Wahrscheinlichkeitsverteilung über Aktionen

Stochastische Policies sind im Training fast immer besser: sie sorgen automatisch für **Exploration** und sind bei POMDPs sogar nachweislich überlegen.

**Ziel des RL:** Finde die Policy, die den erwarteten Return maximiert:

$$\pi^{*} = \arg\max_{\pi}\ \mathbb E_{\tau \sim \pi}\!\left[\sum_{t} \gamma^{t} r_{t+1}\right]$$

## 6. Warum ist das schwer?
Prince nennt vier Eigenschaften, die RL von [[Supervised Learning]] unterscheiden:

1. **Stochastizität:** Umgebung und Policy sind zufällig — dieselbe Aktion liefert unterschiedliche Ergebnisse.
2. **Temporal Credit Assignment:** Der Reward kommt oft erst viel später. Welche der 500 vorherigen Aktionen war schuld?
3. **Exploration vs. Exploitation:** Der Agent muss selbst Daten erzeugen und weiß nie, ob es woanders besser gewesen wäre.
4. **Nichtstationarität:** Die Datenverteilung ändert sich, während die Policy lernt. Die "Trainingsmenge" ist kein fester Datensatz.

## 7. Reward Design
Der Reward ist die einzige Stelle, an der man dem Agenten sagt, *was* er will — nie *wie*.

* **Sparse Reward:** nur am Ende (+1 bei Erfolg). Ehrlich, aber sehr schwer zu lernen.
* **Dense / Shaped Reward:** jeder Schritt bekommt Feedback. Lernt schneller, aber der Agent findet zuverlässig jedes Schlupfloch in deiner Formel (**Reward Hacking**).
* **Potential-based Shaping** $F = \gamma\Phi(s') - \Phi(s)$ ist die einzige Shaping-Form, die die optimale Policy garantiert nicht verändert.

Typische Struktur bei Regelungsaufgaben (siehe [[Model Predictive Control]] — dort ist es die Kostenfunktion mit umgekehrtem Vorzeichen):

$$r = -\underbrace{w_1 (T - T_{soll})^2}_{\text{Regelabweichung}} - \underbrace{w_2 P_{el}}_{\text{Energie}} - \underbrace{w_3 \mathbb 1[\text{Schaltvorgang}]}_{\text{Verschleiß}}$$

## 8. Bezug zur Gym-API
Die `gymnasium`-Schnittstelle ist eine 1:1-Umsetzung des MDP:

```python
obs, info = env.reset()             # s_1
obs, reward, terminated, truncated, info = env.step(action)
#  s_{t+1}, r_{t+1}, Terminalzustand?, Zeitlimit?, Debug
```

* `observation_space` → $\mathcal S$
* `action_space` → $\mathcal A$
* `step()` → $Pr(s_{t+1}|s_t,a_t)$ und $r(s_t,a_t)$ (bei dir: der FMU-Solver)
* `terminated` = echter Terminalzustand des MDP (Bootstrapping stoppt)
* `truncated` = künstlicher Abbruch nach Zeitlimit (Bootstrapping läuft weiter!)

> Die Unterscheidung `terminated`/`truncated` ist kein API-Detail, sondern MDP-Semantik: bei `truncated` ist der Return **nicht** zu Ende, der Wert des letzten Zustands muss weitergeschätzt werden.

---
**Links:** [[Reinforcement Learning]], [[Bellman Gleichung und Dynamic Programming]], [[Temporal Difference Learning und Q-Learning]], [[Policy Gradient und Actor Critic]], [[Model Predictive Control]], [[FMU Integration in Gym Env]]
