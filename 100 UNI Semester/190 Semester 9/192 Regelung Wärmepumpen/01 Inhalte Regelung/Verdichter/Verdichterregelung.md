---
title: "04 Verdichterregelung"
type: vorlesung
tags:
  - Verdichter
  - Regelung
  - Teillast
  - Inverter
erstellt: 2026-05-05
---

**Vorlesung**: [[192 Regelung von Wärmepumpen]]
**Datum**: 2026-01-08

![[RKA_04_VO_Verdichterregelung.pdf]]

---

# 04 Verdichterregelung

> [!INFO] Zusammenfassung
> Die Verdichterregelung dient der Anpassung der Kälteleistung an den aktuellen Bedarf (Teillast). Da Kälteanlagen zu 98% im Teillastbetrieb laufen, ist die Wahl der Regelungsmethode entscheidend für die **Energieeffizienz** (EER/SEER) und die **Temperaturstabilität**.

## 1. Motivation und Regelgröße
Das Ziel ist es, den Massenstrom $\dot{m}_K$ so zu regeln, dass die Kälteleistung $\dot{Q}_0$ dem Bedarf entspricht.

* **Führungsgröße:** Meist der **Saugdruck** $p_0$ (in Verbundanlagen) oder die **Kaltwasseraustrittstemperatur** (bei Chiller).
* **Herausforderung:** Takten vermeiden, Schmierfilm sicherstellen, Resonanzen vermeiden.

## 2. Methoden der Leistungsregelung
Man unterscheidet prinzipiell zwischen systemseitigen und verdichtereigenen Methoden.

### A. Systemseitige Regelung (Externe Eingriffe)
Diese Methoden funktionieren unabhängig von der Verdichterbauart.
1.  **[[Zweipunktregelung]]**: Der Klassiker (An/Aus). Einfach, aber grob.
2.  **[[Drehzahlregelung]]**: Die effizienteste Methode (Inverter). Passt den Volumenstrom stetig an.
3.  **[[Heißgasbypass]]**: Energetisch ungünstig (Vernichtung von Exergie), aber billig und präzise für Schwachlast.

### B. Bauartspezifische Regelung (Interne Mechanik)
Hier wird die Geometrie oder Arbeitsweise des Verdichters mechanisch verändert. Details siehe:
👉 **[[Bauartspezifische Leistungsregelung]]**

* **Hubkolben:** Zylinderabschaltung (Stufenweise).
* **Schraube:** Regelschieber (Veränderung des Flanken-Eingriffs).
* **Scroll:** Digital Scroll (PWM-Prinzip durch "Abheben").
* **Turbo:** Vordrallregelung (IGV - Inlet Guide Vanes).

## 3. Anlaufverhalten (Entlasteter Anlauf)
Große Verdichter können nicht gegen hohen Differenzdruck ($\Delta p$) anlaufen (Motorstrom wäre zu hoch).
* **Lösung:** Bypass zwischen Hoch- und Niederdruckseite beim Start oder mechanisches Offenhalten der Saugventile, bis Nenndrehzahl erreicht ist.