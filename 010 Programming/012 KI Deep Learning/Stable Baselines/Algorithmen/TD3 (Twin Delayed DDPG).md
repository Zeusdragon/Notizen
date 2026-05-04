---
tags: [ReinforcementLearning, Off-Policy, Actor-Critic, Deterministic]
---
# TD3 (Twin Delayed DDPG)

## Funktionsprinzip
TD3 ist ein **Off-Policy Actor-Critic** Algorithmus für kontinuierliche Räume und löst die fundamentalen mathematischen Probleme seines Vorgängers DDPG.

**Der Kernmechanismus: Drei Tricks gegen Überschätzung**
Bei DDPG tendiert der Critic dazu, Aktionen viel zu positiv zu bewerten (Overestimation Bias), was die Strategie ruiniert. TD3 nutzt drei Mechanismen dagegen:
1. **Twin Q-Networks:** Es werden *zwei* unabhängige Critics trainiert. Bei der Bewertung einer Aktion wird immer der *niedrigere* Wert der beiden genommen (Pessimismus).
2. **Delayed Policy Updates:** Der Critic lernt sehr schnell, der Actor wird aber künstlich gebremst. Der Actor wird zz.B. nur bei jedem zweiten Update des Critics aktualisiert, damit er auf Basis soliderer Bewertungen lernt.
3. **Target Policy Smoothing:** Beim Berechnen des Ziel-Wertes für den Critic wird künstliches Rauschen (Noise) auf die Aktion addiert. Das verhindert, dass das Netzwerk "Spitzen" in der Q-Funktion ausnutzt.

**Was passiert genau?**
Der Agent agiert deterministisch (schlägt für einen Zustand eine feste Zahl vor, z.B. 0.82 Gas geben). Um zu explorieren, wird Rauschen hinzugefügt. Das Lernen erfolgt extrem dateneffizient aus dem Replay Buffer, wobei die drei genannten Tricks sicherstellen, dass die Netzwerke nicht in eine Fehlkalkulation abdriften.