---
tags: [ReinforcementLearning, Off-Policy, Actor-Critic, Deterministic]
---
# DDPG (Deep Deterministic Policy Gradient)

## Funktionsprinzip
DDPG ist ein **Off-Policy Actor-Critic** Algorithmus. Man kann ihn sich als eine Anpassung von DQN für **kontinuierliche Aktionsräume** vorstellen.

**Der Kernmechanismus: Deterministische Policy**
Bei kontinuierlichen Räumen kann man nicht (wie bei DQN) für jede mögliche Aktion einen Wert ausrechnen und den höchsten nehmen (es gibt unendlich viele Werte zwischen z.B. 0.1 und 0.2). Daher nutzt DDPG einen **Actor**, der deterministisch handelt: Das Netzwerk gibt direkt den exakten Aktionswert aus (z.B. Lenkwinkel = 14 Grad). Der **Critic** lernt dann, das Paar aus (Zustand + vorgeschlagene Aktion) zu bewerten.

**Was passiert genau?**
Da die Policy deterministisch ist, würde der Agent niemals neue Dinge ausprobieren (Exploration). Deshalb wird auf die Ausgabe des Actors künstliches Rauschen (oft Ornstein-Uhlenbeck-Noise) addiert, bevor die Aktion in der Umgebung ausgeführt wird. Die gesammelten Daten (Zustand, Aktion, Belohnung, nächster Zustand) landen in einem Replay Buffer. Aus diesem zieht DDPG Daten, um den Critic (Q-Wert) und den Actor in Richtung höherer Q-Werte zu trainieren.