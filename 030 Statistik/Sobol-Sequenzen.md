---
title: "Sobol-Sequenzen"
type: konzept
status: fertig
tags:
  - statistik
  - sampling
  - sensitivity-analysis
  - doe
erstellt: 2026-08-11
---

# Sobol-Sequenzen

**Bezug:** [[DoE Grundlagen]] · [[Gaussian Process]] · [[200 Diplom/Arbeitsstand]]

---

## 1. Was sind Sobol-Sequenzen?

Eine Sobol-Sequenz ist eine **quasi-zufällige, deterministische** Punktfolge, die einen Parameterraum (z.B. den Würfel $[0,1]^d$) so gleichmäßig wie möglich auffüllt — im Gegensatz zu "echtem" Zufall (Pseudo-Random), der immer klumpt und Lücken lässt.

* **Kein Zufall, sondern Konstruktion:** Die Punkte werden nicht gewürfelt, sondern über eine feste mathematische Vorschrift erzeugt (daher reproduzierbar, kein Seed im klassischen Sinn nötig — nur ein Startindex/Skip).
* **Low-Discrepancy:** "Discrepancy" misst, wie ungleichmäßig eine Punktmenge einen Raum abdeckt. Sobol-Sequenzen minimieren diese Größe gezielt.
* **Space-Filling:** Für dieselbe Anzahl Punkte $N$ ist die Abdeckung des Raums deutlich gleichmäßiger als bei zufälligem Sampling — wichtig, wenn jeder Punkt eine teure Simulation kostet (siehe [[200 Diplom/Geometrische Parameter]]).

> **Kernidee:** Man will mit möglichst wenigen Simulationspunkten möglichst viel über den gesamten Parameterraum wissen. Sobol-Sequenzen sind dafür effizienter als reines Zufalls-Sampling oder ein grobes Raster.

## 2. Wie funktionieren sie (Grundprinzip)?

* Basis ist die **van-der-Corput-Folge**: eine 1D-Folge, die das Intervall $[0,1]$ durch Bit-Umkehrung einer Zählvariable sukzessive halbiert und die entstehenden Lücken auffüllt (0, 1/2, 1/4, 3/4, 1/8, 5/8, …).
* Sobol erweitert das auf $d$ Dimensionen, wobei jede Dimension eine eigene, über sogenannte **Direction Numbers** und Gray-Code-Arithmetik erzeugte Bit-Folge bekommt — so bleiben die Dimensionen näherungsweise unkorreliert zueinander, statt einfach dieselbe 1D-Folge zu kopieren.
* **Progressive Eigenschaft:** Jede Verdopplung der Punktzahl ($N \to 2N$) füllt exakt die verbliebenen Lücken der vorherigen Menge auf. Man kann also jederzeit mehr Punkte nachziehen, ohne die bisherigen zu verwerfen — praktisch, wenn man die Anzahl Simulationen im Nachhinein erhöhen will.
* In der Praxis nutzt man meist $N = 2^m$ Punkte (Zweierpotenzen), da die Gleichmäßigkeits-Eigenschaften dafür am besten bewiesen sind. Moderne Implementierungen (z.B. `scipy`) nutzen zusätzlich **Owen-Scrambling**, um die Sequenz leicht zu randomisieren und damit auch Fehlerabschätzungen (Konfidenzintervalle) zu ermöglichen.

## 3. Warum nicht einfach Zufall oder ein Raster?

| Methode | Anzahl Punkte für $d$ Parameter | Problem |
|---|---|---|
| Full-Factorial-Raster | $k^d$ (bei $k$ Stufen je Parameter) | Explodiert exponentiell ("curse of dimensionality") |
| Pseudo-Zufall (Monte Carlo) | frei wählbar | Klumpt, braucht viele Punkte für gute Abdeckung, Konvergenz nur $O(1/\sqrt{N})$ |
| Sobol-Sequenz | frei wählbar | Gleichmäßige Abdeckung, Konvergenz für Integrale bis $O(1/N)$ möglich |
| One-Factor-at-a-Time (OFAT) | $\sum_i k_i$ | Erfasst keine Interaktionen zwischen Parametern |

Für teure, deterministische Simulationen (Dymola/FMU-Sweeps, kein Messrauschen) ist das entscheidend: Man kann sich mit gleicher Rechenzeit ein viel vollständigeres Bild des Parameterraums verschaffen.

## 4. Zwei konkrete Anwendungsfälle

### a) Als Sampling-/DoE-Methode
Sobol-Sequenzen werden verwendet, um Stichprobenpunkte für ein Design of Experiments zu erzeugen — z.B. um Kombinationen mehrerer Geometrieparameter (finPitch, finThickness, TubeLength, …) gleichzeitig und gleichmäßig über den zulässigen Bereich zu verteilen, statt nur einen Parameter nach dem anderen zu variieren (OFAT). Details dazu in [[DoE Grundlagen]].

### b) Für Sobol-Sensitivitätsanalyse (globale, varianzbasierte Sensitivität)
Hier werden Sobol-Sequenzen genutzt, um die Monte-Carlo-Integrale zu schätzen, mit denen man berechnet, **wie viel der Ausgangsvarianz** (z.B. Varianz im SCOP) **durch welchen Eingangsparameter** verursacht wird.

Varianzzerlegung (Sobol-Zerlegung) der Modellausgabe $Y = f(X_1, \dots, X_d)$:

$$\text{Var}(Y) = \sum_i V_i + \sum_{i<j} V_{ij} + \dots$$

Daraus werden zwei zentrale Kennzahlen berechnet:

* **First-Order-Index** $S_i = V_i / \text{Var}(Y)$ — wie viel Varianz erklärt Parameter $i$ **allein**.
* **Total-Order-Index** $S_{T_i} = 1 - V_{\sim i}/\text{Var}(Y)$ — wie viel Varianz erklärt Parameter $i$ **inklusive aller Interaktionen** mit anderen Parametern.

Ist $S_{T_i} \gg S_i$, hat der Parameter starke Wechselwirkungen mit anderen — genau das, was ein reiner OFAT-Sweep nie zeigen kann.

## 5. Praktisches Vorgehen in Python

```python
from scipy.stats.qmc import Sobol

sampler = Sobol(d=3, scramble=True, seed=42)   # d = Anzahl Parameter
punkte = sampler.random_base2(m=5)             # 2^5 = 32 Punkte in [0,1]^d

# anschließend auf echte Parametergrenzen skalieren, z.B.:
from scipy.stats.qmc import scale
grenzen_min = [1.0, 0.15e-3, 0.4]   # finPitch, finThickness, TubeLength ...
grenzen_max = [7.0, 0.30e-3, 0.8]
skaliert = scale(punkte, grenzen_min, grenzen_max)
```

Für eine vollständige Sensitivitätsanalyse-Pipeline (inkl. korrektem Sampling-Schema nach Saltelli) eignet sich die Bibliothek **SALib** (`SALib.sample.sobol`, `SALib.analyze.sobol`) — sie übernimmt automatisch, dass für Sobol-Indizes ein bestimmtes, größeres Sample-Schema nötig ist als für reines Space-Filling-Sampling.

## 6. Bezug zur Diplomarbeit

Aktuell wird der Rippenabstand einzeln variiert ([[200 Diplom/Design of Experiment]]), alle anderen Mikro-Parameter bleiben fix. Für die geplante Zero-Shot-Transfer-Auswertung (mehrere Mikro-Parameter gleichzeitig, siehe [[200 Diplom/Arbeitsstand]] Abschnitt 2.3) wäre ein Sobol-Sample über alle relevanten Mikro-Parameter gemeinsam sinnvoll — das liefert automatisch auch die Datenbasis für ein [[Gaussian Process]]-Surrogatmodell und eine echte Sensitivitätsanalyse statt nur einer visuellen "Baseline gut / außerhalb schlecht"-Einschätzung.
