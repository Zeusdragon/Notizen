---
title: "Reinforcement Learning"
type: konzept
status: fertig
tags:
  - reinforcement-learning
  - deep-learning
  - ai
  - agents
erstellt: 2025-12-05
aktualisiert: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 19 (Reinforcement learning)

> Einstiegsnotiz. Die Details stehen in den verlinkten Vertiefungsnotizen.

## 1. Definition
Reinforcement Learning ist ein Teilgebiet des Machine Learning, bei dem ein **Agent** lernt, Entscheidungen zu treffen, indem er mit einer **Umgebung** (Environment) interagiert. Ziel ist es, die kumulative Belohnung (Reward) über die Zeit zu maximieren.

Der Unterschied zum [[Supervised Learning]]: Es gibt keine richtige Antwort pro Beispiel, sondern nur ein Bewertungssignal — oft erst viele Schritte später. Und der Agent erzeugt seine Trainingsdaten selbst.

## 2. Der RL-Loop (Markov Decision Process)
Der Prozess wird formal als MDP beschrieben → [[Markov Decision Process]]:

1.  **State ($S_t$):** Der Agent beobachtet den aktuellen Zustand der Umgebung.
2.  **Action ($A_t$):** Basierend auf seiner Strategie (**Policy $\pi$**) wählt der Agent eine Aktion.
3.  **Reward ($R_{t+1}$):** Die Umgebung gibt Feedback, ob die Aktion gut oder schlecht war.
4.  **Next State ($S_{t+1}$):** Die Umgebung wechselt in einen neuen Zustand.

Maximiert wird nicht der einzelne Reward, sondern der **Return** $G_t = \sum_k \gamma^{k} r_{t+k+1}$ — die abdiskontierte Summe aller zukünftigen Belohnungen.

## 3. Wichtige Konzepte

### Policy ($\pi$)
Die Strategie des Agenten.
* **Deterministisch:** $a = \pi(s)$ (Immer gleiche Aktion bei gleichem Zustand).
* **Stochastisch:** $\pi(a|s)$ (Wahrscheinlichkeitsverteilung über Aktionen).

### Value Function ($V(s)$) & Q-Function ($Q(s, a)$)
* **Value:** Wie gut ist es, in Zustand $s$ zu sein? (Erwarteter zukünftiger Reward).
* **Q-Value:** Wie gut ist es, in Zustand $s$ die Aktion $a$ zu wählen?
    * Der Q-Wert erlaubt eine Entscheidung **ohne Modell der Umgebung** ($a^* = \arg\max_a Q(s,a)$) und ist deshalb die zentrale Größe im modellfreien RL.
    * Deep Q-Learning ([[DQN (Deep Q-Network)]]) nutzt ein Neural Network, um diese Q-Werte zu schätzen.
* Beide hängen über die **Bellman-Gleichung** zusammen → [[Bellman Gleichung und Dynamic Programming]].
* Ihre Differenz $A(s,a) = Q(s,a) - V(s)$ ist der **Advantage** — die Lerngröße aller Actor-Critic-Verfahren.

### Exploration vs. Exploitation
Das zentrale Dilemma im RL:
* **Exploration:** Neue Dinge ausprobieren, um evtl. bessere Strategien zu finden (Risiko).
* **Exploitation:** Das Wissen nutzen, das man schon hat, um Rewards zu sichern.

Umsetzung: $\epsilon$-greedy (bei Value-Verfahren), Entropie-Bonus oder Aktionsrauschen (bei Policy-Verfahren).

## 4. Arten von Algorithmen
* **Model-Free:** Der Agent lernt nur aus Erfahrung (Trial & Error).
    * *Value-Based:* [[Temporal Difference Learning und Q-Learning|Q-Learning]], DQN, QR-DQN — nur diskrete Aktionen, sehr dateneffizient.
    * *Policy-Based / Actor-Critic:* [[Policy Gradient und Actor Critic|REINFORCE, A2C, PPO, SAC]] — auch kontinuierliche Aktionen.
* **Model-Based:** Der Agent lernt ein Modell der Umgebung und plant darin (ähnlich zu [[Model Predictive Control]]).

Zweite Achse:
* **On-Policy** (PPO, A2C): lernt nur aus Daten der aktuellen Policy, verwirft sie danach.
* **Off-Policy** (DQN, SAC, TD3): lernt aus einem **Replay Buffer** und ist dadurch deutlich dateneffizienter — entscheidend, wenn ein Simulationsschritt teuer ist.

## 5. Vertiefung (Prince, Kapitel 19)
| Notiz | Inhalt |
| :--- | :--- |
| [[Markov Decision Process]] | 19.1 — Formalisierung, Return, Discount, Reward Design, Gym-API |
| [[Bellman Gleichung und Dynamic Programming]] | 19.2 — Value/Q-Funktion, Bellman, Policy & Value Iteration |
| [[Temporal Difference Learning und Q-Learning]] | 19.3–19.4 — TD, SARSA, Q-Learning, DQN, Double DQN |
| [[Policy Gradient und Actor Critic]] | 19.5–19.7 — REINFORCE, Advantage, GAE, PPO, Offline RL |
| [[SB3 Algorithmen Übersicht]] | Praxis: welcher Algorithmus wofür |

## 6. Praxisnotiz
Die häufigsten Ursachen für scheiterndes Training sind nicht die Hyperparameter, sondern:
1. **Zustand nicht Markov'sch** (relevante Information fehlt in der Observation),
2. **unnormalisierte Beobachtungen** ([[Backpropagation und Initialisierung]]),
3. **$\gamma$ passt nicht zur Zeitschrittweite** der Simulation,
4. **Reward misst das falsche Ziel** oder lässt sich austricksen.

---
**Links:** [[Markov Decision Process]], [[Bellman Gleichung und Dynamic Programming]], [[Temporal Difference Learning und Q-Learning]], [[Policy Gradient und Actor Critic]], [[SB3 Algorithmen Übersicht]], [[Model Predictive Control]], [[Pytorch Workflow]]
