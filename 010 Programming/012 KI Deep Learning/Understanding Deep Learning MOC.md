---
title: "Understanding Deep Learning MOC"
type: moc
tags:
  - MOC
  - deep-learning
  - ai
erstellt: 2026-09-07
---

# Understanding Deep Learning — Übersicht

Referenzwerk für diesen Ordner: **Simon J.D. Prince, *Understanding Deep Learning* (MIT Press, 2023)**. Eine Notiz pro Kapitel bzw. Kapitelgruppe.

## Grundlagen
| Kap. | Notiz | Kern |
| :--- | :--- | :--- |
| 2 | [[Supervised Learning]] | Modell, Loss, Training, Test — das Grundrezept |
| 3 | [[Shallow Neural Network]] | eine Hidden Layer, stückweise lineare Funktionen, Aktivierungen |
| 4 | [[Deep Neural Network]] | Verkettung von Schichten, Falten des Inputraums, Depth Efficiency |
| 5 | [[Loss Functions]] | Loss aus Maximum Likelihood herleiten statt aussuchen |
| 6 | [[Gradient Descent und Optimierer]] | SGD, Momentum, Adam, Lernraten |
| 7 | [[Backpropagation und Initialisierung]] | Kettenregel, vanishing/exploding, He-Init |
| 8 | [[Generalisierung und Double Descent]] | Bias/Varianz, Overfitting, Double Descent |
| 9 | [[Regularisierung]] | Weight Decay, Dropout, implizite Regularisierung |

## Architekturen
| Kap. | Notiz | Kern |
| :--- | :--- | :--- |
| 10 | [[Convolutional Neural Network]] | Faltung, weight sharing, Invarianz |
| 11 | [[Residual Networks und Batch Normalization]] | Skip Connections, Batch/Layer Norm |
| 12 | [[Transformer]] | Self-Attention, Multi-Head, Encoder/Decoder |
| 13 | [[Graph Neural Network]] | Message Passing, Permutationsäquivarianz |

## Generative Modelle
| Kap. | Notiz | Kern |
| :--- | :--- | :--- |
| 14–18 | [[Generative Modelle]] | GAN, Normalizing Flows, VAE, Diffusion |

## Reinforcement Learning
| Kap. | Notiz | Kern |
| :--- | :--- | :--- |
| 19 | [[Reinforcement Learning]] | Einstieg und Landkarte |
| 19.1 | [[Markov Decision Process]] | MDP, Return, Discount, Policy, Reward Design |
| 19.2 | [[Bellman Gleichung und Dynamic Programming]] | Value, **Q-Wert**, Bellman, Value Iteration |
| 19.3–19.4 | [[Temporal Difference Learning und Q-Learning]] | TD-Error, SARSA, Q-Learning, DQN |
| 19.5–19.7 | [[Policy Gradient und Actor Critic]] | REINFORCE, Advantage, GAE, PPO, Offline RL |

## Theorie und Einordnung
| Kap. | Notiz |
| :--- | :--- |
| 20–21 | [[Warum Deep Learning funktioniert]] |

## Praxis (nicht aus dem Buch)
* [[Pytorch Workflow]] — Implementierung
* [[Tensorboard]] — Trainings-Monitoring
* [[SB3 Algorithmen Übersicht]] — Algorithmenwahl in Stable Baselines 3
* [[Model Predictive Control]] — der klassische Gegenpol zum RL

---

## Roter Faden
Die ersten neun Kapitel bilden eine geschlossene Kette und lohnen sich in dieser Reihenfolge:

**Modell** ([[Shallow Neural Network|3]] → [[Deep Neural Network|4]]) → **Bewertung** ([[Loss Functions|5]]) → **Optimierung** ([[Gradient Descent und Optimierer|6]] → [[Backpropagation und Initialisierung|7]]) → **Generalisierung** ([[Generalisierung und Double Descent|8]] → [[Regularisierung|9]]).

Alles danach sind Varianten davon: andere Architekturen (10–13), andere Zielverteilungen (14–18), anderes Lernsignal (19).
