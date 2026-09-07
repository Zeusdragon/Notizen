---
title: "Backpropagation und Initialisierung"
type: konzept
status: fertig
tags:
  - deep-learning
  - training
  - ai
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 7 (Gradients and initialization)

---

## 1. Warum ein eigenes Verfahren?
[[Gradient Descent und Optimierer|Gradient Descent]] braucht $\partial L / \partial \boldsymbol\phi$ für **jeden einzelnen Parameter**. Bei Millionen Parametern ist numerisches Differenzieren (ein Forward-Pass pro Parameter) völlig aussichtslos.

**Backpropagation** berechnet alle Ableitungen in *einem* Rückwärtsdurchlauf, der ungefähr so teuer ist wie der Vorwärtsdurchlauf. Es ist kein Lernalgorithmus, sondern nur eine effiziente Anwendung der **Kettenregel**.

## 2. Zwei Durchläufe
### Forward Pass
Netz auswerten und dabei **alle Zwischenergebnisse speichern** (Pre-Activations $f_k$, Activations $h_k$). Diese Speicherung ist der Grund, warum große Batches viel VRAM brauchen.

### Backward Pass
Vom Loss aus rückwärts. Zwei Sorten von Ableitungen:

1. **Ableitung nach den Aktivierungen** wird durchgereicht:
$$\frac{\partial L}{\partial \mathbf f_{k}} = \boldsymbol\Omega_{k+1}^{T}\,\Big(\mathbb 1[\mathbf f_{k+1} > 0] \odot \frac{\partial L}{\partial \mathbf f_{k+1}}\Big)$$
2. **Ableitung nach den Parametern** wird an jeder Schicht abgegriffen:
$$\frac{\partial L}{\partial \boldsymbol\Omega_k} = \frac{\partial L}{\partial \mathbf f_k}\,\mathbf h_{k}^{T} \qquad\qquad \frac{\partial L}{\partial \boldsymbol\beta_k} = \frac{\partial L}{\partial \mathbf f_k}$$

Bei ReLU ist die lokale Ableitung eine simple 0/1-Maske: Wo die Pre-Activation negativ war, kommt kein Gradient durch (das ist die Ursache des **dying ReLU**, siehe [[Shallow Neural Network]]).

**Aufwand:** Vorwärts wie rückwärts dominieren Matrixmultiplikationen. Backprop kostet etwa das Doppelte des Forward-Passes, aber Speicher proportional zur Netzgröße mal Batchgröße.

## 3. Das Stabilitätsproblem
Der Gradient ist ein **Produkt** vieler Matrizen. Bei $K$ Schichten multiplizieren sich die Skalierungen auf:

* Sind die Gewichte im Schnitt zu klein: **Vanishing Gradients**, die frühen Schichten lernen nicht mehr.
* Sind sie zu groß: **Exploding Gradients**, Loss wird NaN.

Beide Effekte sind exponentiell in der Tiefe. Deshalb waren tiefe Netze vor ~2010 praktisch untrainierbar.

## 4. Initialisierung
Ziel: Die **Varianz der Aktivierungen** soll von Schicht zu Schicht konstant bleiben, und die Varianz der Gradienten ebenso.

* Alles auf **null** setzen ist tödlich: Alle units einer Schicht bekämen identische Gradienten und blieben für immer identisch (Symmetrie wird nie gebrochen).
* **He-Initialisierung** (für ReLU): Gewichte normalverteilt mit
$$\sigma^2 = \frac{2}{D_h}$$
Der Faktor 2 kompensiert, dass ReLU im Mittel die Hälfte der Aktivierungen auf null setzt.
* **Xavier/Glorot** (für Tanh/Sigmoid): $\sigma^2 = 1/D_h$ bzw. $2/(D_{in}+D_{out})$.
* Biases werden üblicherweise auf null gesetzt.

PyTorch initialisiert `nn.Linear` per Default sinnvoll. Bewusst eingreifen muss man vor allem bei eigenen Architekturen, und im RL beim **Output-Layer der Policy**: eine sehr kleine Initialisierung (z. B. `gain=0.01`) sorgt für eine anfangs nahezu uniforme Aktionsverteilung und verhindert, dass der Agent von Anfang an auf eine Aktion festgenagelt ist.

## 5. Was heute zusätzlich hilft
| Mittel | Wirkung |
| :--- | :--- |
| [[Residual Networks und Batch Normalization|Residual Connections]] | schaffen einen direkten Gradientenpfad an allen Schichten vorbei |
| [[Residual Networks und Batch Normalization|Batch/Layer Norm]] | halten die Aktivierungsstatistik zur Laufzeit stabil |
| **Gradient Clipping** | kappt die Gradientennorm hart (in SB3: `max_grad_norm=0.5`) |
| **Input-Normalisierung** | schlecht skalierte Inputs erzeugen schlecht skalierte Gradienten |

> **Praxis RL:** Unnormalisierte Zustände sind die häufigste Ursache für scheiterndes Training bei Simulationsdaten. Ein Vektor aus Temperatur in Kelvin (~300), Druck in Pa ($10^5$) und einem Ventilzustand (0/1) ist für ein Netz unlernbar. `VecNormalize` in SB3 (oder eigene Skalierung auf ungefähr Mittelwert 0, Std 1) ist keine Kür, sondern Voraussetzung.

## 6. Autograd in PyTorch
Man implementiert Backprop nie selbst. PyTorch baut beim Forward-Pass dynamisch einen Berechnungsgraphen auf; `loss.backward()` läuft ihn rückwärts ab und legt die Gradienten in `.grad` ab.

```python
optimizer.zero_grad()   # Gradienten sind kumulativ -> zwingend zurücksetzen
loss = criterion(model(x), y)
loss.backward()         # Backpropagation
torch.nn.utils.clip_grad_norm_(model.parameters(), 0.5)
optimizer.step()        # Parameter-Update
```

Der häufigste Anfängerfehler ist das vergessene `zero_grad()`, der zweithäufigste ein fehlendes `with torch.no_grad():` bei der Auswertung.

---
**Links:** [[Gradient Descent und Optimierer]], [[Deep Neural Network]], [[Shallow Neural Network]], [[Residual Networks und Batch Normalization]], [[Pytorch Workflow]]
