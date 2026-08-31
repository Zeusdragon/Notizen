---
title: "Reinforcement Learning"
type: konzept
status: in-bearbeitung
tags:
  - reinforcement-learning
  - deep-learning
  - ai
  - agents
erstellt: 2025-12-05
---

## 1. Definition
Reinforcement Learning ist ein Teilgebiet des Machine Learning, bei dem ein **Agent** lernt, Entscheidungen zu treffen, indem er mit einer **Umgebung** (Environment) interagiert. Ziel ist es, die kumulative Belohnung (Reward) über die Zeit zu maximieren.


## 2. Der RL-Loop (Markov Decision Process)
Der Prozess wird formal oft als MDP (Markov Decision Process) beschrieben:

1.  **State ($S_t$):** Der Agent beobachtet den aktuellen Zustand der Umgebung.
2.  **Action ($A_t$):** Basierend auf seiner Strategie (**Policy $\pi$**) wählt der Agent eine Aktion.
3.  **Reward ($R_{t+1}$):** Die Umgebung gibt Feedback, ob die Aktion gut oder schlecht war.
4.  **Next State ($S_{t+1}$):** Die Umgebung wechselt in einen neuen Zustand.

## 3. Wichtige Konzepte

### Policy ($\pi$)
Die Strategie des Agenten.
* **Deterministisch:** $a = \pi(s)$ (Immer gleiche Aktion bei gleichem Zustand).
* **Stochastisch:** $\pi(a|s)$ (Wahrscheinlichkeitsverteilung über Aktionen).

### Value Function ($V(s)$) & Q-Function ($Q(s, a)$)
* **Value:** Wie gut ist es, in Zustand $s$ zu sein? (Erwarteter zukünftiger Reward).
* **Q-Value:** Wie gut ist es, in Zustand $s$ die Aktion $a$ zu wählen?
    * Deep Q-Learning (DQN) nutzt ein Neural Network, um diese Q-Werte zu schätzen.

### Exploration vs. Exploitation
Das zentrale Dilemma im RL:
* **Exploration:** Neue Dinge ausprobieren, um evtl. bessere Strategien zu finden (Risiko).
* **Exploitation:** Das Wissen nutzen, das man schon hat, um Rewards zu sichern.

## 4. Arten von Algorithmen
* **Model-Free:** Der Agent lernt nur aus Erfahrung (Trial & Error).
    * *Beispiele:* Q-Learning, Policy Gradients (PPO, A3C).
* **Model-Based:** Der Agent lernt ein Modell der Umgebung und plant darin (ähnlich zu [[Model Predictive Control]]).

---
**Links:** [[Pytorch Workflow]], [[Model Predictive Control]]