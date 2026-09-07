---
title: "Convolutional Neural Network"
type: konzept
status: fertig
tags:
  - deep-learning
  - neural-networks
  - computer-vision
  - ai
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 10 (Convolutional networks)

---

## 1. Warum kein MLP für Bilder?
Ein 224x224 RGB-Bild hat 150.528 Inputs. Eine einzige fully-connected Schicht mit gleicher Breite hätte über $2\cdot10^{10}$ Gewichte. Drei Probleme:

1. **Parameterexplosion** — nicht trainierbar, nicht speicherbar.
2. **Keine Ortsunabhängigkeit** — ein MLP müsste "Katze oben links" und "Katze unten rechts" getrennt lernen.
3. **Nachbarschaft geht verloren** — für ein MLP sind benachbarte Pixel nicht ähnlicher als weit entfernte.

Ein CNN löst alle drei über einen **induktiven Bias**: lokale Verbindungen + geteilte Gewichte.

## 2. Invarianz und Äquivarianz
* **Äquivariant:** Verschiebt sich der Input, verschiebt sich der Output mit. Das gilt für die Faltung selbst.
* **Invariant:** Der Output ändert sich bei Verschiebung nicht. Das erzeugen Pooling und globale Aggregation am Ende.

Ein Klassifikator soll invariant sein, eine Segmentierung äquivariant.

## 3. Die Faltung
Ein kleiner **Kernel** wird über den Input geschoben; überall dieselben Gewichte (**weight sharing**). 1D-Fall mit Kernelgröße 3:

$$h_i = a\big(\beta + \omega_1 x_{i-1} + \omega_2 x_i + \omega_3 x_{i+1}\big)$$

### Die Stellschrauben
| Parameter | Bedeutung | Effekt |
| :--- | :--- | :--- |
| **Kernel size** | Größe des Fensters (3x3, 5x5) | größer = mehr Kontext, mehr Parameter |
| **Stride** | Schrittweite | Stride 2 halbiert die Auflösung |
| **Padding** | Rand auffüllen ("zero padding", "valid", "same") | erhält die Größe |
| **Dilation** | Löcher im Kernel | vergrößert das Sichtfeld ohne Mehrkosten |
| **Channels** | Anzahl paralleler Kernel | jeder Kernel lernt ein anderes Muster |

Ein Layer mit $C_{in}$ Eingangs- und $C_{out}$ Ausgangskanälen und Kernel $k \times k$ hat $C_{in}\cdot C_{out}\cdot k^2 + C_{out}$ Parameter — unabhängig von der Bildgröße.

## 4. Receptive Field
Das Sichtfeld eines Neurons wächst mit jeder Schicht. Zwei gestapelte 3x3-Faltungen sehen denselben Bereich wie eine 5x5 — aber mit weniger Parametern und einer zusätzlichen Nichtlinearität. Deshalb bestehen moderne Netze aus vielen kleinen Kerneln statt wenigen großen.

## 5. Downsampling und Upsampling
* **Max Pooling:** Maximum im Fenster. Auflösung runter, leichte Verschiebungsinvarianz.
* **Average Pooling:** Mittelwert.
* **Strided Convolution:** lernbares Downsampling, hat Pooling in vielen Architekturen ersetzt.
* **Transposed Convolution:** lernbares Upsampling (für Segmentierung, Generative Modelle).

## 6. Typischer Aufbau und Meilensteine
Klassisches Muster: `[Conv -> ReLU -> Conv -> ReLU -> Pool] x N -> Flatten/GlobalPool -> Linear`. Die räumliche Auflösung sinkt, die Kanalzahl steigt.

| Netz | Beitrag |
| :--- | :--- |
| **LeNet** (1998) | erstes praktisch nutzbares CNN |
| **AlexNet** (2012) | ImageNet-Durchbruch, GPUs, ReLU, Dropout |
| **VGG** (2014) | tief und uniform, nur 3x3-Kernel |
| **ResNet** (2015) | Residual Connections, siehe [[Residual Networks und Batch Normalization]] |
| **U-Net** | Encoder-Decoder mit Skip-Connections, Standard für Segmentierung und [[Generative Modelle|Diffusion]] |

## 7. Einordnung
CNNs sind kein Selbstzweck, sondern das Musterbeispiel dafür, dass **eingebautes Vorwissen** über die Datenstruktur schlägt, was man sonst aus Daten lernen müsste. Dieselbe Idee liegt [[Graph Neural Network|GNNs]] (Permutationsäquivarianz) und [[Transformer]]n (Attention über Sequenzen) zugrunde.

> **Bezug RL:** Bei Bild-Beobachtungen nutzt man die `CnnPolicy` (SB3 bringt die NatureCNN-Architektur aus dem DQN-Paper mit). Für Zustandsvektoren aus einer Simulation ist das nutzlos — dort ist die `MlpPolicy` richtig, weil es keine räumliche Nachbarschaftsstruktur gibt, die ein Kernel ausnutzen könnte.

---
**Links:** [[Deep Neural Network]], [[Residual Networks und Batch Normalization]], [[Transformer]], [[Regularisierung]], [[Pytorch Workflow]]
