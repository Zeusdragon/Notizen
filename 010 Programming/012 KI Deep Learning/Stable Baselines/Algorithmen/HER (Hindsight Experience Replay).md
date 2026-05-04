---
tags: [ReinforcementLearning, Replay-Buffer, Goal-Conditioned]
---
# HER (Hindsight Experience Replay)

## Funktionsprinzip
HER ist kein eigenständiger RL-Algorithmus (wie PPO oder SAC), sondern eine **intelligente Art, den Replay Buffer (Speicher) zu verwalten**. Es wird als Erweiterung für Off-Policy-Methoden (z.B. DQN, SAC, TD3) genutzt.

**Der Kernmechanismus: Lernen aus Fehlern durch Rückschau (Hindsight)**
HER löst das Problem der "Sparse Rewards". Das sind Umgebungen, in denen es nur eine Belohnung (+1) gibt, wenn man das exakte Ziel erreicht, und sonst immer 0. Standard-Algorithmen finden dieses Ziel oft zufällig nie und lernen somit gar nichts.
HER manipuliert die gespeicherten Erfahrungen: Wenn der Agent sein Ziel verfehlt, ändert HER im Nachhinein (Hindsight) das Ziel in den Replay-Daten. Es tut so, als wäre der Ort, an dem der Agent *tatsächlich* gelandet ist, das eigentliche Ziel gewesen.

**Was passiert genau?**
Ein Roboter soll einen Ball an Position X werfen. Er wirft ihn stattdessen an Position Y. Normalerweise gäbe das 0 Punkte und keinen Lerneffekt. Mit HER speichert das System eine *zusätzliche* Erinnerung ab, in der steht: "Ziel war Position Y, Aktion hat Position Y erreicht -> Erfolg, +1 Punkt!". Der Agent lernt also, wie man die Welt kontrolliert, selbst wenn er das Hauptziel verfehlt. Er lernt nützliche Zwischenschritte.