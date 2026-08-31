---
title: "Heißgasbypass-Regelung"
type: vorlesung
tags:
  - Bypass
  - Leistungsregelung
  - Ineffizienz
erstellt: 2026-05-05
---

**Vorlesung**: [[192 Regelung von Wärmepumpen]]
**Thema**: [[Verdichterregelung]]

---

# Heißgasbypass-Regelung

Eine Methode zur Leistungsanpassung durch "künstliche Last".

## Funktionsweise
Ein Regelventil öffnet eine Verbindung zwischen der Hochdruckseite (Druckleitung) und der Niederdruckseite (Saugleitung). Heißes Gas strömt direkt zurück vor den Verdichter, ohne durch den Verdampfer zu gehen.

* Der Verdichter "denkt", er müsste volle Last fördern.
* Die Kälteleistung am Verdampfer sinkt (da weniger Kältemittel dort ankommt), aber der Verdichter läuft weiter stabil.

## Bewertung
* **Energetik:** Katastrophal. Die aufgewendete Verdichterarbeit wird vernichtet (Drosselung). Der COP sinkt drastisch gegen Null.
* **Einsatzgebiet:**
    * Wenn Inverter technisch nicht möglich sind.
    * Um Mindest-Massenströme zu sichern.
    * Bei sehr genauen Temperaturanforderungen (stetige Regelung ohne Inverter-Kosten).
    * Als **Anlaufentlastung** (Start gegen geringeren Druck).

> [!TIP] Quench-Ventil
> Da das Heißgas den Verdichter überhitzen könnte, wird oft zusätzlich flüssiges Kältemittel eingespritzt ("Nacheinspritzung"), um das Sauggas zu kühlen.