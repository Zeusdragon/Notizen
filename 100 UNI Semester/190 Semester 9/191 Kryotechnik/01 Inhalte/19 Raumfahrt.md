---
title: "19 Raumfahrt"
type: vorlesung
erstellt: 2026-05-05
---

**Vorlesung**: [[191 Kryotechnik]]
**Datum**: 2026-01-20
**Topics**: [[Raumfahrt]], [[Kryokühler]], [[Infrarot-Astronomie]], [[Treibstoffe]]

---
![[19 Weltraumkryotechnik.pdf]]

# 19 Weltraumkryotechnik

> [!INFO] Übersicht
> Kryotechnik ist im Weltraum unverzichtbar für zwei Hauptbereiche:
> 1.  **Antriebstechnik**: Hochenergetische Treibstoffe ($LH_2 + LOX$).
> 2.  **Instrumentierung**: Kühlung von Infrarot-Sensoren (IR) zur Reduktion von thermischem Rauschen ("Dunkelstrom").

## 1. Kryogene Antriebe
Wasserstoff bietet in Kombination mit Sauerstoff die höchste energiedichte Verbrennung für chemische Raketen.
* **Treibstoff**: Flüssigwasserstoff ($LH_2$) und Flüssigsauerstoff ($LOX$).
* **Vorteil**: Höchster spezifischer Impuls ($I_{sp}$).
* **Herausforderung**: Speicherung großer Mengen bei extrem tiefen Temperaturen in leichten Tanks (Ariane 5/6 Hauptstufe).
* **Zukunft**: Nuklear-thermische Antriebe (NERVA) nutzen $H_2$ als Stützmasse, die durch einen Reaktor erhitzt wird (keine Verbrennung $\to$ doppelter $I_{sp}$).

## 2. Infrarot-Astronomie
Das Universum "leuchtet" im Infraroten (Wärmestrahlung kalter Objekte, rotverschobene Galaxien). Um diese schwache Strahlung zu messen, müssen die Detektoren kälter sein als das Signal.

### Kühlmethoden im Satelliten
Da im Weltraum Konvektion fehlt und Strom (Solarzellen) begrenzt ist, sind spezielle Kühlsysteme nötig.

#### A. Speicher-Kühlung (Open Loop)
Ein Tank mit **superfluidem Helium** (He-II, ca. 1,6 - 1,8 K) wird mitgeführt.
* **Prinzip**: Helium verdampft durch einen "porösen Stopfen" (Porous Plug), der als Phasenseparator wirkt (nur Gas entweicht, Flüssigkeit bleibt durch thermomechanischen Effekt im Tank).
* **Vorteil**: Keine Vibrationen, extrem stabile Temperatur.
* **Nachteil**: Lebensdauer ist durch den Vorrat begrenzt (z.B. Herschel-Teleskop: ca. 3,5 Jahre). Wenn He leer $\to$ Mission vorbei.

#### B. Mechanische Kryokühler (Closed Loop)
Für Langzeitmissionen (> 5-10 Jahre) oder wenn Vibrationen tolerierbar sind.
* **Stirling / Pulse Tube**: Hohe Effizienz, aber bewegte Teile (Vibration).
* **Joule-Thomson / Sorption**: Vibrationsfrei, aber geringere Effizienz.
* **Aktuelle Technik (z.B. JWST - James Webb)**:
    * **Passive Kühlung**: Riesiges Sonnenschild kühlt das Teleskop auf ca. 40 K.
    * **Aktive Kühlung (MIRI-Instrument)**: Ein **Pulse-Tube-Kühler** (Vorkühlung) kombiniert mit einem **Joule-Thomson-Kreislauf** kühlt den Detektor auf **< 7 K**.

### Spezielle Effekte im Weltraum (µg)
* **Phasentrennung**: In der Schwerelosigkeit trennen sich Gas und Flüssigkeit nicht von selbst (kein "Oben" und "Unten").
    * $\to$ Nutzung von Kapillarkräften (Vanes/Bleche im Tank) oder Rotation (künstliche Schwerkraft), um Flüssigkeit zum Auslass zu führen.
    * $\to$ **Porous Plug** (Sintermetall) nutzt den thermomechanischen Effekt von He-II, um Gas und Flüssigkeit zu trennen.