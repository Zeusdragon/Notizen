---
title: "Bellman Gleichung und Dynamic Programming"
type: konzept
status: fertig
tags:
  - reinforcement-learning
  - deep-learning
  - ai
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 19.2 (Expected return, Bellman equations, dynamic programming)

---

## 1. Die zwei Wertfunktionen
Um eine Policy zu verbessern, muss man sie bewerten können. Dafür gibt es zwei Größen — beide sind **erwartete Returns**, siehe [[Markov Decision Process]].

### State Value $v[s_t|\pi]$
"Wie gut ist es, im Zustand $s_t$ zu sein, wenn ich danach Policy $\pi$ folge?"

$$v[s_t \mid \pi] = \mathbb E\!\left[\; \sum_{k=0}^{\infty} \gamma^{k} r_{t+k+1} \;\middle|\; s_t, \pi \right]$$

### Action Value / **Q-Wert** $q[s_t, a_t|\pi]$
"Wie gut ist es, im Zustand $s_t$ die Aktion $a_t$ zu wählen und *danach* $\pi$ zu folgen?"

$$q[s_t, a_t \mid \pi] = \mathbb E\!\left[\; \sum_{k=0}^{\infty} \gamma^{k} r_{t+k+1} \;\middle|\; s_t, a_t, \pi \right]$$

**Der entscheidende Unterschied:** Der Q-Wert erlaubt eine Entscheidung *ohne Modell der Umgebung*. Kenne ich $q$, wähle ich einfach

$$a^{*} = \arg\max_{a} q[s,a]$$

Kenne ich nur $v$, müsste ich wissen, in welchem Zustand jede Aktion landet — also die Transition $Pr(s'|s,a)$ kennen. Deshalb ist der **Q-Wert die zentrale Größe im modellfreien RL** ([[Temporal Difference Learning und Q-Learning]], DQN).

### Zusammenhang
$$v[s\mid\pi] = \sum_{a} \pi(a\mid s)\, q[s,a\mid\pi] \qquad\qquad v[s\mid\pi^*] = \max_a q[s,a\mid\pi^*]$$

Und die Differenz beider heißt **Advantage** — die Kerngröße von [[Policy Gradient und Actor Critic]]:

$$A[s,a] = q[s,a] - v[s]$$

> *"War diese Aktion besser oder schlechter als das, was ich in diesem Zustand im Schnitt sowieso mache?"* Ein positiver Advantage bedeutet: Aktion häufiger wählen.

## 2. Die Bellman-Gleichungen
Sie folgen direkt aus der Rekursion $G_t = r_{t+1} + \gamma G_{t+1}$: Der Wert eines Zustands ist der sofortige Reward plus der abdiskontierte Wert des Folgezustands.

**Bellman für $v$:**
$$v[s_t\mid\pi] = \sum_{a}\pi(a\mid s_t)\sum_{s_{t+1}} Pr(s_{t+1}\mid s_t,a)\Big[\, r(s_t,a) + \gamma\, v[s_{t+1}\mid\pi] \,\Big]$$

**Bellman für $q$:**
$$q[s_t,a_t\mid\pi] = r(s_t,a_t) + \gamma \sum_{s_{t+1}} Pr(s_{t+1}\mid s_t,a_t)\sum_{a_{t+1}} \pi(a_{t+1}\mid s_{t+1})\, q[s_{t+1},a_{t+1}\mid\pi]$$

**Bellman-Optimalitätsgleichung** (für die beste Policy — Erwartungswert über $\pi$ wird zum $\max$):

$$q^{*}[s_t,a_t] = r(s_t,a_t) + \gamma\, \mathbb E_{s_{t+1}}\Big[\ \max_{a_{t+1}} q^{*}[s_{t+1},a_{t+1}]\ \Big]$$

Diese eine Zeile ist der Kern von Q-Learning und DQN. Alles Weitere ist die Frage, wie man sie ohne Kenntnis von $Pr(s_{t+1}|s_t,a_t)$ näherungsweise löst.

## 3. Dynamic Programming (Modell bekannt)
Kennt man Transitionen und Rewards *exakt* und ist der Zustandsraum klein und diskret, kann man optimal lösen — ohne Neuronale Netze, ohne Sampling.

### Policy Iteration
Zwei Schritte im Wechsel bis zur Konvergenz:
1. **Policy Evaluation:** Bellman-Gleichung für die aktuelle Policy iterativ anwenden, bis $v$ stabil ist.
2. **Policy Improvement:** In jedem Zustand gierig die Aktion mit dem besten $q$ nehmen.

Garantiert konvergent zur optimalen Policy.

### Value Iteration
Beides zusammenziehen — nur die Optimalitätsgleichung als Update-Regel iterieren:

$$v_{k+1}[s] \leftarrow \max_{a}\Big[ r(s,a) + \gamma \sum_{s'} Pr(s'\mid s,a)\, v_k[s'] \Big]$$

Am Ende einmal gierig ablesen → optimale Policy.

## 4. Warum reicht das in der Praxis nicht?
| Problem                      | Konsequenz                                                                                                                                       |
| :--------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| **Modell unbekannt**         | $Pr(s's,a)$ ist bei realen Anlagen/FMUs nicht in geschlossener Form verfügbar → modellfreie Methoden nötig                                       |
| **Curse of Dimensionality**  | Bei $n$ kontinuierlichen Sensorwerten explodiert die Tabelle. Ein diskretisierter 8D-Zustand mit 20 Stufen hat $20^8 = 2.6\cdot10^{10}$ Einträge |
| **Kontinuierliche Aktionen** | $\max_a$ über einen kontinuierlichen Raum ist selbst ein Optimierungsproblem                                                                     |


**Antwort des Deep Learning:** Ersetze die Tabelle durch ein Neuronales Netz $q[s,a,\boldsymbol\phi]$ ([[Deep Neural Network]]) und schätze die Erwartungswerte durch Stichproben aus echten Rollouts → [[Temporal Difference Learning und Q-Learning]].

## 5. Verwandtschaft zu MPC
[[Model Predictive Control]] löst dasselbe Optimierungsproblem — nur **online und über einen endlichen Horizont**, mit explizitem Modell. RL löst es **offline, über den unendlichen Horizont**, und speichert die Lösung in Netzgewichten.

| | Dynamic Programming / RL | MPC |
| :--- | :--- | :--- |
| Horizont | unendlich (via $\gamma$) | endlich ($N$ Schritte) |
| Wann gerechnet | vorab (Training) | zur Laufzeit, jeden Takt |
| Modell | nicht nötig (model-free) | zwingend |
| Ergebnis | Policy / Q-Funktion | eine Stellgrößenfolge |

Die Value-Funktion in RL ist exakt das, was ein MPC als **Terminal Cost** bräuchte, um den abgeschnittenen Horizont zu kompensieren — deshalb kombiniert man beides gerne.

---
**Links:** [[Markov Decision Process]], [[Temporal Difference Learning und Q-Learning]], [[Policy Gradient und Actor Critic]], [[Reinforcement Learning]], [[Model Predictive Control]]
