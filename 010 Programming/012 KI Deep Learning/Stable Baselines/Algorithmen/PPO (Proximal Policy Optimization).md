---
tags: [ReinforcementLearning, On-Policy, Actor-Critic]
---
# PPO (Proximal Policy Optimization)

## Funktionsprinzip
PPO ist ein **On-Policy Actor-Critic** Algorithmus. Er besteht aus zwei neuronalen Netzen:
1. **Actor:** Bestimmt, welche Aktion ausgeführt wird (Strategie/Policy).
2. **Critic:** Bewertet, wie gut der aktuelle Zustand ist (Value-Function).

**Der Kernmechanismus: Clipping**
Das größte Problem bei älteren Algorithmen (wie REINFORCE) war, dass ein Update der neuronalen Netze die Strategie komplett zerstören konnte. PPO löst das durch eine "Clipped Surrogate Objective Function". 
Wenn der Agent lernt, vergleicht er die *neue* Strategie mit der *alten*. PPO verbietet (clippt) Updates, die die Strategie zu drastisch verändern würden (meist auf maximal 20% Änderung pro Schritt limitiert). 

**Was passiert genau?**
Der Agent sammelt Daten in der Umgebung. Dann berechnet er, wie viel besser bestimmte Aktionen waren als erwartet (Advantage). Anstatt nun das Netzwerk komplett auf diese "guten" Aktionen auszurichten, macht PPO nur einen vorsichtigen Schritt in diese Richtung. Danach werden die gesammelten Daten weggeworfen (On-Policy) und der Prozess beginnt von vorn.