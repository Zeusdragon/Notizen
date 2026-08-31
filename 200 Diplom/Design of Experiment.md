---
title: Design of Experiment – Verdampfergeometrie
type: diplom
status: in-bearbeitung
tags:
  - doe
  - sobol
  - parameterstudie
  - verdampfer
erstellt: 2026-08-31
---

# Design of Experiment – Verdampfergeometrie

**Bezug:** [[DoE Grundlagen]] · [[Sobol-Sequenzen]] · [[Geometrische Parameter]] · [[Parameterstudie Ergebnisse]]

---

## 1. Ziel

Systematisches Screening der **Mikro-Parameter** des Verdampfers, um deren Einfluss auf Reifdynamik, COP/SCOP und Abtauverhalten zu erfassen. Die Makro-Parameter (Tube Length, n Serial/Parallel Tubes) bleiben fix, damit die Leistungsklasse von 10 kW nicht verfälscht wird — siehe [[Geometrische Parameter]].

Ersetzt den früheren OFAT-Sweep, der ausschließlich den Rippenabstand variiert hat und daher weder Wechselwirkungen erfassen noch die Repräsentativität der Baseline prüfen konnte.

## 2. Variierte Parameter

Vier Mikro-Parameter, jeweils mit Baseline aus [[Geometrische Parameter]]:

| Parameter | Baseline | Min | Max |
| --- | --- | --- | --- |
| fin pitch (Rippenabstand) | 2,2e-3 m | | |
| fin thickness | 0,2e-3 m | | |
| SerialTubeDistance | 22e-3 m | | |
| ParallelTubeDistance | 25,4e-3 m | | |

> [!todo] Grenzen eintragen
> Min-/Max-Werte noch ergänzen. Für fin pitch deckte der alte OFAT-Sweep 1,0 – 7,0 mm ab (Baseline 2,2 mm, Charakteristik „um Baseline gut, außerhalb schlecht", Cliff-Anordnung).

## 3. Versuchsplan

Der Plan kombiniert **Space-Filling** (globale Abdeckung) mit **OFAT** (Randverhalten) und einem Referenzpunkt:

| Block | Anzahl | Zweck |
| --- | --- | --- |
| Sobol-Sample über alle 4 Parameter | 64 | Gleichmäßige Abdeckung des Parameterraums, Datenbasis für Sensitivitätsanalyse und Surrogatmodell |
| OFAT: je Parameter auf Min und Max, Rest auf Baseline | 8 (4 × 2) | Randverhalten und Vergleichbarkeit zum bisherigen Vorgehen |
| Referenz (alle Parameter auf Baseline) | 1 | Bezugspunkt für alle relativen Auswertungen |
| **Summe** | **73** | |

### Warum 64 Sobol-Punkte

Sobol-Sequenzen entfalten ihre Gleichmäßigkeits-Eigenschaften am besten bei Zweierpotenzen ($N = 2^m$), hier $2^6$. Dank der progressiven Eigenschaft lässt sich das Sample bei Bedarf auf 128 Punkte verdoppeln, ohne die bereits gerechneten 64 zu verwerfen — Details in [[Sobol-Sequenzen]].

### Warum zusätzlich OFAT

Ein reines Sobol-Sample trifft die Ecken des Parameterraums nicht zuverlässig. Die 8 OFAT-Läufe sichern das Verhalten an den Intervallgrenzen explizit ab und bleiben zum früheren Rippenabstand-Sweep anschlussfähig.

## 4. Auswertung

* Zielgrößen: COP, SCOP, Zykluszeit, Abtauvorgänge pro Tag, Eismasse
* Sobol-Indizes zur formalen Identifikation der einflussreichsten Parameter
* Antwortfläche über [[Gaussian Process]]-Surrogatmodell
* Auswahl der drei Archetypen (**Baseline, Frost-Falle, Dauerläufer**) für die Zero-Shot-Transfer-Auswertung — datenbasiert statt intuitiv

Ergebnisse des ersten Durchlaufs: [[Parameterstudie Ergebnisse]]

## 5. Offene Punkte

- [ ] Min-/Max-Grenzen der vier Parameter festlegen (siehe [[Plan für Arbeit]], Schritt 5)
- [ ] Prüfen, ob finThickness laut Run 1 tatsächlich einflusslos ist oder nur im gewählten Intervall zu schmal variiert wurde
- [ ] Entscheiden, ob auf 128 Sobol-Punkte verdoppelt wird
