---
title: "Pytorch Workflow"
type: konzept
status: fertig
tags:
  - pytorch
  - coding
  - python
  - deep-learning
erstellt: 2025-12-05
---

**Quelle:** Eigene Projektstruktur

---

## Übersicht: Die 7 Schritte
Dieser Workflow beschreibt den Standard-Prozess für ein Deep Learning Projekt in PyTorch.

### 1. Datenquelle & Import
* **Ziel:** Rohdaten laden (aus CSV, Datenbanken, Bildern oder Messdateien).
* **Tools:** `pandas`, `numpy`, `PIL`.
* Erste Verarbeitung findet oft hier statt, bevor es zu PyTorch geht.

### 2. Datenvorverarbeitung (Tensors & Split)
* **Konvertierung:** Daten müssen in `torch.Tensor` umgewandelt werden. Alles in PyTorch ist ein Tensor.
* **Split:** Unterteilung der Daten, um Overfitting zu vermeiden.
    * `X` (Features) und `Y` (Labels).
    * **Train Set:** Zum Lernen der Gewichte.
    * **Validation Set:** Zum Tunen von Hyperparametern während des Trainings.
    * **Test Set:** Zur finalen Prüfung (nie im Training verwenden!).
* **Code:** `sklearn.model_selection.train_test_split` ist hier oft hilfreich.

### 3. DataLoader & Dataset
PyTorch nutzt spezielle Klassen, um Daten effizient in Batches (Stapel) zu laden.
* **Dataset Class:** Muss `__len__` und `__getitem__` implementieren.
* **DataLoader:** Nimmt das Dataset und erstellt Batches, shuffelt die Daten und lädt parallel.
```python
from torch.utils.data import DataLoader, TensorDataset
train_loader = DataLoader(dataset=train_data, batch_size=32, shuffle=True)