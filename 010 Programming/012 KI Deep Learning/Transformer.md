---
title: "Transformer"
type: konzept
status: fertig
tags:
  - deep-learning
  - neural-networks
  - nlp
  - ai
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 12 (Transformers)

---

## 1. Das Problem mit Sequenzen
Text, Zeitreihen und Messdaten haben zwei unangenehme Eigenschaften: **variable Länge** und **weitreichende Abhängigkeiten**. Ein MLP braucht feste Inputgröße, ein [[Convolutional Neural Network|CNN]] sieht nur lokale Fenster, ein RNN verarbeitet sequenziell und ist damit nicht parallelisierbar.

**Self-Attention** löst beides: Jedes Element schaut direkt auf jedes andere, und alle Positionen werden gleichzeitig berechnet.

## 2. Dot-Product Self-Attention
Jedes Input-Embedding $\mathbf x_n$ wird auf drei Vektoren projiziert:

$$\mathbf q_n = \boldsymbol\beta_q + \boldsymbol\Omega_q \mathbf x_n \qquad \mathbf k_n = \boldsymbol\beta_k + \boldsymbol\Omega_k \mathbf x_n \qquad \mathbf v_n = \boldsymbol\beta_v + \boldsymbol\Omega_v \mathbf x_n$$

* **Query:** "Was suche ich?"
* **Key:** "Was biete ich an?"
* **Value:** "Was gebe ich weiter, wenn ich gefragt werde?"

Die Ähnlichkeit von Query und Key bestimmt die Gewichtung:

$$\text{Sa}[\mathbf X] = \text{Softmax}\!\left(\frac{\mathbf Q \mathbf K^{T}}{\sqrt{D_q}}\right)\mathbf V$$

Der Output an Position $n$ ist eine **gewichtete Summe aller Values** — die Gewichte sind datenabhängig und werden zur Laufzeit berechnet. Genau das ist der Unterschied zu einer Faltung, deren Gewichte fest sind.

Der Faktor $\sqrt{D_q}$ verhindert, dass die Skalarprodukte bei großen Dimensionen zu groß werden und das Softmax in die Sättigung läuft (dort verschwindet der Gradient).

### Eigenschaften
* **Permutationsäquivariant:** Attention allein kennt keine Reihenfolge. Deshalb braucht es **Positional Encodings** (sinusförmig, gelernt oder relativ/RoPE), die vor dem ersten Block addiert werden.
* **Komplexität $O(N^2)$** in der Sequenzlänge — der zentrale Engpass und der Grund für Varianten wie FlashAttention oder lineare Attention.

## 3. Multi-Head Attention
Statt einer Attention rechnet man $H$ parallele "Köpfe" mit eigenen $\Omega_q, \Omega_k, \Omega_v$, konkateniert die Ergebnisse und projiziert zurück. Jeder Kopf kann eine andere Art von Beziehung modellieren (Syntax, Bezug, Position).

## 4. Der Transformer-Block
$$\mathbf X \leftarrow \mathbf X + \text{MhSa}\big[\text{LayerNorm}(\mathbf X)\big]$$
$$\mathbf X \leftarrow \mathbf X + \text{MLP}\big[\text{LayerNorm}(\mathbf X)\big]$$

Also: Attention **mischt Information zwischen den Positionen**, das MLP **verarbeitet jede Position einzeln** (mit geteilten Gewichten). Dazu Residual Connections und Layer Norm aus [[Residual Networks und Batch Normalization]]. Ein ganzes Modell ist nur dieser Block, N-mal gestapelt.

## 5. Die drei Bauformen
| Typ | Attention | Beispiel | Aufgabe |
| :--- | :--- | :--- | :--- |
| **Encoder** | bidirektional | BERT | Verstehen, Klassifikation, Embeddings |
| **Decoder** | **maskiert** (nur Vergangenheit) | GPT | autoregressive Generierung |
| **Encoder-Decoder** | beides + Cross-Attention | T5, Übersetzung | Sequenz zu Sequenz |

**Masked Attention** setzt alle zukünftigen Positionen vor dem Softmax auf $-\infty$. Nur so kann ein Decoder mit dem ganzen Text gleichzeitig trainiert werden, ohne zu "schummeln".

**Tokenisierung:** Text wird nicht in Wörter, sondern in Subwörter zerlegt (Byte Pair Encoding, WordPiece) — Kompromiss zwischen Vokabulargröße und Umgang mit unbekannten Wörtern.

## 6. Über Text hinaus
* **Vision Transformer (ViT):** Bild in 16x16-Patches zerlegen, als Sequenz behandeln. Schlägt CNNs bei sehr großen Datenmengen, ist bei wenig Daten unterlegen — weil ihm der induktive Bias der Faltung fehlt.
* **Zeitreihen:** direkt anwendbar, konkurriert aber mit einfacheren Modellen.
* **Decision Transformer:** RL als Sequenzmodellierung. Man konditioniert auf den *gewünschten* Return und lässt das Modell die passende Aktionsfolge generieren — interessant für Offline-RL aus Betriebsdaten ([[Policy Gradient und Actor Critic]]).

## 7. Einordnung
Der Transformer ist die derzeit dominierende Architektur, weil er drei Dinge vereint: **Parallelisierbarkeit**, **globale Abhängigkeiten in einem Schritt** und **sehr gute Skalierung** mit Daten und Parametern. Sein schwacher induktiver Bias ist beides — Fluch bei kleinen Datensätzen, Segen bei großen.

> **Für Regelungsaufgaben mit wenigen Sensorwerten ist ein Transformer Overkill.** Relevant wird er erst, wenn der Zustand eine echte Historie ist (POMDP, siehe [[Markov Decision Process]]) — und selbst dann ist Frame-Stacking oder ein LSTM (`RecurrentPPO` in sb3-contrib) meist der pragmatischere Weg.

---
**Links:** [[Deep Neural Network]], [[Residual Networks und Batch Normalization]], [[Convolutional Neural Network]], [[Graph Neural Network]], [[Generative Modelle]]
