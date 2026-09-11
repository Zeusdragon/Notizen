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
> UNd soll der Code in ein Repo was auf Uni seite gemacht ist oder soll ich eins auf Gitlab Account machen
> 

> [!check] Antwort
> 

> [!question] F3 — Welches Frostmodell genau verwendung am sinnvollsten 
> Haaf und Steiner notwendig oder Konstanter WÜ auch fein haaf und Steiner halt geometriebasierte Wärmeübergänge wäre schon wichtig ansonsten wäre nur ein unterschied im sinne der Strömungsöffnunf und Abtaudynamik durch zufrieren der öffnunfg sowie Verdampferfläche.
> 

> [!check] Antwort
> 


> [!question] F4 — USB oder CD als Datenträger
> Datenträger für Arbeit was wird bevorzugt CD oder USB stick?

> [!check] Antwort
> USB 

> [!question] F5 — Fairness der Basisregler beim Zero-Shot-Transfer
> Bleiben die Basisregler starr auf die Baseline-Geometrie kalibriert (zeigt, was passiert wenn sie blind auf neue Geometrien losgelassen werden) oder werden sie für jede Geometrie fair nachkalibriert? Bei der bedarfsgesteuerten Abtauung passiert das intrinsisch, bei der Zeitabtauung müsste der Auslösezeitpunkt je Variante neu bestimmt werden. Die Entscheidung bestimmt, wie belastbar der Vergleich am Ende ist — siehe [[Arbeitsstand]].

> [!check] Antwort
> 

> [!question] F6 — Trainings- und Evaluationsmodell trennen?
> Wäre es methodisch akzeptabel, auf dem schnellen, stabilen Modell zu trainieren und nur auf dem detaillierten Modell (Haaf und Steiner + thermal resistance Frostmodell) zu evaluieren? Das würde die Instabilität und die Rechenzeit im Training umgehen und wäre ein sauberer Sim-to-Sim-Transfer. Ergänzt F3.

> [!check] Antwort
> 

> [!question] F7 — Validierung des Frost- und Abtaumodells
> Gibt es Prüfstands- oder Messdaten, gegen die sich Reifbildung und Abtauverhalten abgleichen lassen? Falls nicht: reicht eine Plausibilisierung gegen Literatur, und wie wird das in der Arbeit argumentiert?

> [!check] Antwort
> 

> [!question] F8 — Numerische Instabilität bei der Kreislaufumkehr
> Kondensatorzellen gehen während der Abtauung unter 250 K, der ExpansionTank läuft nicht weiter. Gibt es dazu Erfahrung im Institut — Rampe statt Sprung beim Vier-Wege-Ventil, andere Initialisierung, TIL-Support?

> [!check] Antwort
> 

### Falls Zeit bleibt

> [!question] F9 — DoE: Parametergrenzen und finThickness
> Min-/Max-Grenzen der vier Mikroparameter stehen noch aus ([[Design of Experiment]]). Gibt es fertigungstechnisch sinnvolle Grenzen oder Herstellerdaten? Und: finThickness war in Run 1 einflusslos — aus dem Plan nehmen (dann 3 Parameter, dichtere Abdeckung) oder Intervall weiten?

> [!check] Antwort
> 

> [!question] F10 — Gebäudemodell
> Ist das konzentrierte RC-Modell in der gewählten Tiefe akzeptiert, inklusive Vernachlässigung interner und solarer Gewinne? Und wird die flächenbezogene Wärmekapazität als zusätzliche DoE-Dimension aufgenommen oder nur über zwei Extremfälle (leicht/schwer) abgedeckt? Siehe [[Gebäude]].

> [!check] Antwort
> 


> [!question] F12 — Reward und Komfort
> Der Reward ist rein COP/COP_Carnot, Komfort wird nirgends bestraft. Soll ein Raumtemperatur-Constraint aufgenommen werden oder bleibt Komfort bewusst außerhalb der Zielfunktion?

> [!check] Antwort
> 

> [!question] F13 — Statistische Absicherung und Scope
> Wie viele Trainingsläufe (Seeds) pro Konfiguration werden erwartet, damit die Ergebnisse nicht als Einzellauf angreifbar sind? Und gehört der Algorithmenvergleich (QR-DQN vs. DQN vs. PPO) in den Umfang oder fällt er raus? Ebenso: TRY/TMY-Daten statt historischer Jahre?

> [!check] Antwort
> 

---

## 3. Sonstige Notizen

- Wie kann man Hydaraulikkreis realistisch darstellen geht das 

---

## 4. Ergebnis & nächste Schritte

- 
- 


