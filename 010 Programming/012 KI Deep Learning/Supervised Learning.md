---
title: "Supervised Learning"
type: konzept
status: fertig
tags:
  - deep-learning
  - machine-learning
  - ai
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 2 (Supervised learning)

---

## 1. Das Grundrezept
Supervised Learning heißt: aus Paaren $\{x_i, y_i\}$ eine Abbildung $x \rightarrow y$ lernen. Prince zerlegt jedes Modell in **drei** Bestandteile — und dieses Schema gilt vom linearen Regressor bis zum [[Transformer]]:

1. **Modell** $y = f(x, \boldsymbol\phi)$ — feste Struktur, freie Parameter $\boldsymbol\phi$
2. **Loss** $L(\boldsymbol\phi)$ — eine Zahl, die sagt, wie schlecht das Modell die Trainingsdaten beschreibt
3. **Training** — finde $\hat{\boldsymbol\phi} = \arg\min_{\boldsymbol\phi} L(\boldsymbol\phi)$

Danach kommt noch der vierte, oft unterschätzte Schritt: **Testen** auf Daten, die das Modell nie gesehen hat.

## 2. Das 1D-Beispiel
Lineare Regression mit zwei Parametern:

$$y = f(x,\boldsymbol\phi) = \phi_0 + \phi_1 x$$

Least-Squares-Loss:

$$L(\boldsymbol\phi) = \sum_{i=1}^{I} \big(f(x_i,\boldsymbol\phi) - y_i\big)^2$$

Die **Loss-Fläche** über $(\phi_0,\phi_1)$ ist hier eine konvexe Schüssel mit genau einem Minimum. Bei Neuronalen Netzen ist sie hochdimensional, nicht konvex und voller Sattelpunkte — deshalb braucht es [[Gradient Descent und Optimierer|iterative Optimierung]] statt einer geschlossenen Lösung.

## 3. Begriffe, die durchgehend gelten
| Begriff | Bedeutung |
| :--- | :--- |
| **Parameter** $\boldsymbol\phi$ | werden *gelernt* (Weights, Biases) |
| **Hyperparameter** | werden *gesetzt* (Lernrate, Netztiefe, $\gamma$ im RL) |
| **Trainingsdaten** | zum Anpassen der Parameter |
| **Validierungsdaten** | zum Wählen der Hyperparameter |
| **Testdaten** | einmalig, zum ehrlichen Schätzen der Generalisierung |
| **Inference** | Anwendung des trainierten Modells |

> Wer Hyperparameter am Testset optimiert, hat kein Testset mehr — siehe [[Generalisierung und Double Descent]].

## 4. Regression vs. Klassifikation
Der Unterschied liegt allein im Output und damit im Loss ([[Loss Functions]]):

* **Regression:** $y \in \mathbb R$ → Least Squares
* **Binäre Klassifikation:** $y \in \{0,1\}$ → Sigmoid + Binary Cross-Entropy
* **Multiclass:** $y \in \{1..K\}$ → Softmax + Cross-Entropy
* **Strukturierte Outputs:** Bild, Text, Segmentierungsmaske — das Prinzip bleibt identisch, nur die Wahrscheinlichkeitsverteilung wird komplexer.

## 5. Abgrenzung
| Paradigma | Daten | Beispiel |
| :--- | :--- | :--- |
| **Supervised** | $(x, y)$ mit Labels | Regression, Klassifikation |
| **Unsupervised** | nur $x$ | Clustering, [[Generative Modelle]] |
| **Self-supervised** | $x$, Labels aus $x$ konstruiert | Wortvorhersage, Masked Modeling |
| **[[Reinforcement Learning]]** | Interaktion + Reward | Regelung, Spiele |

Der entscheidende Unterschied zum RL: Hier gibt es zu jedem Input die *richtige Antwort*. Im RL gibt es nur ein Signal, ob es besser oder schlechter war — und das oft erst viel später.

---
**Links:** [[Shallow Neural Network]], [[Deep Neural Network]], [[Loss Functions]], [[Generalisierung und Double Descent]], [[Pytorch Workflow]]
