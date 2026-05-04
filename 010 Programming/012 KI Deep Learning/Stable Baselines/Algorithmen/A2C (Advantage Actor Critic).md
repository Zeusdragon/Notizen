---
tags: [ReinforcementLearning, On-Policy, Actor-Critic]
---
# A2C (Advantage Actor Critic)

## Funktionsprinzip
A2C ist die Basis für viele moderne **On-Policy Actor-Critic** Algorithmen und die synchrone Version von A3C.

**Der Kernmechanismus: Die Advantage-Funktion**
A2C nutzt zwei Komponenten:
- Den **Actor**, der die Wahrscheinlichkeit für Aktionen anpasst.
- Den **Critic**, der vorhersagt, wie gut ein Zustand generell ist (Value Function $V(s)$).

Der Clou ist der **Advantage-Wert ($A$)**. Er berechnet sich aus:
`A = (Tatsächliche Belohnung nach Aktion) - (Vom Critic vorhergesagter Wert des Zustands)`
Der Advantage sagt also aus: "War diese Aktion besser oder schlechter als das, was wir ohnehin erwartet haben?"

**Was passiert genau?**
Der Agent sammelt Erfahrungen. Wenn eine Aktion einen positiven Advantage hat (besser als erwartet), erhöht der Actor die Wahrscheinlichkeit, diese Aktion in Zukunft wieder zu wählen. War sie schlechter, wird die Wahrscheinlichkeit gesenkt. Gleichzeitig lernt der Critic, seine Vorhersagen an die Realität anzupassen. Alle parallel laufenden Agenten (Environments) sammeln synchron Daten, bevor ein Netzwerk-Update gemacht wird.