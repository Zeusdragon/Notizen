---
title: "Gebäudemodell & Fußbodenheizung"
type: diplom
status: in-bearbeitung
tags:
  - gebaeude
  - fussbodenheizung
  - randbedingungen
  - rc-modell
erstellt: 2026-09-09
---

# Gebäudemodell & Fußbodenheizung

**Bezug:** [[Dymola-Modell]] · [[Arbeitsstand]] · [[Design of Experiment]] · [[Wetterdaten]] · [[Geometrische Parameter]]

---

## 1. Ziel und Modellierungstiefe

Das Gebäude ist in dieser Arbeit **nicht Untersuchungsgegenstand**, sondern **Randbedingung**. Es liefert der Wärmepumpe die Senke, gibt über die Vorlauftemperatur den Betriebspunkt vor und bestimmt — das ist der eigentlich interessante Teil für den RL-Agenten — **wie lange ein Abtauvorgang thermisch überbrückt werden kann**, ohne dass die Raumtemperatur wegläuft.

Daraus folgt die Modellierungstiefe: kein Mehrzonenmodell, keine bauteilaufgelöste Simulation, sondern ein **konzentriertes RC-Modell** (lumped capacitance). Begründung:

* Der Agent beobachtet laut [[Dymola-Modell]] ohnehin nur $T_{Luft}$, $T_{Verdampfer}$, $\varphi_{Luft}$ — er *sieht* die Gebäudedetails gar nicht.
* Die Zeitkonstanten des Gebäudes (Stunden bis Tage) liegen um Größenordnungen über denen der Reifbildung und der Abtauung (Minuten). Für das Regelungsproblem zählt nur die **integrale Speicherwirkung**, nicht deren räumliche Verteilung.
* Ein einfaches RC-Modell hat wenige, physikalisch interpretierbare Parameter — genau das, was man für eine saubere Parametervariation im [[Design of Experiment]] braucht.

> [!info] Kernaussage
> Das Gebäude wird auf **drei Zahlen** reduziert: einen Wärmeverlustleitwert $UA$, eine Wärmekapazität $C$ und einen Übergangswiderstand $R$ zwischen Fußbodenheizung und Gebäude.

---

## 2. Randbedingungen (Festlegungen)

| Größe | Wert | Herkunft / Begründung |
| --- | --- | --- |
| Beheizte Wohnfläche $A$ | 130 m² | Typisches freistehendes EFH, Zielmarkt für 10-kW-Luft/Wasser-WP |
| Norm-Heizlast $\dot{Q}_{N}$ | 10 kW | Leistungsklasse der Anlage, fixiert über die Makro-Parameter in [[Geometrische Parameter]] |
| Spez. Heizlast | 77 W/m² | Ergibt sich aus 10 kW / 130 m² → Bestandsgebäude, teilsaniert |
| Raumsolltemperatur $T_i$ | 20 °C | Konsistent zum unteren Punkt der Heizkurve im [[Dymola-Modell]] |
| Norm-Außentemperatur $T_{a,N}$ | −10 °C | Norm-Außentemperatur Frankfurt a. M., Auslegungspunkt der Anlage |
| Heizgrenze | 15 °C | Oberer Punkt der Heizkurve |
| Wärmeübergabe | Fußbodenheizung 35/28 | Niedertemperatur, Voraussetzung für brauchbaren COP |
| Standort / Wetter | Frankfurt a. M. | siehe [[Wetterdaten]] |

**Bewusste Vereinfachungen:** Keine internen Gewinne (Personen, Geräte), keine solaren Gewinne, keine Lüftungsdynamik, keine Zonierung. Alle drei würden die Heizlast reduzieren und damit die Abtauhäufigkeit senken — sie wirken also *nicht* konservativ und müssen unter [[#9. Offene Punkte]] bewertet werden.

---

## 3. Gedankengang Schritt 1: Wärmeverlustleitwert des Gebäudes

Die Heizlast ist die einzige belastbare Größe, die man von außen vorgibt. Alles Weitere wird daraus rückwärts erschlossen — **nicht** aus U-Werten einzelner Bauteile, denn die kennt man bei einem generischen Gebäude nicht besser als die Heizlast selbst.

$$UA_{Geb} = \frac{\dot{Q}_N}{T_i - T_{a,N}} = \frac{10\,000\ \text{W}}{20 - (-10)\ \text{K}} = 333{,}3\ \frac{\text{W}}{\text{K}}$$

$$R_{Geb} = \frac{1}{UA_{Geb}} = 3{,}0 \cdot 10^{-3}\ \frac{\text{K}}{\text{W}}$$

Damit ist die stationäre Kennlinie des Gebäudes vollständig festgelegt:

| $T_a$ | −10 °C | −5 °C | 0 °C | 5 °C | 10 °C | 15 °C |
| --- | --- | --- | --- | --- | --- | --- |
| Heizlast | 10,0 kW | 8,3 kW | 6,7 kW | 5,0 kW | 3,3 kW | 0 kW * |
| Vorlauf (Heizkurve) | 35,0 °C | 32,0 °C | 29,0 °C | 26,0 °C | 23,0 °C | 20,0 °C |

\* Rechnerisch verbleiben an der Heizgrenze noch 1,7 kW Transmissionsverlust, die real durch interne und solare Gewinne gedeckt werden. Da beide hier vernachlässigt sind (Abschn. 2), ist die Heizgrenze von 15 °C eine **gesetzte**, keine hergeleitete Größe.

Die Heizkurve ist linear zwischen $(T_a = -10\,^\circ\text{C},\ T_{VL} = 35\,^\circ\text{C})$ und $(T_a = 15\,^\circ\text{C},\ T_{VL} = 20\,^\circ\text{C})$:

$$T_{VL} = 20\,^\circ\text{C} + 15\ \text{K}\cdot\frac{15\,^\circ\text{C} - T_a}{25\ \text{K}}$$

Die Steigung beträgt damit glatte **0,6 K Vorlauf je K Außentemperatur**.

Das ist genau der Sollwert, auf den der PI-Regler des Verdichters im [[Dymola-Modell]] fährt.

> [!note] Warum der Bereich um 0…5 °C der kritische ist
> Bei $T_a \approx 0\ldots5\,^\circ\text{C}$ und hoher Luftfeuchte ist die Reifbildung am stärksten — gleichzeitig liegt die Heizlast nur bei 5,0…6,7 kW. Der Verdichter läuft also in Teillast, während er am häufigsten abtauen muss. Genau in diesem Fenster entscheidet sich der SCOP.

---

## 4. Gedankengang Schritt 2: Wärmekapazität des Gebäudes

### 4.1 Warum flächenbezogen

Die wirksame Wärmekapazität eines Gebäudes hängt von Bauweise, Bauteilaufbau und wirksamer Eindringtiefe ab. Sie exakt zu berechnen (DIN EN ISO 13786, Bauteil für Bauteil) wäre hier Scheingenauigkeit. Vereinfachte Verfahren (DIN EN ISO 13790, VDI 6007) arbeiten deshalb mit einer **flächenbezogenen wirksamen Wärmekapazität** $c''$ in J/(m²K), bezogen auf die Nettogrundfläche. Das ist ein Parameter, kein Messwert — und damit ideal als DoE-Dimension.

$$C_{Geb} = c'' \cdot A$$

### 4.2 Kapazitätsklassen

Als **Baseline** wird $c'' = 110\,000\ \text{J/(m}^2\text{K)}$ gesetzt (mittelschwere Bauweise: Massivwände, Betondecke, leichte Innenwände, üblicher Möblierungsgrad). Die übrigen Klassen spannen den Variationsbereich für die Robustheitsanalyse auf:

| Bauweise | $c''$ [J/(m²K)] | $C_{Geb}$ [MJ/K] | $C_{Geb}$ [kWh/K] | $\tau = R_{Geb}C_{Geb}$ |
| --- | --- | --- | --- | --- |
| Sehr leicht (Holzständer, Trockenbau) | 50 000 | 6,50 | 1,81 | 5,4 h |
| Leicht (Holzbau mit Estrich) | 75 000 | 9,75 | 2,71 | 8,1 h |
| **Mittel — Baseline** | **110 000** | **14,30** | **3,97** | **11,9 h** |
| Schwer (Massivbau, Betondecken) | 175 000 | 22,75 | 6,32 | 19,0 h |
| Sehr schwer (Massivbau, hohe Speichermasse) | 250 000 | 32,50 | 9,03 | 27,1 h |

Der Bereich 50 000 … 250 000 J/(m²K) deckt damit Zeitkonstanten von **5,4 h bis 27,1 h** ab — also praktisch das gesamte Spektrum realer Wohngebäude.

### 4.3 Wozu das gut ist: die Abtau-Überbrückung

Das ist der Grund, warum diese Tabelle in der Arbeit überhaupt steht. Während einer Abtauung (Kreislaufumkehr) liefert die Anlage nicht nur keine Wärme, sie entzieht dem Heizkreis sogar welche. Für eine **10-minütige Abtauung bei $T_a = 2\,^\circ\text{C}$** (Heizlast 6,0 kW → 1,00 kWh entfallene Wärme) ergibt sich als Abschätzung des Raumtemperatur-Einbruchs:

| $c''$ [J/(m²K)] | 50 000 | 75 000 | **110 000** | 175 000 | 250 000 |
| --- | --- | --- | --- | --- | --- |
| $\Delta T_{Raum}$ pro Abtauung | 0,55 K | 0,37 K | **0,25 K** | 0,16 K | 0,11 K |

> [!important] Konsequenz für den RL-Agenten
> Im leichten Gebäude kostet jede Abtauung spürbaren Komfort — der Agent muss Abtauungen **seltener und gezielter** setzen. Im schweren Gebäude ist die einzelne Abtauung thermisch nahezu unsichtbar; hier zählt fast nur der COP-Verlust. **Dieselbe Politik ist also nicht automatisch in beiden Gebäuden optimal.** Das macht $c''$ zu einem sinnvollen Kandidaten für das Zero-Shot-Transfer-Testing neben den Verdampfergeometrien aus [[Design of Experiment]].

---

## 5. Gedankengang Schritt 3: Der Widerstand $R = 0{,}001$ K/W

### 5.1 Was dieser Widerstand physikalisch ist

$R$ ist **nicht** der Wärmedurchgang des Gebäudes nach außen (das ist $R_{Geb}$ aus Abschnitt 3). $R$ sitzt **zwischen dem Estrich-/Heizwasser-Knoten der Fußbodenheizung und dem Raum- bzw. Gebäudeknoten**. Er beschreibt also: *Wie gut kommt die Wärme, die im Rohr steckt, tatsächlich im Gebäude an?*

```
      Wärmepumpe
          │  Q_zu
          ▼
   ┌─────────────┐       R = 1e-3 K/W      ┌──────────────┐   R_Geb = 3,0e-3 K/W
   │   C_FBH     │───────/\/\/\/\──────────│    C_Geb     │──────/\/\/\/\──────►  T_a
   │  Estrich +  │                         │ Wände, Decken│                     (Wetter)
   │   Wasser    │                         │ Möbel, Luft  │
   └─────────────┘                         └──────────────┘
    T_FBH ≈ 31 °C                            T_i = 20 °C
```

Ohne diesen Widerstand wäre die Fußbodenheizung ein idealer Wärmeübertrager — die Vorlauftemperatur hätte keinen Einfluss mehr auf die übertragene Leistung, und die charakteristische **Trägheit** der Flächenheizung ginge verloren. Genau diese Trägheit ist aber der Puffer, der eine Abtauung überbrückt.

### 5.2 Herleitung über die Schichtwiderstände

$R$ wird als Reihenschaltung der flächenbezogenen Widerstände von der Rohrebene bis zur Raumluft aufgebaut:

| Schicht | Annahme | $R''$ [m²K/W] |
| --- | --- | --- |
| Wärmeübergang Rohr innen + Rohrwand | $\alpha_i > 1000$ W/(m²K), PE-Xa 17×2 | ≈ 0 (vernachlässigt) |
| Wärmeleitung Rohr → Estrich (Verlegeabstand 150 mm) | Spreizwiderstand, pauschal | 0,010 |
| Estrich über Rohrachse | 45 mm, $\lambda = 1{,}4$ W/(mK) | 0,032 |
| Bodenbelag | Fliesen 10 mm, $\lambda = 1{,}0$ W/(mK) | 0,010 |
| Wärmeübergang Boden → Raum | $\alpha = 10{,}8$ W/(m²K) (Strahlung + Konvektion nach oben, EN 1264) | 0,093 |
| **Summe** | | **0,145** |

$$U_{FBH} = \frac{1}{0{,}145\ \text{m}^2\text{K/W}} = 6{,}9\ \frac{\text{W}}{\text{m}^2\text{K}}$$

$$UA_{FBH} = 6{,}9 \cdot 130\ \text{m}^2 = 898\ \frac{\text{W}}{\text{K}} \qquad\Rightarrow\qquad R = \frac{1}{898} = 1{,}11 \cdot 10^{-3}\ \frac{\text{K}}{\text{W}}$$

**Im Modell wird auf $R = 1{,}0 \cdot 10^{-3}$ K/W gerundet.** Die Abweichung von 10 % liegt deutlich innerhalb der Unsicherheit des Bodenbelags allein (Fliesen 0,01 vs. Teppich 0,10 m²K/W ändern $R''$ um ±60 %).

> [!tip] Merkregel
> Der dominierende Anteil ist mit 64 % der **Wärmeübergang Boden → Raum**, nicht der Schichtaufbau. Eine Fußbodenheizung ist im Wesentlichen durch die Physik der Oberfläche begrenzt — deshalb landet man bei praktisch jedem Aufbau in der Größenordnung $R'' \approx 0{,}12\ldots0{,}18$ m²K/W, also $R \approx 10^{-3}$ K/W bei ~130 m².

### 5.3 Gegenprobe: Reproduziert $R$ die Auslegung?

Der Test, der den Wert belastbar macht — mit der Auslegung 35/28 und $T_i = 20\,^\circ\text{C}$:

$$\Delta T_{log} = \frac{T_{VL} - T_{RL}}{\ln\frac{T_{VL}-T_i}{T_{RL}-T_i}} = \frac{35-28}{\ln\frac{15}{8}} = 11{,}1\ \text{K}$$

$$\dot{q} = U_{FBH}\cdot\Delta T_{log} = 6{,}9 \cdot 11{,}1 = 77\ \frac{\text{W}}{\text{m}^2} \qquad\Rightarrow\qquad \dot{Q} = 77 \cdot 130 = \mathbf{10{,}0\ \text{kW}}$$

Das trifft die Norm-Heizlast aus Abschnitt 2 **exakt**. Der Widerstand ist damit nicht frei gewählt, sondern durch Heizlast, Fläche und Auslegungstemperaturen überbestimmt und konsistent.

Zweite Gegenprobe — Komfortgrenze nach EN 1264 (max. 29 °C Bodenoberfläche im Aufenthaltsbereich):

$$T_{Boden} = T_i + \frac{\dot{q}}{\alpha} = 20 + \frac{77}{10{,}8} = 27{,}1\ ^\circ\text{C} \quad < 29\ ^\circ\text{C} \quad\checkmark$$

Die maximal zulässige Flächenleistung wäre 97 W/m². Die Auslegung nutzt davon 79 % — die Anlage ist also auslegungskonform und hat noch Reserve für das Wiederaufheizen nach einer Abtauung.

---

## 6. Gedankengang Schritt 4: Auslegung des Rohrnetzes

### 6.1 Rohranzahl und Rohrlänge

Ausgangspunkt ist der Verlegeabstand VA = 150 mm (Standard für Wohnräume bei dieser Flächenleistung). Pro Meter Rohr wird damit eine Fläche von 0,15 m² beheizt:

$$L_{erf} = \frac{A}{VA} = \frac{130\ \text{m}^2}{0{,}15\ \text{m}} = 867\ \text{m}$$

Diese Länge muss auf mehrere Kreise aufgeteilt werden, weil die Kreislänge durch den Druckverlust begrenzt ist (Faustwert für 17×2: **max. 100…120 m**, damit der Kreis unter ~250 mbar bleibt). Mit **16 Kreisen à 60 m** ergibt sich:

| Größe | Wert |
| --- | --- |
| Anzahl Heizkreise $n$ | 16 |
| Länge je Kreis | 60 m |
| Gesamtrohrlänge | 960 m |
| davon Anbindeleitung zum Verteiler (Annahme) | ≈ 5 m je Kreis → 80 m |
| tatsächlich in der Fläche verlegt | 880 m |
| **beheizte Fläche** $880 \cdot 0{,}15$ | **132 m² ≈ 130 m²** ✔ |
| Fläche je Kreis | 8,25 m² |

Die Konfiguration **16 × 60 m schließt sich also mit den 130 m² Wohnfläche bei VA 150 mm**. 8,25 m² je Kreis entspricht einem kleinteiligen Raumzuschnitt (ein Kreis je Raum, größere Räume mit zwei Kreisen) — plausibel für ein EFH mit rund 8 Räumen auf zwei Geschossen.

> [!check] Warum nicht weniger, dafür längere Kreise?
> 8 Kreise à 110 m wären hydraulisch noch zulässig, aber der Druckverlust steigt linear mit der Länge und quadratisch mit dem Volumenstrom je Kreis → grob 8-facher Kreisdruckverlust. Bei 16 kurzen Kreisen bleibt die Pumpe klein, und das geht direkt in den COP ein (Hilfsenergie).

### 6.2 Hydraulik

Mit der Auslegungsspreizung $\Delta T = 7$ K (35/28):

$$\dot{m}_{ges} = \frac{\dot{Q}_N}{c_p \cdot \Delta T} = \frac{10\,000}{4180 \cdot 7} = 0{,}342\ \frac{\text{kg}}{\text{s}} = 1{,}23\ \frac{\text{m}^3}{\text{h}}$$

| Größe | Wert | Bewertung |
| --- | --- | --- |
| Massenstrom gesamt | 0,342 kg/s = 1,23 m³/h | |
| Massenstrom je Kreis | 21,4 g/s = 77 l/h | |
| Innendurchmesser 17×2 | 13 mm ($A_i = 1{,}33\cdot10^{-4}$ m²) | |
| **Strömungsgeschwindigkeit** | **0,16 m/s** | im Zielband 0,15…0,5 m/s ✔ |
| Reynoldszahl | ≈ 2 600 | Übergangsbereich |
| Druckverlust je Kreis | ≈ 2 650 Pa = 26 mbar | unkritisch, ≪ 250 mbar ✔ |
| Wasserinhalt gesamt | 127 l | |

Die untere Grenze 0,15 m/s ist die **Entlüftungsgrenze** (darunter werden Luftblasen nicht mehr mitgerissen), die obere 0,5 m/s die **Geräuschgrenze**. Mit 0,16 m/s liegt die Auslegung knapp, aber zulässig darüber — typisch für Flächenheizungen.

> [!warning] Re ≈ 2600 — Übergangsbereich
> Die Strömung ist weder sicher laminar noch voll turbulent. Für den Druckverlust wurde Blasius angesetzt ($\lambda = 0{,}044$); rein laminar gerechnet wären es nur ~15 mbar. Für die **Wärmeübertragung** ist das relevanter als für den Druckverlust: im laminaren Fall sinkt $\alpha_i$ deutlich. Da $\alpha_i$ in Abschnitt 5.2 aber ohnehin vernachlässigt wird (Anteil < 2 % am Gesamtwiderstand), bleibt die Auswirkung auf $R$ vernachlässigbar.
> Der Verteilerdruckverlust (Ventile, Durchflussmesser, Anbindung) dominiert mit typisch 100…200 mbar ohnehin — das ist die Größe, die die **Pumpenauslegung** im [[Dymola-Modell]] bestimmt, nicht der Kreisdruckverlust.

---

## 7. Kapazität des Fußbodenheizungs-Knotens

Der Estrich ist selbst ein erheblicher Speicher und liegt *hinter* $R$ — er gehört daher an den FBH-Knoten, nicht ans Gebäude:

| Anteil | Rechnung | $C$ |
| --- | --- | --- |
| Estrich | 130 m² · 0,065 m · 2000 kg/m³ · 1000 J/(kgK) | 16,90 MJ/K |
| Heizwasser | 127 l · 4180 J/(kgK) | 0,53 MJ/K |
| **$C_{FBH}$** | | **17,4 MJ/K (4,8 kWh/K)** |

Die Eigenzeitkonstante des Estrichknotens:

$$\tau_{FBH} = R \cdot C_{FBH} = 1{,}11\cdot10^{-3} \cdot 17{,}4\cdot10^{6} = 19\,400\ \text{s} \approx \mathbf{5{,}4\ h}$$

Das ist die berüchtigte Trägheit der Fußbodenheizung — und zugleich ein **zweiter, unabhängiger Puffer** neben der Gebäudemasse. Während einer 10-minütigen Abtauung kühlt primär der Estrich aus, nicht der Raum. Erst wenn Abtauungen zu dicht aufeinander folgen, greift der Einbruch auf $C_{Geb}$ durch.

> [!danger] Wichtige Modellierungs-Entscheidung — Doppelzählung vermeiden
> $C_{FBH} = 17{,}4$ MJ/K ist **größer** als die Baseline-Gebäudekapazität $C_{Geb} = 14{,}3$ MJ/K. In den vereinfachten Normverfahren ist der Estrich in $c''$ bereits *enthalten*. Wird er hier zusätzlich als eigener Knoten geführt, ist die Speichermasse doppelt angesetzt.
>
> **Festlegung für dieses Modell:** Estrich + Wasser bilden den separaten Knoten $C_{FBH}$ (weil sie eine eigene, deutlich schnellere Dynamik hinter $R$ haben). Die Werte der Tabelle aus Abschnitt 4.2 sind dann als *wirksame Kapazität der übrigen Gebäudemasse* zu lesen — Wände, Decken, Innenbauteile, Möblierung, Luft, **ohne Estrich**. Das ist so zu dokumentieren, sonst ist die Tabelle missverständlich.

> [!note] Randnotiz zum RL-Setup
> $\tau_{FBH} \approx 19\,400$ s entspricht bei 100 s Zeitschritt rund **194 Schritten**. Der `VecFrameStack` mit $n = 36$ (= 1 h, siehe [[Arbeitsstand]]) deckt damit nur etwa **0,2 $\tau_{FBH}$** ab. Der Agent sieht den thermischen Zustand des Estrichs also nur als kurzen Ausschnitt. Ob das ausreicht, oder ob eine langsame Zustandsgröße (z. B. gleitender Mittelwert der Vorlauftemperatur) in die Observation gehört, ist zu prüfen.

---

## 8. Zusammenfassung: die Modellparameter

| Parameter | Symbol | Wert | Quelle |
| --- | --- | --- | --- |
| Beheizte Fläche | $A$ | 130 m² | Festlegung |
| Wärmeverlustleitwert | $UA_{Geb}$ | 333 W/K | aus 10 kW @ ΔT 30 K |
| Widerstand nach außen | $R_{Geb}$ | 3,0 · 10⁻³ K/W | $1/UA_{Geb}$ |
| Gebäudekapazität (Baseline) | $C_{Geb}$ | 14,3 MJ/K | 110 000 J/(m²K) · 130 m² |
| **Widerstand FBH → Gebäude** | $R$ | **1,0 · 10⁻³ K/W** | Schichtaufbau, Abschn. 5.2 |
| Kapazität FBH-Knoten | $C_{FBH}$ | 17,4 MJ/K | Estrich + Wasser |
| Heizkreise | $n$ | 16 | Abschn. 6.1 |
| Länge je Kreis | $L$ | 60 m | Abschn. 6.1 |
| Verlegeabstand | $VA$ | 150 mm | Standard Wohnraum |
| Rohr | | PE-Xa 17 × 2 mm | Standard |
| Auslegung | | 35/28 °C bei −10 °C | Heizkurve [[Dymola-Modell]] |
| Nennvolumenstrom | $\dot{V}$ | 1,23 m³/h | aus 10 kW @ ΔT 7 K |

### Konsistenzprüfungen (alle bestanden)

| Prüfung | Soll | Ist | |
| --- | --- | --- | --- |
| Flächenleistung reproduziert Heizlast | 10,0 kW | 10,0 kW | ✔ |
| Bodenoberflächentemperatur | ≤ 29 °C | 27,1 °C | ✔ |
| Flächenleistung vs. Maximum | ≤ 97 W/m² | 77 W/m² | ✔ |
| Rohrlänge deckt Wohnfläche | 130 m² | 132 m² | ✔ |
| Kreislänge | ≤ 100…120 m | 60 m | ✔ |
| Strömungsgeschwindigkeit | 0,15…0,5 m/s | 0,16 m/s | ✔ |
| Kreisdruckverlust | ≤ 250 mbar | 26 mbar | ✔ |
| Gebäude-Zeitkonstante | 5…50 h | 11,9 h | ✔ |

---

## 9. Offene Punkte

- [ ] **Doppelzählung Estrich klären** (Abschn. 7): Entweder die $c''$-Werte explizit als „ohne Estrich" definieren oder den Estrich in $C_{Geb}$ integrieren und den FBH-Knoten auf den Wasserinhalt reduzieren. Auswirkung auf $\tau$ prüfen.
- [ ] Interne und solare Gewinne bewerten — sie senken die Heizlast im kritischen Bereich 0…5 °C und damit die Verdichterlaufzeit; ggf. als konstanter Abzug (z. B. 3 W/m²) modellieren.
- [ ] Entscheiden, ob $c''$ als **zusätzliche DoE-Dimension** neben der Verdampfergeometrie aufgenommen wird ([[Design of Experiment]]) oder ob nur zwei Extremfälle (leicht / schwer) für das Zero-Shot-Testing exportiert werden.
- [ ] Sensitivität des Bodenbelags prüfen: Teppich ($R'' = 0{,}10$ m²K/W) senkt $U_{FBH}$ auf 4,2 W/(m²K) → die Auslegung 35/28 trägt dann nur noch 47 W/m², die Heizkurve müsste angehoben werden. Relevant für die Frage, wie robust der Agent gegenüber der Vorlauftemperatur ist.
- [ ] Verteilerdruckverlust und Pumpenkennlinie für den Punkt „Pumpe" im [[Dymola-Modell]] konkretisieren.
- [ ] Prüfen, ob eine langsame Zustandsgröße in die Observation gehört (Abschn. 7, Randnotiz).
