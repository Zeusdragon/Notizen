---
title: "Temporal Difference Learning und Q-Learning"
type: konzept
status: fertig
tags:
  - reinforcement-learning
  - deep-learning
  - ai
  - value-based
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 19.3 (Tabular RL) und 19.4 (Fitted Q-learning / Deep Q-networks)

---

## 1. Ausgangslage
Die [[Bellman Gleichung und Dynamic Programming|Bellman-Gleichungen]] brauchen das Übergangsmodell $Pr(s'|s,a)$. Im **model-free** Fall hat man das nicht — man hat nur Erfahrung: Tupel $(s_t, a_t, r_{t+1}, s_{t+1})$. Die Frage lautet also: *Wie schätzt man Q-Werte allein aus Stichproben?*

## 2. Monte-Carlo vs. Temporal Difference
### Monte-Carlo (MC)
Episode zu Ende spielen, den tatsächlichen Return $G_t$ ausrechnen, Schätzung dahin ziehen:

$$q[s_t,a_t] \leftarrow q[s_t,a_t] + \alpha\big(G_t - q[s_t,a_t]\big)$$

* **Unverzerrt** (bias-frei), aber **hohe Varianz** — der Return hängt von allen späteren Zufällen ab.
* Braucht abgeschlossene Episoden → bei langen Simulationen unpraktisch.

### Temporal Difference (TD)
Nicht bis zum Ende warten, sondern die **eigene spätere Schätzung** als Ziel benutzen (**Bootstrapping**):

$$q[s_t,a_t] \leftarrow q[s_t,a_t] + \alpha\underbrace{\big(\,\overbrace{r_{t+1} + \gamma\, q[s_{t+1},a_{t+1}]}^{\text{TD-Target}} - q[s_t,a_t]\big)}_{\text{TD-Error }\delta_t}$$

* Lernt nach **jedem einzelnen Schritt**, funktioniert auch bei endlosen Episoden.
* **Niedrige Varianz**, dafür **verzerrt** (bias), solange die eigene Schätzung falsch ist.
* Der **TD-Error** $\delta_t$ ist die zentrale Lerngröße im gesamten RL — er taucht auch im Critic von [[Policy Gradient und Actor Critic]] wieder auf.

> **Merksatz:** MC lernt aus dem Ergebnis, TD lernt aus der *Überraschung*. Der Kompromiss dazwischen heißt **n-step Return** bzw. **TD($\lambda$)** — und in modernen Implementierungen **GAE** (Generalized Advantage Estimation).

## 3. SARSA vs. Q-Learning
Beide sind TD-Verfahren; der Unterschied liegt allein im Target.

|           | **SARSA** (on-policy)                                         | **Q-Learning** (off-policy)               |
| :-------- | :------------------------------------------------------------ | :---------------------------------------- |
| Target    | $r_{t+1} + \gamma\, q[s_{t+1}, a_{t+1}]$                      | $r_{t+1} + \gamma \max_{a} q[s_{t+1}, a]$ |
| $a_{t+1}$ | die **tatsächlich ausgeführte** nächste Aktion                | die **beste denkbare** Aktion             |
| Lernt     | den Wert der *aktuell verhaltenen* Policy (inkl. Exploration) | den Wert der *optimalen* Policy           |
| Verhalten | vorsichtig (berücksichtigt eigene Zufallsfehler)              | risikofreudig / optimistisch              |

Das klassische Beispiel ist die "Cliff Walk"-Umgebung: SARSA läuft mit Sicherheitsabstand an der Klippe entlang, Q-Learning direkt am Rand — weil es so tut, als würde es nie zufällig danebengreifen.

**Off-policy** bedeutet: Die Daten dürfen von einer *anderen* Policy stammen als der, die gelernt wird. Genau das erlaubt einen **Replay Buffer** und macht Q-Learning so dateneffizient.

## 4. Exploration: $\epsilon$-greedy
Reines $\arg\max$ probiert nie etwas Neues aus. Deshalb:

$$a_t = \begin{cases} \arg\max_a q[s_t,a] & \text{mit } 1-\epsilon\ \text{zufällige Aktion} & \text{mit } \epsilon\end{cases}$$

$\epsilon$ wird über das Training abgesenkt (z. B. 1.0 → 0.05). In SB3: `exploration_initial_eps`, `exploration_final_eps`, `exploration_fraction`.

## 5. Vom Tabellen-Q zu Deep Q
### Fitted Q-Learning
Ersetze die Tabelle durch ein Netz $q[s,a,\boldsymbol\phi]$ und mache aus dem TD-Update ein **Regressionsproblem** ([[Loss Functions|Least Squares]]):

$$L(\boldsymbol\phi) = \sum_{i}\Big( \underbrace{r_{i} + \gamma \max_{a} q[s_{i+1}, a, \boldsymbol\phi^{-}]}_{\text{Target (fixiert)}} - q[s_i,a_i,\boldsymbol\phi] \Big)^{2}$$

Das Netz bekommt den Zustand hinein und gibt **einen Q-Wert pro diskreter Aktion** aus — so kostet das $\max$ nur einen Forward-Pass.

### Die "deadly triad"
Funktionsapproximation + Bootstrapping + Off-Policy-Daten zusammen können divergieren. DQN (Mnih et al. 2015) macht das mit zwei Tricks beherrschbar:

1. **Experience Replay:** Erfahrungen in einen Ringpuffer schreiben und daraus **zufällige Batches** ziehen. Bricht die zeitliche Korrelation aufeinanderfolgender Samples auf (die i.i.d.-Annahme von [[Gradient Descent und Optimierer|SGD]]) und nutzt jedes Sample mehrfach.
2. **Target Network:** Eine eingefrorene Kopie $\boldsymbol\phi^{-}$ erzeugt die Targets und wird nur alle $N$ Schritte kopiert. Ohne sie jagt das Netz seinem eigenen, ständig verrutschenden Ziel hinterher.

Dazu: Reward-Clipping, Frame-Stacking (Markov-Eigenschaft herstellen!), $\epsilon$-greedy.

## 6. Bekannte Schwächen und ihre Fixes
| Problem                         | Ursache                                                                                              | Lösung                                                                                                                                              |
| :------------------------------ | :--------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Overestimation Bias**         | $\max$ über verrauschte Schätzungen ist systematisch zu hoch ($\mathbb E[\max] \geq \max \mathbb E$) | **Double DQN:** Aktion mit dem Online-Netz *auswählen*, mit dem Target-Netz *bewerten*                                                              |
| Value und Advantage vermischt   | in vielen Zuständen ist die Aktion egal                                                              | **Dueling DQN:** getrennte Köpfe für $v[s]$ und $A[s,a]$                                                                                            |
| Alle Samples gleich wichtig     | wenige Übergänge sind lehrreich                                                                      | **Prioritized Replay:** Sampling proportional zum TD-Error                                                                                          |
| Nur der Mittelwert wird gelernt | Risiko ist unsichtbar                                                                                | **Distributional RL / [[QR-DQN (Quantile Regression DQN) \| QR-DQN]]** — lernt die ganze Return-Verteilung                                          |
| Nur diskrete Aktionen           | $\max_a$ über kontinuierlichen Raum unlösbar                                                         | Actor lernt das $\arg\max$: [[DDPG (Deep Deterministic Policy Gradient)\|DDPG]], [[TD3 (Twin Delayed DDPG)\|TD3]], [[SAC (Soft Actor-Critic)\|SAC]] |

**Rainbow** ist die Kombination all dieser Erweiterungen.

## 7. Praxis
* Für **diskrete Aktionen** (An/Aus, Stufe 0–3, Abtauung starten/nicht starten) und teure Simulationen ist ein Value-Based-Verfahren die dateneffizienteste Wahl → [[DQN (Deep Q-Network)]], besser noch [[QR-DQN (Quantile Regression DQN)]].
* Q-Werte sind **interpretierbar**: Man kann sie über den Zustandsraum plotten und sieht direkt, ab wann sich z. B. eine Abtauung "lohnt". Das ist bei einer Policy-Gradient-Methode nicht so einfach.
* Typische Fehlerquellen: zu kleiner Replay Buffer, `learning_starts` zu klein, unnormalisierte Zustände (siehe [[Backpropagation und Initialisierung]]), $\gamma$ passt nicht zur Zeitschrittweite.

---
**Links:** [[Markov Decision Process]], [[Bellman Gleichung und Dynamic Programming]], [[Policy Gradient und Actor Critic]], [[DQN (Deep Q-Network)]], [[QR-DQN (Quantile Regression DQN)]], [[Reinforcement Learning]]
