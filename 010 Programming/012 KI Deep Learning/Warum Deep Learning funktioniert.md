---
title: "Warum Deep Learning funktioniert"
type: konzept
status: fertig
tags:
  - deep-learning
  - theorie
  - ai
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 20 (Why does deep learning work?) und 21 (Ethics)

---

## 1. Die offene Frage
Deep Learning funktioniert besser, als die klassische Lerntheorie erlaubt. Zwei Dinge sollten eigentlich schiefgehen:

1. **Das Training** — die Loss-Fläche ist nicht konvex und hochdimensional. Gradientenabstieg müsste in schlechten lokalen Minima hängenbleiben. Tut er aber praktisch nie.
2. **Die Generalisierung** — Netze haben oft mehr Parameter als Datenpunkte und könnten die Trainingsdaten auswendig lernen (können sie auch: sie passen sogar zufällige Labels perfekt an). Trotzdem generalisieren sie bei echten Labels gut.

Prince ist hier bewusst ehrlich: Es gibt Teilerklärungen, aber keine geschlossene Theorie.

## 2. Warum das Training gelingt
* **Überparametrisierung glättet die Loss-Fläche.** In sehr hohen Dimensionen sind echte lokale Minima selten — die meisten kritischen Punkte sind **Sattelpunkte**, aus denen SGD entkommt. Und die vorhandenen Minima liegen fast alle auf ähnlichem Loss-Niveau.
* **Verbundene Minima:** Gefundene Lösungen sind oft durch flache Pfade miteinander verbunden ("mode connectivity") — es gibt kein einzelnes, schwer zu treffendes Optimum.
* **Lottery Ticket Hypothesis:** Ein großes Netz enthält per Zufall Teilnetze mit besonders günstiger Initialisierung; das Training verstärkt im Wesentlichen diese.

## 3. Warum es generalisiert
* **Impliziter Bias des Optimierers:** SGD wählt unter allen perfekt passenden Lösungen bevorzugt **flache Minima** mit kleiner Gradienten-/Gewichtsnorm — also die "einfachsten" ([[Regularisierung]]).
* **Induktiver Bias der Architektur:** Faltungen kodieren Translationsinvarianz, Attention Permutationsäquivarianz, Tiefe eine hierarchische Kompositionsstruktur. Reale Daten haben genau diese Struktur.
* **Passung zur Datenstruktur:** Bilder, Sprache und physikalische Prozesse sind hierarchisch und lokal aufgebaut — dieselbe Struktur, die tiefe Netze effizient darstellen können.
* **Double Descent** zeigt empirisch, dass mehr Kapazität jenseits des Interpolationspunkts wieder hilft ([[Generalisierung und Double Descent]]).

## 4. Was noch ungeklärt ist
* Warum genau ist ein flaches Minimum besser? (Der Begriff "flach" ist nicht einmal reparametrisierungsinvariant.)
* Wie viele Daten braucht eine Aufgabe? Es gibt keine brauchbare Vorhersage.
* Warum funktionieren manche Architekturen für manche Aufgaben so viel besser?
* Warum sind Netze anfällig für **adversarial examples** — winzige, unsichtbare Störungen, die die Vorhersage kippen?

## 5. Praktische Konsequenzen
Aus dem Kapitel ergeben sich ein paar nüchterne Arbeitsregeln:
* **Empirie schlägt Theorie.** Hyperparameter werden gesucht, nicht hergeleitet.
* **Zu klein anfangen ist ein häufigerer Fehler als zu groß.** Erst Kapazität schaffen, dann regularisieren.
* **Der induktive Bias ist die stärkste Stellschraube** — die Wahl der Architektur und der Zustandsrepräsentation bringt meist mehr als jede Optimierer-Feinjustage.
* **Interpolation ist nicht Extrapolation.** Netze sind innerhalb der Trainingsverteilung stark und außerhalb unzuverlässig. Für sicherheitskritische Regelung heißt das: Die Trainingsverteilung muss den Betriebsbereich abdecken, sonst bleibt eine harte Absicherung nötig ([[Model Predictive Control]], klassische Grenzwertlogik).

## 6. Ethik (Kapitel 21)
Prince widmet das Schlusskapitel bewusst den nichttechnischen Folgen: Bias in Trainingsdaten, mangelnde Erklärbarkeit, Datenschutz, Konzentration von Rechenmacht, Energieverbrauch, Automatisierung von Arbeit. Für technische Anwendungen ist vor allem die **Erklärbarkeit** relevant: Eine gelernte Policy, die eine reale Anlage steuert, muss nachvollziehbar und in ihren Grenzen abgesichert sein — sonst ist sie nicht betriebsfähig, egal wie gut der Reward aussieht.

---
**Links:** [[Generalisierung und Double Descent]], [[Regularisierung]], [[Deep Neural Network]], [[Gradient Descent und Optimierer]]
