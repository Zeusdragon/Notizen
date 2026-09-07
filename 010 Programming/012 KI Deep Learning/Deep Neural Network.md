---
title: "Deep Neural Network"
type: konzept
status: fertig
tags:
  - deep-learning
  - neural-networks
  - ai
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 4 (Deep neural networks)

---

## 1. Von flach zu tief
Ein [[Shallow Neural Network]] hat genau eine Hidden Layer. Ein **Deep Neural Network** verkettet mehrere: die hidden units einer Schicht sind der Input der nächsten.

$$\mathbf h_1 = a(\boldsymbol\beta_0 + \boldsymbol\Omega_0 \mathbf x)$$
$$\mathbf h_2 = a(\boldsymbol\beta_1 + \boldsymbol\Omega_1 \mathbf h_1)$$
$$\vdots$$
$$\mathbf y = \boldsymbol\beta_K + \boldsymbol\Omega_K \mathbf h_K$$

Kompakt als Komposition:

$$\mathbf y = \boldsymbol\beta_K + \boldsymbol\Omega_K\, a\big(\boldsymbol\beta_{K-1} + \boldsymbol\Omega_{K-1}\, a(\cdots a(\boldsymbol\beta_0 + \boldsymbol\Omega_0\mathbf x)\cdots)\big)$$

Wichtig: Ohne die Aktivierungsfunktion $a(\cdot)$ zwischen den Schichten wäre das Ganze wieder nur eine einzige lineare Abbildung — Tiefe brächte gar nichts.

### Notation
| Symbol | Bedeutung |
| :--- | :--- |
| $K$ | Anzahl Hidden Layers = **Tiefe (depth)** |
| $D_k$ | Anzahl units in Layer $k$ = **Breite (width)** |
| $\boldsymbol\Omega_k$ | Gewichtsmatrix von Layer $k$ nach $k+1$ |
| $\boldsymbol\beta_k$ | Bias-Vektor |
| **Kapazität** | Gesamtzahl der hidden units |

Ein Netz mit $K$ Layers hat $K+1$ Gewichtsmatrizen. Netze dieser Bauart heißen **Multi-Layer Perceptron (MLP)** oder *fully connected network*.

## 2. Was Tiefe geometrisch bewirkt: Falten
Die anschaulichste Erklärung im Buch: Jede Schicht **faltet** den Inputraum. Bereiche, die durch ReLU auf dieselben Aktivierungen abgebildet werden, werden "übereinandergelegt". Die nächste Schicht bearbeitet dann alle gefalteten Kopien gleichzeitig — eine Knickstelle in Layer 2 erzeugt dadurch *mehrere* Knicke im ursprünglichen Inputraum.

Das Ergebnis bleibt eine **stückweise lineare Funktion**, aber die Zahl der linearen Regionen wächst dramatisch anders:

| | flach | tief |
| :--- | :--- | :--- |
| Regionen | polynomiell in der Breite | **exponentiell in der Tiefe** |

Beispiel aus dem Buch: Ein Netz mit 5 Layers à 10 units (471 Parameter) erzeugt über 160.000 lineare Regionen. Ein flaches Netz mit derselben Parameterzahl schafft nur einen Bruchteil davon.

## 3. Warum dann nicht immer tiefer?
Prince betont ehrlich, dass die Antwort nicht abschließend geklärt ist ([[Warum Deep Learning funktioniert]]):

* Das **Universal Approximation Theorem** gilt schon für flache Netze — Ausdrucksstärke *allein* erklärt den Vorteil nicht.
* Die exponentiell vielen Regionen sind nicht unabhängig; ihre Form ist durch die Faltung gekoppelt. Das ist ein **Induktiver Bias**: gut für Daten mit hierarchischer Struktur (Bilder, Sprache, Physik), nicht automatisch gut für alles.
* Empirisch: Tiefe Netze **trainieren besser** und **generalisieren besser** — bei gleicher Parameterzahl.
* Es gibt Grenzen: Ab einer bestimmten Tiefe wird das Training instabil ([[Backpropagation und Initialisierung|vanishing/exploding gradients]]) — deshalb [[Residual Networks und Batch Normalization|Residual Connections]].

## 4. Depth Efficiency
Es existieren Funktionen, die ein tiefes Netz mit wenigen units darstellen kann, für die ein flaches Netz **exponentiell viele** units bräuchte. Die Umkehrung gilt nicht. Tiefe ist also nie schlechter, aber oft dramatisch billiger.

## 5. In PyTorch
```python
import torch.nn as nn

model = nn.Sequential(
    nn.Linear(D_in, 64), nn.ReLU(),   # Layer 1
    nn.Linear(64, 64),   nn.ReLU(),   # Layer 2
    nn.Linear(64, 64),   nn.ReLU(),   # Layer 3
    nn.Linear(64, D_out),             # Output (keine Aktivierung!)
)
```

Die Output-Schicht bleibt linear — die passende Nichtlinearität (Sigmoid, Softmax) steckt beim Training im [[Loss Functions|Loss]].

> **Bezug RL:** Die `net_arch=[64, 64]`-Angabe in Stable Baselines 3 beschreibt genau so ein MLP. Für Regelungsaufgaben mit wenigen Sensorwerten sind zwei bis drei Schichten fast immer ausreichend — die Datenmenge, nicht die Netzgröße, ist dort der Engpass.

---
**Links:** [[Shallow Neural Network]], [[Backpropagation und Initialisierung]], [[Gradient Descent und Optimierer]], [[Residual Networks und Batch Normalization]], [[Pytorch Workflow]]
