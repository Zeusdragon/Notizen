---
tags: [ReinforcementLearning, Off-Policy, Actor-Critic, Maximum-Entropy]
---
# SAC (Soft Actor-Critic)

## Funktionsprinzip
SAC ist ein **Off-Policy Actor-Critic** Algorithmus, der auf dem Prinzip des "Maximum Entropy Reinforcement Learning" basiert.

**Der Kernmechanismus: Entropie-Maximierung**
Normale RL-Algorithmen wollen nur eines: Die höchste Belohnung (Reward) erreichen. SAC fügt ein zweites Ziel hinzu: **Entropie**. Entropie ist das Maß für Chaos oder Zufälligkeit. Der Algorithmus wird belohnt, wenn er so unvorhersehbar (zufällig) wie möglich handelt, *während* er gleichzeitig die Aufgabe erfolgreich löst.

**Was passiert genau?**
Stell dir vor, ein Roboter soll ein Glas Wasser greifen. Ein normaler Algorithmus lernt genau einen perfekten Pfad. Wenn das Glas um 1 cm verschoben wird, scheitert er. 
SAC lernt durch den Entropie-Bonus alle möglichen, funktionierenden Wege zu greifen. Wenn der Roboter eine Aktion berechnet, gibt er eine Wahrscheinlichkeitsverteilung aus. Der "Actor" lernt diese Verteilung, der "Critic" bewertet sie. Durch den Replay Buffer (Off-Policy) kann SAC auch alte Erfahrungen immer wieder zum Lernen nutzen, was ihn extrem dateneffizient macht.