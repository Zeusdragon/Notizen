---
title: "Residual Networks und Batch Normalization"
type: konzept
status: fertig
tags:
  - deep-learning
  - neural-networks
  - training
  - ai
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 11 (Residual networks)

---

## 1. Das Problem mit sehr tiefen Netzen
Empirisch: Ab ~20 Schichten wird ein einfaches Netz beim Training **schlechter**, nicht besser — und zwar schon im *Trainingsfehler*. Es ist also kein Overfitting, sondern ein Optimierungsproblem:

* **Shattered Gradients:** Bei tiefen Netzen ist der Gradient bezüglich der frühen Schichten nahezu unkorreliert mit sich selbst bei kleinen Parameteränderungen. Die Loss-Fläche wird "zerhackt", und Gradient Descent findet keine verlässliche Abstiegsrichtung.
* Dazu die exponentielle Skalierung aus [[Backpropagation und Initialisierung]] (vanishing/exploding).

## 2. Residual Connections
Statt $\mathbf h_{k+1} = f(\mathbf h_k)$ rechnet man:

$$\mathbf h_{k+1} = \mathbf h_k + f(\mathbf h_k, \boldsymbol\phi_k)$$

Die Schicht lernt also nur noch die **Abweichung** vom Identitäts-Durchgang (das "Residuum"). Zwei Konsequenzen:

1. Die Identität ist gratis. Eine überflüssige Schicht muss nur $f \approx 0$ lernen — ein tieferes Netz kann nie schlechter sein als ein flacheres.
2. Der Gradient fließt über die Additionen **ungehindert** bis nach vorne durch (die Ableitung der Summe enthält den Term 1).

### Unraveling
Ein Netz mit $K$ Residualblöcken lässt sich ausmultiplizieren zu $2^{K}$ parallelen Pfaden unterschiedlicher Länge. Ein ResNet verhält sich dadurch weniger wie ein sehr tiefes Netz und mehr wie ein **implizites Ensemble** vieler kürzerer Netze — das erklärt gleichzeitig die gute Trainierbarkeit und die gute Generalisierung.

**Problem dabei:** Die Varianz verdoppelt sich bei jeder Addition, wächst also exponentiell mit der Tiefe. Genau deshalb braucht man Normalisierung.

## 3. Batch Normalization
Normiere die Aktivierungen einer unit über den **Batch** auf Mittelwert 0 und Varianz 1 und lass das Netz anschließend selbst entscheiden, welche Statistik es haben will:

$$\hat h = \frac{h - \mu_{\text{batch}}}{\sqrt{\sigma^2_{\text{batch}} + \epsilon}} \qquad h' = \gamma\,\hat h + \delta$$

$\gamma$ (Skala) und $\delta$ (Offset) sind **gelernte** Parameter.

**Was BatchNorm bringt:**
* stabile Aktivierungs- und Gradientenstatistik über die Tiefe
* erlaubt deutlich **höhere Lernraten** und schnellere Konvergenz
* macht das Netz robuster gegen die Initialisierung
* wirkt als [[Regularisierung|Regularisierer]] — die Batch-Statistik ist verrauscht, jedes Beispiel wird je nach Batchpartnern anders normiert

**Fallstricke:**
* **Train- und Eval-Verhalten unterscheiden sich.** Beim Training wird die Batch-Statistik genutzt, bei der Inferenz ein laufender Mittelwert. `model.eval()` ist Pflicht.
* Bei **kleinen Batches** wird die Statistik unbrauchbar.
* In rekurrenten Netzen und bei variablen Sequenzlängen unpraktisch.

### Die Alternativen
| Variante | Normiert über | Typischer Einsatz |
| :--- | :--- | :--- |
| **Batch Norm** | Batch, pro Kanal | CNNs mit großen Batches |
| **Layer Norm** | alle Features eines Samples | [[Transformer]], RNNs, RL |
| **Group Norm** | Kanalgruppen eines Samples | CNNs mit kleinen Batches |
| **Instance Norm** | einzelner Kanal eines Samples | Style Transfer |

Layer Norm ist batchunabhängig und verhält sich in Training und Inferenz identisch — deshalb der Standard überall dort, wo Batches klein oder Daten sequenziell sind.

## 4. Bekannte Architekturen
* **ResNet-50/101/152:** Bottleneck-Blöcke (1x1 -> 3x3 -> 1x1), bis über 100 Schichten trainierbar.
* **Pre-Activation ResNet (v2):** Normalisierung und Aktivierung *vor* der Faltung, dadurch ein völlig ungestörter Identitätspfad.
* **DenseNet:** jede Schicht bekommt alle vorherigen als Input (Konkatenation statt Addition).
* **U-Net:** Skip-Connections zwischen Encoder und Decoder, Basis der meisten Diffusionsmodelle ([[Generative Modelle]]).

## 5. Einordnung
Residual Connections + Normalisierung sind der Grund, warum "deep" heute wirklich tief sein darf. Beides steckt in praktisch jeder modernen Architektur — im [[Transformer]] ist jeder einzelne Block als `x + Attention(LayerNorm(x))` aufgebaut.

> **Bezug RL:** Policy-Netze sind meist klein (2 Schichten), Residual Connections sind dort unnötig. Relevant ist stattdessen die **Beobachtungsnormalisierung** (`VecNormalize`) — dieselbe Idee, nur am Eingang statt zwischen den Schichten. BatchNorm im Policy-Netz ist wegen der kleinen, korrelierten RL-Batches eher schädlich; wenn Normalisierung im Netz, dann Layer Norm.

---
**Links:** [[Deep Neural Network]], [[Convolutional Neural Network]], [[Backpropagation und Initialisierung]], [[Transformer]], [[Regularisierung]]
