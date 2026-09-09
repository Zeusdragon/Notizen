---
title: "Meeting 1"
type: meeting
status: in-bearbeitung
tags:
  - meeting
erstellt: 2026-09-09
---

# Meeting 1

**Datum**: 10.09.2026
**Teilnehmer**: Parssa Feizi, Diandra, Tim Rudzik
**Bezug**: [[Dymola-Modell]], [[Geometrische Parameter]], [[Parameterstudie Ergebnisse]]

---

## 1. Fortschritte

### Erledigt seit dem letzten Termin
- Simulationsmodell vom Start model Erweitert und angepasst 
- Erste tests mit mehreren FMU Modellen 
- Erste Trainingsdurchgänge durch mit Stabilen aber vereinfachten modell ohne Haaf, Steiner und thermal resistance Frostmodell
- Geometrievarianten auf 4 Parameter begrenzt und über die 4 Parameter Sampling per Sobol auf 64 varianten + 8 One at the time + referenz [[Parameterstudie Ergebnisse]]
- Auslegung der Dimensionen der Komponenten anhand von echten ausgelegten Komponenten 
- Unterkühlungsregelung nachgeschaut, Papaer der Purdue University hatte das mal erprobt. Allgemein macht die Schaltung schon mehr sinn als Überhitzung da sowieso immer ein abscheider/Akkumulator für den Abtauubetrieb nötig ist . 
- Abtauubetrieb hat Konstante Betriebspunkte keine PI Regelung wärhend Abtauung
- Kleiner Test gemacht ob es optimale Verdampfergeometrie für Anlage gibt und ob sie für alle regler gleich ist oder Reglerabhängig   

### In Arbeit
- Stabilität von Modell verbessern
- Fine Tuning vom Training für bessere Performance und Stabilität

### Probleme / Blocker
- Simulations Modell leicht instabil während Abtauung im Hydraulikkreis gerne Warn meldung das Kondensator zellen unter 250K gehen und ExpansionTank nicht mehr weiter macht
- Simulations durch einführen von Haaf und Steiner Wärmeübergang und Frostmodell mit thermal Resistance langsamer und instabiler geworden

---

## 2. Fragen

> [!question] F1 — Nutzung von KI Dokumentation
> Wie soll arbeit mit KI dokumentiert werden. Aus Startepaket nicht ganz ersichtlich wie das gehandhabt werden soll. Da dort ausgegangen wird das ich die Komplette Arbeit einmal reingebe und dann korrigieren lasse ist aber meist eher eine disskusion über kleinere Abteile

> [!check] Antwort
> 

> [!question] F2 — Datashare für Anlegen?
> Für Dokumente und Abbildungs austausch einen Datashare Anlegen?
> 

> [!check] Antwort
> 

> [!question] F3 — Welches Frostmodell genau verwendung am sinnvollsten 
> Haaf und Steiner notwendig oder Konstanter WÜ auch fein haaf und Steiner halt geometriebasierte Wärmeübergänge wäre schon wichtig ansonsten wäre nur ein unterschied im sinne der Strömungsöffnunf und Abtaudynamik durch zufrieren der öffnunfg sowie Verdampferfläche.
> 

> [!check] Antwort
> 

---

## 3. Sonstige Notizen

-

---

## 4. Ergebnis & nächste Schritte

- 
- 


