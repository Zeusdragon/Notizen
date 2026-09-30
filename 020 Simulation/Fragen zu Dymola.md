---
title: "Fragen zu Dymola"
type: konzept
erstellt: 2026-08-18
---

# Fragen zu Dymola

## Performance

> [!question] F1 — Multiprocessing
> Wie lassen sich in Dymola mehrere Simulationen parallel ausführen (z. B. Parameterstudien oder Sweeps)? Gibt es dafür eine eingebaute Funktion oder muss das über Python bzw. mehrere Dymola-Instanzen gelöst werden – und wie verhält es sich mit den Lizenzen pro Prozess?

> [!check] Antwort
> Multiprocessing nur mit FMU

> [!question] F2 — GPU-Beschleunigung
> Kann Dymola selbst oder eine daraus exportierte FMU die Berechnung auf die GPU auslagern? Falls nicht: Welche anderen Wege gibt es, die Simulationszeit zu verkürzen (Solver-Wahl, Toleranzen, Modellvereinfachung)?

> [!check] Antwort
> Unklar eher nein

## Modellierung

> [!question] F3 — Kopplung mit Gebäudemodellen
> Wie lässt sich das Wärmepumpenmodell an ein Gebäudemodell anbinden, das entweder in Python geschrieben oder in Dymola mit der [Aixlib](https://github.com/RWTH-EBC/AixLib) (Gebäudesimulationsbibliothek der RWTH Aachen) aufgebaut ist? Problem: Die Medienbibliotheken von Dymola/AixLib (Modelica.Media) und TIL sind nicht kompatibel. Gibt es dafür empfohlene Schnittstellen oder Adapter, oder ist eine Kopplung über FMU bzw. Co-Simulation der bessere Weg?

> [!check] Antwort
> 

> [!question] F4 — Eigene Frostmodelle / TIL-Klassen erweitern
> Wie kann man eigene Frostmodelle in TIL einbinden bzw. bestehende Klassen der TIL-Bibliothek erweitern (z. B. über `extends` oder `replaceable`), ohne die Bibliothek selbst zu verändern? Welche Klassen bzw. Schnittstellen sind dafür der richtige Einstiegspunkt?

> [!check] Antwort
> 

> [!question] F5 — Controller-Bausteine schreiben
> Wie erstellt man eigene Reglerbausteine in Dymola? Lieber grafisch aus der Modelica-Standardbibliothek (`Modelica.Blocks`) zusammensetzen oder direkt als Modelica-Code in einem eigenen Block? Und wie lassen sich diskrete Logiken (z. B. Abtau-Trigger, Zustandsautomaten, Sollwerte) sauber abbilden?

> [!check] Antwort
> Man würde ein neues Modell machen und in Modelica per Equations und when und if bedingungen Schreiben. Geht aber alles ist machbar

> [!question] Iteration Variables
> wie ist das gemint wenn als Warnung Iterations variablen sind nicht gesetzt aufkommt und wie löst man das reicht es einfach nur modifier anzugegeben um es zu hotfixen oder geht das eleganter

> [!check] Antwort
> Gibt eine weiter Fortbildung dazu aktueller Stand wird für mich wahrscheinlich sein per Modifier die Variablen für den Start zu setzen
## Automatisierung & Export

> [!question] F6 — FMU-Export automatisieren
> Lässt sich der FMU-Export automatisieren, sodass bei geänderten Parametern oder Konfigurationen (z. B. verschiedene Verdampfergeometrien) automatisch neue FMUs erzeugt werden?

> [!check] Antwort
> Ja – über das Python-Interface von Dymola lässt sich das skripten, der FMU-Export ist automatisierbar. In Dymola selber unklar ob automatisierung funktioniert

> [!question] F7 — Plots speichern
> Können in Dymola erstellte Plots direkt gespeichert bzw. exportiert werden (als Bild oder Daten, z. B. PNG/SVG/CSV), oder ist das nur über DaVE möglich bzw. innerhalb von Dymola nur durch Screenshots? Lassen sich Plot-Setups für wiederkehrende Auswertungen speichern?

> [!check] Antwort
> Plot speichern unter Tools möglich dort wird dann das aktive Plot window dargestellt.
> 

## System

> [!question] F8 — Linux-Support
> Wie gut läuft Dymola unter Linux (Installation, Lizenzierung, Compiler)? Laufen unter Windows exportierte FMUs auch unter Linux, oder müssen sie dort neu kompiliert bzw. mit Linux-Binaries exportiert werden (relevant für das Training auf einem Linux-Rechner/Cluster)?

> [!check] Antwort
> Linux sollte problemlos funktionieren mit TIL und Dymola das geht.
