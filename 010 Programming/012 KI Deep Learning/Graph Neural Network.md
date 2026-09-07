---
title: "Graph Neural Network"
type: konzept
status: fertig
tags:
  - deep-learning
  - neural-networks
  - ai
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 13 (Graph neural networks)

---

## 1. Wann braucht man das?
Viele Daten sind weder Vektor, noch Gitter, noch Sequenz, sondern ein **Graph**: Moleküle, Straßen- und Stromnetze, soziale Netze, Rohrleitungs- und Anlagentopologien, Finite-Elemente-Netze.

Ein Graph $\mathcal G = (\mathcal V, \mathcal E)$ besteht aus Knoten mit Embeddings $\mathbf X \in \mathbb R^{D \times N}$ und Kanten, beschrieben durch die **Adjazenzmatrix** $\mathbf A$.

## 2. Das Kernproblem: Permutationsäquivarianz
Ein Graph hat keine kanonische Knotenreihenfolge. Nummeriert man die Knoten um, muss dasselbe herauskommen. Ein MLP auf einer flachgeklopften Adjazenzmatrix erfüllt das nicht — es müsste jede der $N!$ Permutationen einzeln lernen.

Die Lösung ist dieselbe wie beim [[Convolutional Neural Network|CNN]]: **geteilte Gewichte** plus eine **permutationsinvariante Aggregation** (Summe, Mittelwert, Maximum) über die Nachbarn.

## 3. Graph Convolution / Message Passing
Jede Schicht aktualisiert jedes Knoten-Embedding aus sich selbst und seinen Nachbarn:

$$\mathbf h_n^{(k+1)} = a\Big(\boldsymbol\beta + \boldsymbol\Omega_{self}\,\mathbf h_n^{(k)} + \boldsymbol\Omega_{neigh}\sum_{m \in \mathcal N(n)} \mathbf h_m^{(k)}\Big)$$

In Matrixform kompakt: $\mathbf H^{(k+1)} = a\big(\boldsymbol\beta\mathbf 1^{T} + \boldsymbol\Omega\,\mathbf H^{(k)}(\mathbf A + \mathbf I)\big)$

Nach $K$ Schichten hat jeder Knoten Information aus seiner $K$-Hop-Nachbarschaft gesehen — exakt analog zum wachsenden Receptive Field im CNN.

**Varianten:** normierte Aggregation (GCN), gelernte Gewichtung der Nachbarn per Attention (**GAT**, siehe [[Transformer]]), Einbeziehung von Kantenmerkmalen.

## 4. Aufgabentypen
| Ebene | Beispiel | Output |
| :--- | :--- | :--- |
| **Knoten** | Betrugserkennung im Netzwerk | pro Knoten eine Klasse |
| **Kante** | Link Prediction, Empfehlungen | pro Kantenpaar ein Score |
| **Graph** | Molekül-Eigenschaft vorhersagen | ein Vektor pro Graph (nach globalem Pooling) |

## 5. Praktische Eigenheiten
* **Transduktiv vs. induktiv:** Lernt man auf *einem* großen Graphen (transduktiv, z. B. ein soziales Netz) oder auf vielen kleinen (induktiv, z. B. Moleküle)? Das ändert Batching und Evaluation komplett.
* **Batching:** Mehrere Graphen werden zu einem blockdiagonalen Supergraphen zusammengefasst.
* **Nachbarschafts-Sampling (GraphSAGE):** Bei riesigen Graphen wird nur eine Stichprobe der Nachbarn genutzt, sonst explodiert die Nachbarschaft mit der Tiefe.
* **Over-smoothing:** Bei zu vielen Schichten gleichen sich alle Knoten-Embeddings an. GNNs bleiben deshalb meist flach (2-4 Schichten).

## 6. Einordnung
GNNs sind das dritte Beispiel für dasselbe Prinzip: Die **Symmetrie der Daten** wird in die Architektur eingebaut, statt sie aus Daten lernen zu müssen.

| Architektur | Eingebaute Symmetrie |
| :--- | :--- |
| CNN | Translation im Gitter |
| Transformer | Permutation (+ Position per Encoding) |
| GNN | Permutation der Knoten / Graphstruktur |

> **Möglicher Bezug zur Anlagentechnik:** Ein hydraulisches oder kältetechnisches System *ist* ein Graph (Komponenten als Knoten, Rohre als Kanten). Ein GNN kann daher eine gelernte Ersatzdynamik über verschiedene Anlagentopologien hinweg generalisieren, während ein MLP für jede Topologie neu trainiert werden müsste. Für eine einzelne, feste Anlage ist das aber unnötiger Aufwand.

---
**Links:** [[Convolutional Neural Network]], [[Transformer]], [[Deep Neural Network]]
