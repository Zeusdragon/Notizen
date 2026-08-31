---
tags: [ReinforcementLearning, Off-Policy, Value-Based]
title: "DQN (Deep Q-Network)"
type: konzept
erstellt: 2026-05-04
---

# DQN (Deep Q-Network)

## Funktionsprinzip
DQN ist ein **Off-Policy Value-Based** Algorithmus. Im Gegensatz zu Actor-Critic-Methoden gibt es hier keinen "Actor", der explizit lernt, *was* zu tun ist. Stattdessen lernt der Agent nur, *wie wertvoll* bestimmte Aktionen sind.

**Der Kernmechanismus: Q-Values und Target Networks**
DQN nutzt ein neuronales Netz, um die "Q-Funktion" anzunähern. Der Q-Wert steht für die maximal zu erwartende zukünftige Belohnung, wenn man im Zustand $S$ die Aktion $A$ ausführt.
Damit das Training nicht kollabiert (da sich das Netz während des Lernens ständig selbst verändert), nutzt DQN zwei Netzwerke:
1. Das **Main Network** (wird bei jedem Schritt aktualisiert).
2. Das **Target Network** (eine eingefrorene Kopie des Main Networks, die nur alle paar tausend Schritte aktualisiert wird, um feste Ziele für das Lernen vorzugeben).

**Was passiert genau?**
Der Agent schaut sich den Zustand an (z.B. den Bildschirm eines Atari-Spiels). Das Netzwerk spuckt für jede mögliche Taste (Links, Rechts, Springen) einen Wert aus (z.B. Links: 10 Punkte, Rechts: 50 Punkte, Springen: -5 Punkte). Der Agent nimmt einfach die Aktion mit dem höchsten Wert (argmax). Seine Erfahrungen speichert er in einem Replay Buffer ab, aus dem er zufällig Batches zieht, um daraus zu lernen.