---
title: "Generalisierung und Double Descent"
type: konzept
status: fertig
tags:
  - deep-learning
  - training
  - ai
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 8 (Measuring performance)

---

## 1. Training Error ist nicht das Ziel
Ein Modell soll nicht die Trainingsdaten reproduzieren, sondern auf **neuen** Daten funktionieren. Deshalb drei getrennte Datensätze:

| Menge | Wofür | Wie oft angeschaut |
| :--- | :--- | :--- |
| **Training** | Parameter lernen | ständig |
| **Validation** | Hyperparameter wählen, Early Stopping | regelmäßig |
| **Test** | ehrliche Endbewertung | einmal, ganz am Schluss |

## 2. Woher kommt der Fehler?
Prince zerlegt den erwarteten Testfehler in drei Anteile:

$$\text{Fehler} = \underbrace{\text{Noise}}_{\text{nicht reduzierbar}} + \underbrace{\text{Bias}}_{\text{Modell zu unflexibel}} + \underbrace{\text{Variance}}_{\text{Modell zu datenabhängig}}$$

* **Noise:** echte Zufälligkeit in den Daten (Messrauschen, fehlende Einflussgrößen). Untere Schranke, gegen die niemand ankommt.
* **Bias:** Das Modell kann die wahre Funktion strukturell nicht abbilden → **Underfitting**.
* **Variance:** Bei anderen Trainingsdaten käme ein deutlich anderes Modell heraus → **Overfitting**.

Klassisch erwartet man einen U-förmigen Testfehler über der Modellkomplexität: erst sinkt der Bias, dann steigt die Varianz.

## 3. Double Descent
Genau dieses U-Bild stimmt bei tiefen Netzen **nicht**. Empirisch beobachtet man:

1. Der Testfehler fällt (klassischer Bereich).
2. Er steigt wieder und erreicht ein Maximum genau am **Interpolationspunkt** — dort, wo das Modell gerade genug Kapazität hat, um die Trainingsdaten exakt zu treffen (Parameterzahl ≈ Datenpunkte).
3. Und dann **fällt er ein zweites Mal** — und wird oft besser als das erste Minimum.

$$\text{Testfehler}\ \searrow\ \nearrow\ \big|_{\text{Interpolation}}\ \searrow$$

**Die Konsequenz ist praktisch relevant:** Ein Netz mit *mehr* Parametern als Datenpunkten (**overparameterized**) ist nicht automatisch schlechter. "Kleiner machen gegen Overfitting" ist bei Deep Learning oft der falsche Reflex.

**Erklärungsversuch:** Im überparametrisierten Bereich gibt es unendlich viele Parametersätze, die die Trainingsdaten exakt treffen. Der Optimierer wählt nicht irgendeinen davon, sondern durch seinen **impliziten Bias** (SGD, Initialisierung) eine besonders "glatte" Lösung. Mehr Kapazität heißt: mehr Auswahl an glatten Lösungen. Siehe [[Warum Deep Learning funktioniert]].

## 4. Curse of Dimensionality
Mit steigender Inputdimension wird der Raum exponentiell leer:
* Der Abstand zwischen Datenpunkten wächst; "nächster Nachbar" verliert seine Bedeutung.
* Fast alles Volumen einer hochdimensionalen Kugel liegt an ihrer Oberfläche.

Deshalb funktioniert Deep Learning nur, weil reale Daten auf einer **niedrigdimensionalen Mannigfaltigkeit** im hochdimensionalen Raum liegen — nicht gleichverteilt darin.

## 5. Was hilft konkret?
In dieser Reihenfolge:
1. **Mehr Daten** (schlägt fast jede andere Maßnahme)
2. **Data Augmentation** / mehr Variation in der Simulation
3. Passender **induktiver Bias** in der Architektur (CNN für Bilder, [[Transformer]] für Sequenzen)
4. [[Regularisierung]]
5. Erst zum Schluss: Modellgröße reduzieren

## 6. Bezug zum RL
Im [[Reinforcement Learning]] gibt es keinen sauberen Test-Split, weil die Daten von der eigenen Policy erzeugt werden. "Generalisierung" bedeutet hier: Funktioniert die Policy auch bei **anderen Startbedingungen, Störgrößen und Parametern** als im Training?

* Auswertung immer auf **separaten Szenarien** (bei dir z. B. andere Wetterdaten, andere Geometrieparameter — siehe [[Design of Experiment]]).
* **Domain Randomization** im Training (Parameter der Simulation streuen) ist das RL-Äquivalent zur Data Augmentation und der wichtigste Hebel gegen den **Sim-to-Real-Gap**.
* Eine Policy, die nur auf genau einer Simulationskonfiguration glänzt, hat overfittet — auch wenn der Reward gut aussieht.

---
**Links:** [[Regularisierung]], [[Supervised Learning]], [[Gradient Descent und Optimierer]], [[Warum Deep Learning funktioniert]], [[Reinforcement Learning]]
