---
title: "Regularisierung"
type: konzept
status: fertig
tags:
  - deep-learning
  - training
  - ai
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 9 (Regularization)

---

## 1. Definition
Regularisierung ist **jede Maßnahme, die den Testfehler senkt, ohne den Trainingsfehler senken zu wollen**. Man verschiebt bewusst die Balance von Varianz zu Bias ([[Generalisierung und Double Descent]]) oder schränkt ein, welche Lösungen der Optimierer überhaupt finden kann.

## 2. Explizite Regularisierung
Ein Zusatzterm im [[Loss Functions|Loss]] bestraft unerwünschte Parameter:

$$\hat{\boldsymbol\phi} = \arg\min_{\boldsymbol\phi}\Big[ L(\boldsymbol\phi) + \lambda\, g(\boldsymbol\phi) \Big]$$

* **L2 / Weight Decay:** $g = \sum \phi_j^2$. Zieht Gewichte gegen null, bevorzugt glatte Funktionen. Aus Bayes-Sicht: ein Normalverteilungs-Prior auf den Gewichten.
* **L1 / Lasso:** $g = \sum |\phi_j|$. Erzeugt *sparse* Lösungen (echte Nullen).
* $\lambda$ steuert die Stärke und ist ein Hyperparameter für das Validierungsset.

> Bei Adam ist "L2 im Loss" nicht dasselbe wie Weight Decay, weil Adam den Gradienten normalisiert. Deshalb **AdamW** verwenden, wenn regularisiert wird.

## 3. Implizite Regularisierung
Der große Aha-Punkt des Kapitels: Auch ohne Zusatzterm ist das Training regularisiert.

* **Gradient Descent** folgt (analytisch zeigbar) einem leicht veränderten Loss, der Lösungen mit kleiner Gradientennorm bevorzugt.
* **SGD** fügt Rauschen hinzu und landet bevorzugt in **flachen Minima**. Flache Minima sind robuster gegen kleine Parameterstörungen und generalisieren empirisch besser.
* Größere Lernrate und kleinere Batchgröße verstärken diesen Effekt.

Das erklärt, warum überparametrisierte Netze trotzdem nicht wild überanpassen.

## 4. Die praktische Werkzeugkiste
| Verfahren | Mechanismus |
| :--- | :--- |
| **Early Stopping** | Training stoppen, wenn der Validierungsfehler steigt. Begrenzt effektiv, wie weit sich die Parameter von der Initialisierung entfernen |
| **Dropout** | Im Training zufällig einen Anteil der hidden units auf null setzen. Verhindert, dass sich units auf einzelne Partner verlassen (Co-Adaptation). Wirkt wie ein Ensemble über exponentiell viele Subnetze |
| **Ensembling** | Mehrere Modelle mitteln. Senkt die Varianz fast garantiert, kostet aber n-fache Rechenzeit |
| **Noise hinzufügen** | Rauschen auf Inputs, Gewichte oder Labels (**Label Smoothing**) |
| **Data Augmentation** | Rotationen, Crops, Rauschen. Kodiert bekannte Invarianzen direkt in die Daten |
| **Batch Normalization** | Das Batch-Rauschen wirkt nebenbei regularisierend ([[Residual Networks und Batch Normalization]]) |
| **Transfer Learning** | Auf einer großen verwandten Aufgabe vortrainieren, dann feintunen. Die vortrainierten Gewichte sind ein starker Prior |
| **Multi-Task Learning** | Mehrere Ziele gleichzeitig lernen zwingt zu allgemeineren Repräsentationen |
| **Self-supervised Learning** | Labels aus den Daten selbst konstruieren, um unbeschriftete Daten zu nutzen |

**Dropout im Detail:** Zur Inferenz wird nicht mehr gedroppt; stattdessen werden die Aktivierungen skaliert (PyTorch macht das über `model.eval()` automatisch). Der häufigste Fehler ist ein vergessenes `model.eval()` bei der Auswertung.

## 5. Was wann nehmen?
1. Mehr/vielfältigere Daten schlägt alles.
2. Ein passender **induktiver Bias** in der Architektur ist die stärkste Form der Regularisierung — ein CNN ist ein MLP mit erzwungener Translationsinvarianz.
3. Weight Decay ist billig und praktisch immer sinnvoll.
4. Dropout eher in großen MLPs; in modernen CNNs meist durch Normalisierung ersetzt.
5. Early Stopping praktisch immer.

## 6. Bezug zum RL
Regularisierung sieht im [[Reinforcement Learning]] anders aus, weil das Problem nicht Overfitting an einen festen Datensatz ist, sondern Instabilität:

* **Entropie-Bonus** hält die Policy stochastisch und verhindert vorzeitiges Kollabieren ([[Policy Gradient und Actor Critic]]).
* **PPO-Clipping / KL-Penalty** regularisieren im Verhaltensraum statt im Parameterraum.
* **Target Networks** und Polyak-Averaging stabilisieren die Value-Schätzung ([[Temporal Difference Learning und Q-Learning]]).
* **Domain Randomization** ist die Data Augmentation des RL.
* Dropout wird im RL meist **nicht** verwendet: es verrauscht die Value-Schätzung zusätzlich und stört das Bootstrapping.

---
**Links:** [[Generalisierung und Double Descent]], [[Gradient Descent und Optimierer]], [[Loss Functions]], [[Deep Neural Network]], [[Pytorch Workflow]]
