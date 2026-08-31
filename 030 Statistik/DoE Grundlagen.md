---
title: "Design of Experiments (DoE) — Grundlagen"
type: konzept
status: fertig
tags:
  - statistik
  - doe
  - sampling
  - methodik
erstellt: 2026-08-11
---

# Design of Experiments (DoE) — Grundlagen

**Bezug:** [[Sobol-Sequenzen]] · [[Gaussian Process]] · [[200 Diplom/Design of Experiment]] · [[200 Diplom/Geometrische Parameter]]

---

## 1. Ziel von DoE

Design of Experiments = systematische Planung, **welche** Kombinationen von Eingangsparametern man simuliert/testet, um mit möglichst wenigen (teuren) Versuchen möglichst viel valide Information über das Systemverhalten zu bekommen. Gegenteil von "einfach mal ein paar Werte durchprobieren".

Zentrale Begriffe:

* **Faktor** = Eingangsparameter (z.B. Rippenabstand, finThickness)
* **Level/Stufe** = konkreter Wert, den ein Faktor annehmen kann
* **Response** = Zielgröße, die man misst/simuliert (z.B. SCOP, COP, Abtauzeit)
* **Design Space** = der gesamte zulässige Parameterraum (alle Faktoren × ihre Grenzen)

## 2. One-Factor-at-a-Time (OFAT)

Ein Parameter wird variiert, alle anderen bleiben auf ihrem Baseline-Wert fixiert. Das ist genau das aktuelle Vorgehen in [[200 Diplom/Design of Experiment]] (nur der Rippenabstand wird durchgesweept).

* ✅ Einfach, günstig, leicht interpretierbar ("um Baseline gut, außerhalb schlecht")
* ❌ Erfasst **keine Interaktionen** zwischen Parametern (wirkt sich Rippenabstand vielleicht anders aus, je nach Tube Length?)
* ❌ Man weiß nicht, ob die Baseline-Werte der *anderen* Parameter überhaupt repräsentativ sind — das Ergebnis gilt nur exakt "um diesen einen Punkt herum"

## 3. Full Factorial Design

Alle Kombinationen aller Level aller Faktoren werden simuliert.

* ✅ Erfasst Interaktionen vollständig
* ❌ Anzahl Versuche explodiert: bei $k$ Stufen und $d$ Faktoren sind es $k^d$ Simulationen. Schon 4 Parameter mit je 5 Stufen ergeben 625 Simulationen — bei rechenintensiven Dymola/FMU-Läufen meist nicht praktikabel.

## 4. Screening-Designs (Fractional Factorial, Morris-Methode, Plackett-Burman)

Reduzierte Teilmengen des vollen Faktoriells, mit denen man mit wenigen Versuchen grob herausfindet, **welche** Faktoren überhaupt relevant sind — bevor man Rechenzeit in die genaue Vermessung von Interaktionen steckt. Sinnvoll als erster, günstiger Schritt bei vielen Kandidaten-Parametern.

## 5. Space-Filling Designs (Standard für Computer-Experimente)

Für **deterministische Simulationen ohne Messrauschen** (wie Dymola/FMU — dieselben Eingaben liefern immer dieselbe Ausgabe) ist die klassische, auf Messrauschen und Replikaten aufbauende DoE-Theorie (Fisher, für physische Experimente) nicht das richtige Werkzeug. Stattdessen will man den Parameterraum einfach **möglichst gleichmäßig abdecken**:

* **Latin Hypercube Sampling (LHS):** Der Wertebereich jedes Parameters wird in $N$ gleich große Intervalle geteilt, aus jedem Intervall wird zufällig genau ein Wert gezogen, die Werte der einzelnen Parameter werden dann zufällig kombiniert. Ergebnis: gute Abdeckung, kein Klumpen wie bei reinem Zufall, aber immer noch mit einer Zufallskomponente.
* **Sobol-Sequenzen:** deterministisch, noch gleichmäßiger als LHS, beliebig erweiterbar ohne bisherige Punkte zu verwerfen. Details: [[Sobol-Sequenzen]].
* Beide eignen sich sehr gut, um anschließend ein Surrogatmodell (z.B. [[Gaussian Process]]) über den gesamten Parameterraum zu fitten — im Gegensatz zu OFAT, das nur entlang einzelner Achsen Information liefert.

## 6. Welche Methode passt wann?

| Situation | Empfehlung |
|---|---|
| Physisches Experiment, Messrauschen, wenige Faktoren | klassisches (fractional) factorial Design mit Replikaten & Randomisierung |
| Sehr viele Kandidaten-Faktoren, erstmal grob filtern | Screening-Design (z.B. Morris) |
| Teure, deterministische Simulation (Dymola/FMU), Ziel = Surrogat-/Metamodell | Space-Filling Design (LHS oder Sobol) + [[Gaussian Process]] |

## 7. Bezug zur Diplomarbeit

Aktuell: OFAT-Sweep nur für Rippenabstand ([[200 Diplom/Design of Experiment]]), Makro-Parameter fix, um die Leistungsklasse (10kW) nicht zu verfälschen — das ist weiterhin sinnvoll, um Bauraum/Leistung von Reifdynamik-Effekten zu trennen (siehe [[200 Diplom/Arbeitsstand]] Abschnitt 1.4).

Für die geplante Zero-Shot-Transfer-Auswertung (Kapitel 5 & 6, Auswahl von 3 Archetypen "Baseline / Frost-Falle / Dauerläufer") lohnt sich aber ein systematisches, gemeinsames Sampling **aller** relevanten Mikro-Parameter (finPitch, finThickness, evtl. Tube-/ParallelTubeDistance) statt nacheinander:

1. Grenzen je Mikro-Parameter festlegen (siehe [[200 Diplom/Geometrische Parameter]])
2. Sobol- oder LHS-Sample über den kombinierten Parameterraum ziehen (z.B. 32–64 Punkte)
3. Für jeden Punkt eine FMU parametrieren und simulieren, Zielgrößen (SCOP, Abtauzyklen, Abtauzeit) extrahieren
4. Auf den Ergebnissen ein [[Gaussian Process]]-Surrogat fitten → daraus die Archetypen ableiten und eine formale Sensitivitätsanalyse (Sobol-Indizes) rechnen, statt die Extrempunkte nur visuell aus der OFAT-Tabelle abzulesen

Konkretes Vorgehen inkl. Python-Werkzeugen: siehe [[Gaussian Process]] Abschnitt "Anwendung für die Diplomarbeit".
