---
title: "Policy Gradient und Actor Critic"
type: konzept
status: fertig
tags:
  - reinforcement-learning
  - deep-learning
  - ai
  - policy-based
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 19.5 (Policy gradient methods) und 19.6 (Actor-critic methods)

---

## 1. Der andere Weg
[[Temporal Difference Learning und Q-Learning|Value-Based]]-Verfahren lernen $q[s,a]$ und leiten die Policy daraus per $\arg\max$ ab. **Policy-Based**-Verfahren überspringen das: Sie parametrisieren die Policy $\pi(a|s,\boldsymbol\theta)$ direkt als Neuronales Netz und schieben die Parameter per Gradientenaufstieg in Richtung höherer Returns.

**Warum überhaupt?**
* **Kontinuierliche Aktionen** ohne $\max_a$-Problem — das Netz gibt z. B. $\mu$ und $\sigma$ einer Normalverteilung aus.
* **Stochastische Policies** sind direkt darstellbar (wichtig bei POMDPs und für Exploration).
* Konvergenz ist glatter: kleine Parameteränderung → kleine Verhaltensänderung. Bei Q-Learning kann eine winzige Wertänderung das $\arg\max$ komplett umklappen.

**Nachteile:** meist **on-policy** (Daten nach jedem Update wertlos → dateningierig) und hohe Gradientenvarianz.

## 2. REINFORCE (Policy Gradient Theorem)
Ziel ist der erwartete Return $J(\boldsymbol\theta) = \mathbb E_{\tau\sim\pi_{\boldsymbol\theta}}[r(\tau)]$. Der Trick ("log-derivative trick") liefert einen Gradienten, der **nur die Policy** ableitet — die unbekannte Umgebungsdynamik fällt heraus:

$$\nabla_{\boldsymbol\theta} J = \mathbb E\left[\ \sum_{t} \nabla_{\boldsymbol\theta} \log \pi(a_t\mid s_t,\boldsymbol\theta)\ \cdot\ G_t \ \right]$$

**Interpretation:** $\nabla\log\pi$ zeigt in die Richtung, die die *ausgeführte* Aktion wahrscheinlicher macht. Multipliziert mit dem Return $G_t$ heißt das:

> War die Episode gut ($G_t>0$), mach alles Getane wahrscheinlicher. War sie schlecht, mach es unwahrscheinlicher.

Das ist plausibel, aber grob: Auch die schlechten Aktionen einer guten Episode werden verstärkt. Nur im Erwartungswert über viele Episoden stimmt es.

### Varianzreduktion
1. **Causality / Reward-to-go:** Eine Aktion kann nur Rewards *nach* ihr beeinflussen → nur $G_t$ ab $t$ verwenden, nicht den Return der ganzen Episode.
2. **Baseline:** Von $G_t$ eine zustandsabhängige Konstante $b(s_t)$ abziehen. Das ist **unverzerrt** (der Erwartungswert ändert sich nicht), senkt aber die Varianz massiv:
$$\nabla_{\boldsymbol\theta} J = \mathbb E\Big[\sum_t \nabla\log\pi(a_t|s_t,\boldsymbol\theta)\,\big(G_t - b(s_t)\big)\Big]$$

Die beste Baseline ist der **State Value** $v[s_t]$ — und $G_t - v[s_t]$ ist genau die Schätzung des **Advantage**. Damit ist man beim Actor-Critic.

## 3. Actor-Critic
Zwei Netze (oder zwei Köpfe auf einem gemeinsamen Rumpf):

| Netz | Aufgabe | Output |
| :--- | :--- | :--- |
| **Actor** $\pi(a\|s,\boldsymbol\theta)$ | *handeln* | Aktionsverteilung |
| **Critic** $v[s,\boldsymbol\phi]$ (oder $q$) | *bewerten* | Skalar |

Der Critic ersetzt den verrauschten Monte-Carlo-Return durch eine gelernte Schätzung. Das tauscht **Varianz gegen Bias** — genau der TD-Kompromiss aus [[Temporal Difference Learning und Q-Learning]]:

$$A_t \approx r_{t+1} + \gamma\, v[s_{t+1},\boldsymbol\phi] - v[s_t,\boldsymbol\phi] \;=\; \delta_t \quad (\text{TD-Error!})$$

### GAE — Generalized Advantage Estimation
Statt sich zwischen 1-Step (viel Bias) und Monte-Carlo (viel Varianz) zu entscheiden, mittelt GAE exponentiell über alle n-Step-Schätzer:

$$\hat A_t^{GAE} = \sum_{l=0}^{\infty} (\gamma\lambda)^{l}\,\delta_{t+l}$$

$\lambda = 0$ → reines TD (Bias), $\lambda = 1$ → Monte-Carlo (Varianz). Praxiswert: $\lambda \approx 0.95$ (SB3: `gae_lambda`).

## 4. Die Loss-Funktion in der Praxis
Was PPO/A2C tatsächlich minimieren, sind drei Terme:

$$L = \underbrace{L_{\text{policy}}}_{\text{Actor}} + c_1 \underbrace{L_{\text{value}}}_{\text{Critic, MSE}} - c_2 \underbrace{H[\pi]}_{\text{Entropie-Bonus}}$$

Der **Entropie-Bonus** belohnt eine "breite" Aktionsverteilung und verhindert, dass die Policy zu früh kollabiert (in SB3: `ent_coef`). [[SAC (Soft Actor-Critic)|SAC]] treibt diese Idee auf die Spitze und macht die Entropie zum Teil des Reward-Ziels ("maximum entropy RL").

## 5. Das Schrittweiten-Problem → PPO
Ein zu großer Policy-Update zerstört die Policy, und weil die Daten von der alten Policy stammen, sammelt der Agent danach nur noch Müll — anders als beim [[Supervised Learning]] gibt es keinen festen Datensatz, zu dem man zurückkann.

**Lösung (TRPO/[[PPO (Proximal Policy Optimization)|PPO]]):** Update im *Verhaltensraum* begrenzen. PPO nutzt das Wahrscheinlichkeitsverhältnis $\rho_t = \frac{\pi_{neu}(a_t|s_t)}{\pi_{alt}(a_t|s_t)}$ und clippt es:

$$L^{CLIP} = \mathbb E\Big[\min\big(\rho_t \hat A_t,\ \text{clip}(\rho_t, 1-\varepsilon, 1+\varepsilon)\,\hat A_t\big)\Big], \quad \varepsilon \approx 0.2$$

Das Clipping entfernt den Anreiz, sich weit von der alten Policy zu entfernen — und erlaubt dadurch, dieselben Daten für mehrere Epochen wiederzuverwenden (`n_epochs`).

## 6. Landkarte der Verfahren
| Verfahren | Typ | Aktionsraum | Sample-Effizienz |
| :--- | :--- | :--- | :--- |
| REINFORCE | on-policy, policy-based | beide | sehr gering |
| [[A2C (Advantage Actor Critic)]] | on-policy, actor-critic | beide | gering |
| [[PPO (Proximal Policy Optimization)]] | on-policy, actor-critic | beide | mittel |
| [[DDPG (Deep Deterministic Policy Gradient)]] / [[TD3 (Twin Delayed DDPG)]] | off-policy, deterministisch | kontinuierlich | hoch |
| [[SAC (Soft Actor-Critic)]] | off-policy, max-entropy | kontinuierlich | sehr hoch |
| [[DQN (Deep Q-Network)]] / [[QR-DQN (Quantile Regression DQN)]] | off-policy, value-based | diskret | sehr hoch |

**Faustregel für teure Simulationen (FMU!):** Jeder Umgebungsschritt kostet Rechenzeit → off-policy mit Replay Buffer bevorzugen. On-policy PPO ist robuster einzustellen, braucht aber ein Vielfaches an Schritten. Siehe [[SB3 Algorithmen Übersicht]].

## 7. Offline RL (Kapitel 19.7)
Wenn gar keine Interaktion möglich ist und nur ein fester Datensatz vorliegt (z. B. Betriebsdaten einer realen Anlage), spricht man von **Offline RL**. Hauptproblem: **Distribution Shift** — das Netz extrapoliert Q-Werte für Aktionen, die im Datensatz nie vorkamen, und überschätzt sie systematisch. Gegenmittel sind Verfahren, die die gelernte Policy an die Daten fesseln (CQL, BCQ) oder das Problem als Sequenzmodellierung auffassen (**Decision Transformer**, siehe [[Transformer]]).

---
**Links:** [[Markov Decision Process]], [[Bellman Gleichung und Dynamic Programming]], [[Temporal Difference Learning und Q-Learning]], [[PPO (Proximal Policy Optimization)]], [[SAC (Soft Actor-Critic)]], [[Reinforcement Learning]]
