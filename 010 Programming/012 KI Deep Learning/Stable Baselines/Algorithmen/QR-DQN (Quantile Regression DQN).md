---
tags: [ReinforcementLearning, Off-Policy, Value-Based, DistributionalRL, SB3-Contrib]
aliases: [Quantile Regression DQN, Distributional DQN]
title: "QR-DQN (Quantile Regression DQN)"
type: konzept
erstellt: 2026-05-04
---

# QR-DQN (Quantile Regression DQN)

> [!info] sb3-contrib
> QR-DQN ist nicht im Standardpaket von Stable Baselines 3 enthalten, sondern im offiziellen Erweiterungspaket `sb3-contrib`. Es lässt sich aber genauso bedienen wie die Standard-Algorithmen.

## Funktionsprinzip
QR-DQN ist ein **Off-Policy Value-Based** Algorithmus. Er ist eine direkte Weiterentwicklung des klassischen [[DQN (Deep Q-Network)]] und gehört zur Familie des **Distributional Reinforcement Learning**.

**Das Problem von Standard-DQN: Der Erwartungswert**
Normale Value-basierte Algorithmen (wie DQN) schätzen für eine Aktion immer nur einen einzigen Wert: den Durchschnitt (Erwartungswert). 
*Beispiel:* Wenn eine Aktion zu 50% eine Belohnung von `+10` und zu 50% eine Strafe von `-10` bringt, sagt DQN: *"Der Wert dieser Aktion ist `0`."* 
Das führt bei komplexen Problemen oft zu massiven Fehleinschätzungen und dem berüchtigten *Overestimation Bias* (das Netzwerk wird zu optimistisch und die Strategie bricht zusammen).

**Der Kernmechanismus: Wahrscheinlichkeitsverteilungen lernen**
QR-DQN sagt nicht: "Der Durchschnitt ist X". Stattdessen lernt es die **komplette Verteilung** der möglichen zukünftigen Belohnungen. Es teilt die Wahrscheinlichkeit in feste Stücke ein (sogenannte Quantile, standardmäßig oft 200 Stück).

Das Netzwerk sagt also voraus: *"Zu 10% passiert das Schlimmste (-10 Punkte), zu 50% passiert das Normale (+5 Punkte) und zu 5% kriege ich den Jackpot (+50 Punkte)."* 
Aus dieser exakten Verteilung berechnet der Agent dann seine Entscheidung.

**Was passiert genau?**
Der Ablauf (Interaktion mit der Umgebung, Speichern im Replay Buffer) ist exakt identisch mit normalem DQN. Der Unterschied liegt rein in der Architektur des neuronalen Netzes und der mathematischen Verlustfunktion (Loss Function). Statt des üblichen Mean-Squared-Error (MSE) nutzt QR-DQN die sogenannte "Huber Quantile Regression Loss". 

**Das Ergebnis:** Der Agent lernt viel nuancierter, welche Aktionen ein hohes Risiko bergen (selbst wenn der Durchschnitt gut wäre), wodurch das Training drastisch stabiler und erfolgreicher wird als beim alten DQN.