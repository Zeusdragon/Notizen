---
title: "Loss Functions"
type: konzept
status: fertig
tags:
  - deep-learning
  - training
  - ai
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 5 (Loss functions)

---

## 1. Die zentrale Idee: Loss ist kein Bauchgefühl
Der wichtigste Gedanke des Kapitels: Man sucht sich eine Loss-Funktion **nicht aus** — man leitet sie her. Das Netz gibt nicht direkt $y$ aus, sondern die **Parameter einer Wahrscheinlichkeitsverteilung** über $y$:

$$Pr(y \mid \boldsymbol\theta), \qquad \boldsymbol\theta = f(x, \boldsymbol\phi)$$

Der Loss ist dann immer derselbe: die **negative Log-Likelihood** der Trainingsdaten.

## 2. Das Rezept (Maximum Likelihood)
1. Wähle eine Verteilung $Pr(y|\boldsymbol\theta)$, die zum Wertebereich von $y$ passt.
2. Lass das Netz $\boldsymbol\theta$ vorhersagen.
3. Maximiere die Likelihood über alle Trainingsbeispiele:
$$\hat{\boldsymbol\phi} = \arg\max_{\boldsymbol\phi} \prod_{i} Pr(y_i \mid f(x_i,\boldsymbol\phi))$$
4. Produkte vieler kleiner Zahlen sind numerisch tot, also logarithmieren und Vorzeichen drehen:
$$L(\boldsymbol\phi) = -\sum_{i} \log Pr\big(y_i \mid f(x_i,\boldsymbol\phi)\big)$$
5. Zur **Inferenz** gibt man den wahrscheinlichsten Wert aus: $\hat y = \arg\max_y Pr(y|f(x,\hat{\boldsymbol\phi}))$.

**Nebeneffekt:** Man bekommt automatisch ein Maß für die **Unsicherheit** der Vorhersage mitgeliefert, nicht nur einen Punktwert.

## 3. Die Standardfälle
### Regression -> Least Squares
Annahme: Normalverteilung mit fester Varianz, Netz sagt den Mittelwert $\mu$ voraus.

$$L = \sum_i (f(x_i,\boldsymbol\phi) - y_i)^2$$

Der **Mean Squared Error ist also keine willkürliche Wahl**, sondern die Konsequenz aus "normalverteiltes Rauschen mit konstanter Varianz".

* Ist die Varianz *nicht* konstant, lässt man das Netz auch $\sigma^2$ vorhersagen (**heteroskedastische Regression**) mit $\sigma^2 = \exp(\cdot)$ oder Softplus, damit sie positiv bleibt.
* MSE reagiert empfindlich auf Ausreißer (quadratisch). Alternativen: **MAE / L1** (entspricht Laplace-Verteilung) oder **Huber Loss** (quadratisch nahe null, linear außen).

### Binäre Klassifikation -> Binary Cross-Entropy
Bernoulli-Verteilung; das Netz gibt einen reellen Wert (**Logit**) aus, den eine Sigmoid-Funktion auf $[0,1]$ abbildet:

$$\lambda = \text{sig}(f(x,\boldsymbol\phi)) = \frac{1}{1+e^{-f}}$$
$$L = -\sum_i \Big[ y_i \log \lambda_i + (1-y_i)\log(1-\lambda_i)\Big]$$

### Multiclass -> Cross-Entropy mit Softmax
Kategorische Verteilung; das Netz gibt $K$ Logits aus:

$$\lambda_k = \frac{\exp(f_k)}{\sum_{j=1}^{K}\exp(f_j)} \qquad L = -\sum_i \log \lambda_{y_i}$$

### Weitere
| Datentyp | Verteilung | Output-Aktivierung |
| :--- | :--- | :--- |
| Zähldaten $y \in \{0,1,2,\dots\}$ | Poisson | $\exp$ |
| Richtung / Winkel | von Mises | — |
| Multimodal (mehrere gültige Antworten) | **Mixture of Gaussians** | Mischgewichte per Softmax |

Der letzte Punkt ist praktisch wichtig: Wenn es mehrere richtige Antworten gibt, mittelt ein MSE-Modell sie zu einer falschen Antwort dazwischen.

## 4. Cross-Entropy-Sichtweise
Dieselbe Formel lässt sich als Minimierung der **KL-Divergenz** zwischen empirischer Datenverteilung und Modellverteilung lesen. Maximum Likelihood und Cross-Entropy-Minimierung sind identisch, nur zwei Erzählungen für dieselbe Gleichung.

## 5. Fallstricke in der Praxis
* **Logits, nicht Wahrscheinlichkeiten:** `nn.CrossEntropyLoss` und `nn.BCEWithLogitsLoss` enthalten Softmax/Sigmoid bereits. Wer vorher selbst ein Softmax anwendet, trainiert falsch und instabil.
* **Numerische Stabilität:** immer die WithLogits-Varianten nehmen (log-sum-exp-Trick).
* **Skalierung der Targets:** Bei MSE dominieren Outputs mit großem Wertebereich den Loss. Normalisieren.
* **Klassenungleichgewicht:** Gewichte (`pos_weight`, `class_weight`) oder Focal Loss.

## 6. Bezug zum RL
Im [[Reinforcement Learning]] taucht dasselbe Muster auf:
* Der **Critic** wird per MSE gegen das TD-Target trainiert, also reine Regression ([[Temporal Difference Learning und Q-Learning]]).
* Der **Actor** einer diskreten Policy gibt Logits aus, aus denen ein Softmax die Aktionsverteilung macht; der Policy-Gradient-Term ist ein gewichteter Log-Likelihood ([[Policy Gradient und Actor Critic]]).
* Bei kontinuierlichen Aktionen gibt der Actor $\mu$ und $\log\sigma$ einer Normalverteilung aus, exakt die heteroskedastische Regression von oben.

---
**Links:** [[Supervised Learning]], [[Gradient Descent und Optimierer]], [[Deep Neural Network]], [[Pytorch Workflow]]
