# Projektstand: RL-Agent für prädiktives Abtauen (Diplomarbeit)

## 1. Aktueller Stand: Was bisher umgesetzt wurde

### 1.1 Simulationsumgebung & Architektur (Digitaler Prüfstand)
* **Basis-Simulation:** Dymola (TIL-Suite). Die Wärmepumpe wird als FMU exportiert. 
* **Regelungstechnik der Anlage:** * Die Leistungsregelung erfolgt numerisch robust über das Hubvolumen (Displacement), welches dauerhaft fest auf 0,5 gesetzt ist. 
  * Das EXV wird im Heizmodus über einen PI-Regler auf eine Unterkühlung von 2 K (Sollwert) eingeregelt und somit über den effektiven Querschnitt gesteuert.
* **RL-Environment:** Custom `HeatPumpEnv` (Gymnasium).
* **Wrapper & Skalierung:**
  * `SubprocVecEnv`: Multiprocessing mit 16 CPU-Kernen für massiv paralleles Training.
  * `VecFrameStack` (n=36): Der Agent sieht die letzten 36 Schritte (entspricht 1 Stunde).
  * `VecNormalize`: Laufende Standardisierung der Observationen und Rewards (verhindert instabile Gradienten). Ein Custom Callback speichert die Stats regelmäßig ab.

### 1.2 Wetterdaten & Datenpipeline
* **Datensatz:** Frankfurt a.M., historische Daten. (2010–2020 für das Training). 
* **Validierungsdaten:** Das Jahr 2021 (Überlegung: potenziell auf ein bis zwei repräsentative Monate reduzieren, um die Evaluierungszeiten zu verkürzen).
* **Filterung:** Die Sommermonate (April bis September) sind herausgefiltert, um den Agenten nicht mit irrelevanten Leerlauf-Daten zu trainieren.
* **Zeitschrittweite & Interpolation:** * Aufgelöst auf 100 Sekunden pro Schritt, linear interpoliert.
  * *Methodische Entscheidung:* Spline- oder Polynom-Interpolationen standen zur Debatte, wurden jedoch verworfen. Grund: Die in Dymola verbauten First-Order-Blöcke (PT1-Glieder) glätten die Input-Signale bereits ausreichend ab und thermodynamische Wettereinflüsse sind ohnehin sehr träge.
* **Alternative Datenquellen:** Standardisierte TRY/TMY-Daten (Test Reference Years) anstatt harter historischer Jahre.

### 1.3 Reinforcement Learning: Algorithmus & Reward
* **Aktueller Algorithmus:** QR-DQN (aus sb3-contrib). 
  * *Warum QR-DQN?* Als distributionsbasierter Q-Learning-Algorithmus ist er robuster gegenüber dem starken Rauschen und der zeitlichen Verzögerung des thermodynamischen Systems als ein klassisches DQN.
* **Alternativen:** PPO (oft stabiler, aber sehr daten-ineffizient), Standard-DQN.
* **Hyperparameter-Tuning:** Vollautomatisiert via Optuna. 
  * *Wichtige Erkenntnis:* Der Fokus muss auf den Explorations-Parametern (`exploration_fraction > 0.4`) liegen. Da das Eiswachstum sehr langsam ist, muss der Agent zwingend lange im "Chaos-Modus" verweilen, um zu lernen, dass Abtauen langfristig Sinn ergibt.
* **Belohnungsfunktion (Reward):** Puristischer Ansatz ohne manuelles "Reward Shaping" auf die reine Eismasse. Die Bewertung erfolgt rein über den relativen Gütegrad ($COP_{aktuell} / COP_{Carnot}$).

### 1.4 Geometrische Sensitivitätsanalyse (Dymola Sweep)
* **Untersuchte Parameter:** finPitch, finThickness, TubeLength, nSerialTubes, nParalelltubes, parallelTubeDistance, SerialtubeDistance.
* **Erkenntnis:** Eine konzeptionelle Trennung in Makro- und Mikro-Parameter ist zwingend erforderlich, um physikalisch sinnvolle Vergleiche ziehen zu können:
  * *Makro (Bauraum/Leistung):* Rohrlängen und Reihenanzahl werden fixiert, um die Nennleistung der Anlage nicht zu verfälschen.
  * *Mikro (Reifdynamik):* Rippenabstand etc. werden für das Zero-Shot-Testing variiert, da sie primär die Blockage-Ratio und den luftseitigen Druckverlust beeinflussen.

---

## 2. Offene Baustellen & Nächste Schritte

### 2.1 Code & Architektur-Feinschliff
* **Akademische Reproduzierbarkeit:** Feste Random-Seeds in `main.py` und `optuna_train.py` (PyTorch, Numpy, Gym) erzwingen.
* **Custom TensorBoard Logging:** Den Monitor-Wrapper erweitern, um physikalische Metriken (z. B. `current_cop`, prozentualer Abtauanteil) für die Thesis-Graphen direkt loggen zu lassen, anstatt sich nur auf abstrakte Rewards zu verlassen.

### 2.2 Das Master-Training
* **Optuna-Auswertung:** Die `best_params.json` des aktuellen Laufs analysieren (Besonderes Augenmerk auf `exploration_fraction` und `initial_eps`).
* **Plan A (Puristisches Training):** Den Sieger-Agenten mit reinem COP-Reward und der virtuellen "Todeszone" trainieren.
* **Plan B (Reward Shaping):** *Nur falls Plan A fehlschlägt.* Die Belohnungsfunktion um den Faktor $1 / (1 + m_{ice})$ erweitern, um das Netz über einen "Dense Reward" schneller in die richtige Richtung zu lenken.

### 2.3 Evaluierung & Zero-Shot-Transfer (Kapitel 5 & 6)
* **Design of Experiments (DoE):** Ein systematisches Screening der Mikro-Parameter (Rippenabstand etc.) mittels Dymola Sweep durchführen.
* **Archetypen-Export:** Genau 3 repräsentative FMUs exportieren (Baseline, Frost-Falle, Dauerläufer).
* **Auswertungs-Metriken:** Die Evaluierung der Performance erfolgt zwingend über folgende Kernparameter:
  1. Anzahl der Abtauzyklen ($n$)
  2. Absolute Abtauzeit
  3. Prozentualer Abtauanteil
  4. Durchschnittlicher SCOP (Seasonal Coefficient of Performance)
* **Automatisierungs-Skript:** Ein `run_robustness_analysis.py` schreiben, das den fertigen RL-Agenten und die klassischen Heuristiken vollautomatisiert durch alle 3 FMUs iteriert.
* **Methodische Fragestellung zu Basisreglern:**
  * *Ansatz 1 (Starr):* Die Basisregler bleiben exakt auf die Baseline-Geometrie kalibriert. So wird demonstriert, was passiert, wenn sie "blind" auf neue Geometrien losgelassen werden.
  * *Ansatz 2 (Skaliert):* Die Basisregler werden für die neuen Geometrien "fair" nachkalibriert. 
    * Bei der *bedarfsgesteuerten Abtauung* sollte dies theoretisch intrinsisch passieren, da sie auf Drücke/Temperaturen reagiert.
    * Bei der *Zeitabtauung* müsste der Auslösezeitpunkt an die neue Geometrie angepasst werden (indem man vorher simuliert, ab wann der COP bei der jeweiligen Variante einbricht).