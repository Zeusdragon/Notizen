---
title: "Zweipunktregelung (On/Off)"
type: vorlesung
tags:
  - Zweipunkt
  - Hysterese
erstellt: 2026-05-05
---

**Vorlesung**: [[192 Regelung von Wärmepumpen]]
**Thema**: [[Verdichterregelung]]

---

# Zweipunktregelung (On/Off)

Die einfachste Form der Leistungsregelung. Der Verdichter kennt nur zwei Zustände: 0% oder 100%.

## Funktionsweise
Ein Thermostat oder Druckschalter schaltet den Verdichter bei Erreichen eines Grenzwertes ab und bei Überschreiten wieder ein.

### Die Schalthysterese
Um "Flattern" (extrem schnelles An/Aus) zu vermeiden, ist eine Hysterese zwingend nötig.
* **Temperatur sinkt** $\to$ Abschalten bei $T_{soll} - \frac{1}{2} \Delta T_{Hyst}$.
* **Temperatur steigt** $\to$ Einschalten bei $T_{soll} + \frac{1}{2} \Delta T_{Hyst}$.

## Vor- und Nachteile

| Vorteile | Nachteile |
| :--- | :--- |
| Sehr kostengünstig | Hoher Verschleiß (Anlaufströme, Lagerbelastung) |
| Einfache Steuerung | Schlechte Regelgüte (Temperatur schwankt sägezahnförmig) |
| Ölrückführung gesichert (bei 100% Betrieb) | Netzrückwirkungen (Lichtflackern bei Start) |

> [!WARNING] Mindestlaufzeiten
> Um den Motor zu schützen (Kühlung der Wicklung) und Ölwurf zu verhindern, werden oft **Mindest-Stillstandszeiten** und **Mindest-Laufzeiten** in der Steuerung programmiert.