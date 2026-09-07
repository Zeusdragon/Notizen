---
title: "Shallow Neural Network"
type: konzept
status: fertig
tags:
  - deep-learning
  - neural-networks
  - ai
erstellt: 2026-01-05
aktualisiert: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 3 (Shallow Neural Networks)

---

## 1. Grundidee
Wir mappen einen Input $x$ über eine Funktion $y = f(x, \boldsymbol\phi)$ auf einen Output $y$. Die Funktion hat **feste Struktur**, aber **freie Parameter** $\boldsymbol\phi$. Das Training sucht die Parameter, die die Trainingsdaten am besten beschreiben (Loss minimieren).

Der Aufbau besteht immer aus drei Schritten:

1. **Lineare Terme** bilden (pre-activations)
2. Jeden Term durch eine **Aktivierungsfunktion** schicken (activations / hidden units)
3. Die Ergebnisse **gewichtet aufsummieren** (+ Bias) → Output

Genau diese Reihenfolge ist der Grund, warum ein NN mehr kann als lineare Regression: ohne Schritt 2 wäre die Verkettung zweier linearer Abbildungen wieder nur linear.

## 2. Beispiel: 1 Input, 1 Output, 3 Hidden Units
Prince' Einstiegsbeispiel mit 10 Parametern:

$$y = \phi_0 + \phi_1 \, a(\theta_{10} + \theta_{11}x) + \phi_2 \, a(\theta_{20} + \theta_{21}x) + \phi_3 \, a(\theta_{30} + \theta_{31}x)$$

Zerlegt in zwei Schritte:

$$h_d = a(\theta_{d0} + \theta_{d1}x) \qquad d = 1,2,3$$
$$y = \phi_0 + \phi_1 h_1 + \phi_2 h_2 + \phi_3 h_3$$

Die $h_d$ heißen **hidden units** — sie stehen zwischen Input und Output und werden nie direkt beobachtet.

> **Achtung Notation:** Die $\theta$ gehören zur *ersten* Schicht (Input → Hidden), die $\phi$ zur *zweiten* Schicht (Hidden → Output). In deiner alten Notiz stand $y = \phi_0 + \sum \theta_{d0} h_d$ — die Gewichte der Ausgabeschicht sind aber $\phi_d$, nicht $\theta_{d0}$.

### Was passiert geometrisch?
Mit ReLU als Aktivierung entsteht eine **stückweise lineare Funktion** (piecewise linear):

* Jede hidden unit ist "aus" (Ausgabe 0), solange $\theta_{d0} + \theta_{d1}x < 0$, und "an" (linear) sonst.
* Der Umschaltpunkt (**joint** / knot) liegt bei
  $$x = -\frac{\theta_{d0}}{\theta_{d1}}$$
* $\theta_{d1}$ bestimmt die **Steigung** des aktiven Astes, $\phi_d$ **skaliert und spiegelt** ihn, $\phi_0$ verschiebt die ganze Kurve vertikal.

Mit $D$ hidden units bekommt man also $D$ Knicke und $D+1$ lineare Abschnitte. Mehr hidden units = feinere Approximation.

## 3. Universal Approximation Theorem
Wenn man die Zahl der hidden units $D$ beliebig groß macht, kann ein Netz mit **einer einzigen** Hidden Layer jede stetige Funktion auf einem kompakten Bereich beliebig genau approximieren.

Wichtig zur Einordnung:
* Das Theorem sagt, dass die Parameter **existieren** — nicht, dass man sie findet (Training ist ein separates Problem).
* Es sagt nichts über die **benötigte Breite**. Für viele Funktionen bräuchte ein flaches Netz absurd viele units, während ein tiefes Netz mit wenigen units auskommt → Motivation für [[Deep Neural Network]].

## 4. Mehrere In- und Outputs
### Mehrere Inputs ($D_i > 1$)
Jede hidden unit bekommt eine gewichtete Summe aller Inputs:

$$h_d = a\!\left(\theta_{d0} + \sum_{i=1}^{D_i} \theta_{di} x_i\right)$$

Geometrisch: Der "Knick" ist jetzt keine Stelle auf der x-Achse mehr, sondern eine **Hyperebene** im Inputraum. Bei 2D-Input ist das eine Gerade, die die Ebene in "unit aktiv" / "unit inaktiv" teilt. Die Überlagerung mehrerer solcher Geraden zerlegt den Inputraum in **konvexe polygonale Regionen**, in denen die Funktion jeweils linear ist.

### Mehrere Outputs ($D_o > 1$)
Alle Outputs teilen sich **dieselben** hidden units, nur mit eigenen Gewichten:

$$y_j = \phi_{j0} + \sum_{d=1}^{D} \phi_{jd} h_d$$

Die Knickstellen liegen deshalb bei allen Outputs an denselben Positionen — nur die Steigungen unterscheiden sich.

## 5. Allgemeine Form & Matrixschreibweise
Netz mit $D_i$ Inputs, $D$ hidden units, $D_o$ Outputs:

$$\mathbf{h} = a(\boldsymbol\Theta \mathbf{x} + \boldsymbol\theta_0) \qquad \mathbf{y} = \boldsymbol\Phi \mathbf{h} + \boldsymbol\phi_0$$

| Symbol | Dimension | Bedeutung |
| :--- | :--- | :--- |
| $\mathbf{x}$ | $D_i$ | Input-Vektor |
| $\boldsymbol\Theta$ | $D \times D_i$ | Gewichte Layer 1 (weights) |
| $\boldsymbol\theta_0$ | $D$ | Bias Layer 1 |
| $\mathbf{h}$ | $D$ | Hidden Units (activations) |
| $\boldsymbol\Phi$ | $D_o \times D$ | Gewichte Layer 2 |
| $\boldsymbol\phi_0$ | $D_o$ | Bias Layer 2 |

**Parameteranzahl:** $(D_i + 1)\cdot D + (D + 1)\cdot D_o$

### Begriffe
* **Pre-activation:** der lineare Term *vor* der Aktivierung ($\boldsymbol\Theta\mathbf{x} + \boldsymbol\theta_0$)
* **Activation:** der Wert *nach* der Aktivierung ($\mathbf{h}$)
* **Weights / Biases:** Multiplikatoren bzw. additive Offsets
* **Fully connected:** jede unit ist mit jeder unit der Nachbarschicht verbunden
* **Feed-forward:** keine Rückkopplungen, Information fließt nur vorwärts
* **Shallow:** genau *eine* Hidden Layer — sonst spricht man von deep

### Anzahl linearer Regionen
Ein flaches Netz mit $D$ hidden units und $D_i$ Inputs erzeugt maximal (Zaslavsky):

$$\sum_{j=0}^{D_i} \binom{D}{j}$$

lineare Regionen. Bei festem $D_i$ wächst das nur **polynomiell** in $D$ — tiefe Netze schaffen exponentielles Wachstum bei gleicher Parameterzahl.

## 6. Aktivierungsfunktionen
Die Aktivierungsfunktion entscheidet, *ab wann* und *wie stark* ein Term $\theta_{d0} + \theta_{d1}x$ durchgelassen wird. Sie muss nichtlinear sein.

| Funktion | Formel | Anmerkung |
| :--- | :--- | :--- |
| **ReLU** | $a(z) = \max(0, z)$ | Standard seit ~2010, billig, kein Sättigungsproblem im positiven Ast |
| **Leaky ReLU** | $\max(\alpha z, z)$, $\alpha \approx 0.1$ | negativer Ast bleibt lebendig |
| **Parametric ReLU** | wie Leaky, aber $\alpha$ wird **gelernt** | |
| **Sigmoid** | $\frac{1}{1+e^{-z}}$ | frühe NN, sättigt an beiden Enden |
| **Tanh** | $\tanh(z)$ | zentriert um 0, sättigt ebenfalls |
| **Heaviside** | $0$ für $z<0$, sonst $1$ | Stufenfunktion, nicht differenzierbar |
| **Softplus** | $\ln(1+e^{z})$ | glatte ReLU-Näherung |
| **ELU** | $z$ bzw. $\alpha(e^{z}-1)$ | glatt, negative Werte möglich |
| **SiLU / Swish** | $z \cdot \text{sigmoid}(z)$ | glatt, in modernen Netzen verbreitet |
| **GELU** | $z \cdot \Phi(z)$ | Standard in Transformern |

**Historisch:** ReLU ist alt (Fukushima 1969), war aber lange unpopulär. In der frühen NN-Phase dominierten Sigmoid und Tanh; seit ~2010 ist ReLU wieder Standard, weil die sättigenden Funktionen bei tiefen Netzen zu **vanishing gradients** führen.

**Nachteil ReLU (dying ReLU):** Alles Negative wird exakt 0 → der Gradient dort ist ebenfalls 0. Eine unit, die für alle Trainingsdaten im negativen Bereich landet, bekommt nie wieder ein Update und ist tot. Genau dagegen helfen Leaky ReLU / ELU / GELU.

## 7. Bezug zum Training
Die Struktur ist fest, gesucht sind $\boldsymbol\Theta, \boldsymbol\theta_0, \boldsymbol\Phi, \boldsymbol\phi_0$. Das Training misst über eine **Loss-Funktion**, wie weit die Vorhersagen von den Labels abweichen, und passt die Parameter per Gradientenabstieg (Backpropagation) an → siehe [[Pytorch Workflow]].

## 8. Umsetzung in PyTorch
Das gesamte Kapitel entspricht diesen drei Zeilen:

```python
import torch.nn as nn

model = nn.Sequential(
    nn.Linear(D_in, D),   # Theta, theta_0  -> pre-activations
    nn.ReLU(),            # a()             -> hidden units h
    nn.Linear(D, D_out),  # Phi, phi_0      -> output y
)
```

---
**Links:** [[Deep Neural Network]], [[Pytorch Workflow]], [[Reinforcement Learning]]
