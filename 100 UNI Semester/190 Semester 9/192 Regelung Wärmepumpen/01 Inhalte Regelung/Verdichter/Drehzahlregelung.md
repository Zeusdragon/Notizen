---
title: "Drehzahlregelung (Inverter)"
type: vorlesung
tags:
  - Inverter
  - Frequenzumrichter
  - Effizienz
erstellt: 2026-05-05
---

**Vorlesung**: [[192 Regelung von Wärmepumpen]]
**Thema**: [[Verdichterregelung]]

---

# Drehzahlregelung (Inverter)

Die Drehzahlregelung gilt als der "Goldstandard" für Teillasteffizienz. Über einen Frequenzumrichter (FU) wird die Frequenz $f$ des Motors variiert.

$$\dot{V}_{geo} \sim n \sim f$$

## Betriebsverhalten
* **Regelbereich:** Typisch 30 Hz ... 90 Hz (je nach Modell auch 20 ... 120 Hz).
* **Proportionalität:** Kälteleistung $\dot{Q}_0$ und elektrische Leistungsaufnahme $P_{el}$ sinken bei reduzierter Drehzahl.
* **Über-Synchroner Betrieb:** Verdichter können kurzzeitig über 50 Hz (Nennfrequenz) betrieben werden ("Boost-Mode"), um Spitzenlasten oder schnelles Abkühlen (Pull-Down) zu ermöglichen.

## Technische Herausforderungen

### 1. Ölschmierung
Bei sehr niedrigen Drehzahlen fördert die interne Ölpumpe (oder Schleuderscheibe) eventuell nicht genug Öl. Zudem sinkt die Gasgeschwindigkeit in den Steigleitungen, sodass Öl nicht mehr zum Verdichter zurücktransportiert wird.
* *Lösung:* Ölabscheider verwenden oder periodische "Oil-Return-Cycles" (kurzzeitiges Hochfahren der Drehzahl).

### 2. Resonanzen
Jeder Verdichter hat mechanische Eigenfrequenzen. Werden diese durchfahren, entstehen Vibrationen und Lärm.
* *Lösung:* Ausblenden kritischer Frequenzbänder im Frequenzumrichter ("Frequency Skipping").

## Vergleich: Inverter vs. On/Off
Der Inverter erzielt einen deutlich besseren **SEER (Seasonal Energy Efficiency Ratio)**, da die Wärmetauscher im Teillastbetrieb (wenig Massenstrom, große Fläche) thermodynamisch effizienter arbeiten (kleineres $\Delta T$).