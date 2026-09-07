---
title: "Gradient Descent und Optimierer"
type: konzept
status: fertig
tags:
  - deep-learning
  - training
  - optimization
  - ai
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 6 (Fitting models)

---

## 1. Das Problem
Gesucht sind die Parameter, die den [[Loss Functions|Loss]] minimieren:

$$\hat{\boldsymbol\phi} = \arg\min_{\boldsymbol\phi} L(\boldsymbol\phi)$$

Für Neuronale Netze gibt es keine geschlossene Lösung. Die Loss-Fläche ist **nicht konvex**, hochdimensional und enthält lokale Minima, Sattelpunkte, Plateaus und schmale Täler. Man kann sie nur iterativ hinunterlaufen.

## 2. Gradient Descent
$$\boldsymbol\phi_{t+1} \leftarrow \boldsymbol\phi_t - \alpha \cdot \frac{\partial L}{\partial \boldsymbol\phi}$$

$\alpha$ ist die **Lernrate**, der wichtigste Hyperparameter überhaupt.

| $\alpha$ | Folge |
| :--- | :--- |
| zu klein | Training dauert ewig, bleibt in Plateaus hängen |
| gut | stetiger Abstieg |
| zu groß | Oszillation über das Tal hinweg, Divergenz, NaN |

**Probleme des reinen Verfahrens:**
* Es findet nur das *nächstgelegene* Minimum, das Ergebnis hängt von der Initialisierung ab.
* In schmalen Tälern (ill-conditioned, stark unterschiedliche Krümmungen) zickzackt es.
* **Sattelpunkte** sind in hohen Dimensionen viel häufiger als echte lokale Minima und bremsen den Abstieg massiv.

## 3. Stochastic Gradient Descent (SGD)
Statt den Gradienten über den **gesamten** Datensatz zu berechnen (teuer), nutzt man zufällige **Mini-Batches**:

* **Batch:** eine Teilmenge, an der ein Update gerechnet wird
* **Epoche:** ein vollständiger Durchlauf durch alle Trainingsdaten

Vorteile:
1. Viel billiger pro Update, dadurch viel mehr Updates pro Zeit.
2. Das **Rauschen** im Gradienten hilft aus flachen lokalen Minima und Sattelpunkten heraus.
3. Es wirkt als **implizite Regularisierung** und führt bevorzugt in *flache* Minima, die besser generalisieren ([[Regularisierung]], [[Generalisierung und Double Descent]]).

> SGD steigt auch mal bergauf. Jeder Batch definiert eine leicht andere Loss-Fläche.

## 4. Momentum
Ein gleitender Mittelwert der bisherigen Gradienten glättet den Zickzack:

$$\mathbf m_{t+1} = \beta\, \mathbf m_t + (1-\beta)\frac{\partial L}{\partial\boldsymbol\phi} \qquad \boldsymbol\phi_{t+1} \leftarrow \boldsymbol\phi_t - \alpha\,\mathbf m_{t+1}$$

Anschaulich: eine Kugel mit Trägheit. Sie rollt durch kleine Dellen hindurch und beschleunigt in konsistenten Richtungen. **Nesterov Momentum** berechnet den Gradienten zusätzlich schon an der *vorausberechneten* Position, also ein Blick nach vorn vor dem Schritt.

## 5. Adam
Das Problem bei einer globalen Lernrate: Verschiedene Parameter haben Gradienten völlig unterschiedlicher Größenordnung. Adam normalisiert jeden Parameter einzeln.

$$\mathbf m_{t+1} = \beta \mathbf m_t + (1-\beta)\,\mathbf g_t \qquad\qquad \mathbf v_{t+1} = \gamma \mathbf v_t + (1-\gamma)\,\mathbf g_t^2$$

Nach Bias-Korrektur ($\tilde{\mathbf m}, \tilde{\mathbf v}$, nötig weil beide bei 0 starten):

$$\boldsymbol\phi_{t+1} \leftarrow \boldsymbol\phi_t - \alpha\,\frac{\tilde{\mathbf m}_{t+1}}{\sqrt{\tilde{\mathbf v}_{t+1}} + \epsilon}$$

Effekt: Die **Schrittweite ist weitgehend unabhängig vom Gradientenbetrag**, nur die Richtung zählt. Das macht Adam robust gegen schlecht skalierte Loss-Flächen und ist der Grund, warum es fast überall der Default ist. Defaults: $\beta = 0.9$, $\gamma = 0.999$, $\epsilon = 10^{-8}$.

**AdamW** trennt Weight Decay korrekt von der Gradientenanpassung ab und ist heute die bessere Standardwahl, sobald regularisiert wird.

## 6. Lernraten-Schedules
* **Step / Exponential Decay:** Lernrate stufenweise senken.
* **Cosine Annealing:** glatt gegen null fahren, verbreitet in modernen Setups.
* **Warmup:** die ersten paar hundert Schritte mit sehr kleiner Lernrate. Wichtig bei Adam, weil die Varianzschätzung $\mathbf v$ anfangs unzuverlässig ist, und faktisch Pflicht bei [[Transformer]]n.

## 7. Praxisreihenfolge beim Debuggen
1. Kann das Netz einen **einzelnen Batch auswendig lernen** (Loss gegen 0)? Wenn nicht, ist der Fehler im Code, nicht in den Hyperparametern.
2. Lernrate über mehrere Größenordnungen scannen ($10^{-5}$ bis $10^{-1}$).
3. Erst dann Batchgröße, Architektur, [[Regularisierung]].

Loss ist NaN? Lernrate zu hoch, Inputs nicht normalisiert, oder ein $\log(0)$ im Loss.

> **Bezug RL:** Dort ist die Loss-Fläche zusätzlich **nichtstationär**, weil sich die Daten mit der Policy ändern. Deshalb sind kleine Lernraten ($3\cdot10^{-4}$ ist der de-facto-Standard in SB3) und **Gradient Clipping** (`max_grad_norm`) wichtiger als im [[Supervised Learning]].

---
**Links:** [[Backpropagation und Initialisierung]], [[Loss Functions]], [[Deep Neural Network]], [[Regularisierung]], [[Pytorch Workflow]]
