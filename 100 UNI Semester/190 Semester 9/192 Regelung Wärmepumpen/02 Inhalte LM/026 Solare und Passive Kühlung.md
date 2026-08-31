---
title: "05 Solare und Passive Kühlung"
type: vorlesung
tags:
  - Solar
  - Adsorption
  - Absorption
  - DEC
  - LNG
  - SkyCooling
erstellt: 2026-05-05
---

**Vorlesung**: [[192 Regelung von Wärmepumpen]]
**Thema**: Solare und passive Kühlung

---

# 05 Solare und Passive Kühlung
![[LM_VO_06_Solare-passive-Kühlung.pdf]]

> [!INFO] Motivation
> Der Kältebedarf korreliert zeitlich fast perfekt mit der Sonneneinstrahlung ("Kühlen, wenn die Sonne scheint"). Dies reduziert den Bedarf an teuren Speichern im Vergleich zum solaren Heizen.

## 1. Thermisch angetriebene Verfahren (Solar Cooling)

Man unterscheidet zwischen der Bereitstellung von Kaltwasser (geschlossene Verfahren) und der direkten Konditionierung von Luft (offene Verfahren).

### A. Geschlossene Verfahren (Kaltwassersätze)
Nutzung thermischer Energie (Solarthermie, Abwärme) statt mechanischer Arbeit (Strom).

#### 1. Absorptionskältemaschinen (AKM)
* **Prinzip**: Ein flüssiges Sorptionsmittel absorbiert das Kältemittel. Ein thermischer Verdichter ersetzt den mechanischen Verdichter.
* **Stoffpaare**:
    * **$H_2O / LiBr$ (Wasser/Lithiumbromid)**: Wasser ist Kältemittel. Einsatz $> 0^\circ C$ (Klimatisierung). Hoher COP ($0,7 \dots 0,8$), benötigt aber Antriebstemperaturen $> 75^\circ C$.
    * **$NH_3 / H_2O$ (Ammoniak/Wasser)**: Ammoniak ist Kältemittel. Einsatz $< 0^\circ C$ (Tiefkühlung) möglich. Benötigt Rektifikation (Trennung Wasser/Ammoniak).
* **Prozess**: Kontinuierlich.

#### 2. Adsorptionskältemaschinen (AdKM)
* **Prinzip**: Ein **festes** Sorptionsmittel (hochporös) lagert Kältemittel an der Oberfläche an.
* **Materialien**: Silikagel (Zeolithe) / Wasser.
* **Prozess**: Diskontinuierlich (Zyklischer Wechsel zwischen Adsorption und Desorption). Um Kälte kontinuierlich zu liefern, sind mindestens zwei Adsorberbetten nötig (eines kühlt/adsorbiert, eines regeneriert/desorbiert).
* **Vorteil**: Arbeitet bereits ab niedrigen Antriebstemperaturen (**$55 \dots 60^\circ C$**). Ideal für normale Flachkollektoren.
* **Nachteil**: Geringerer COP ($0,5 \dots 0,6$), großes Bauvolumen, hoher Preis.

> [!WARNING] Rückkühlung
> Thermische Kältemaschinen müssen ca. **2,5-mal mehr Wärme** an die Umgebung abgeben als Kompressionskältemaschinen (Abwärme aus Antrieb + Prozesswärme). Ein hocheffizienter Rückkühler ist essentiell!

---

### B. Offene Verfahren (Luftgestützt)

#### DEC (Desiccant and Evaporative Cooling)
Verfahren zur direkten Klimatisierung durch Trocknung und Verdunstungskühlung (ohne Kältemittelkreislauf).

**Die 4 Prozessschritte:**
1.  **Sorptive Entfeuchtung**: Außenluft strömt durch ein rotierendes Sorptionsrad (z.B. mit Silikagel beschichtet). Wasser wird gebunden, Adsorptionswärme wird frei $\to$ Luft wird **trocken und warm**.
2.  **Wärmerückgewinnung**: Die warme Zuluft wird im Rotationswärmeübertrager gegen die kühle Abluft vorgekühlt.
3.  **Verdunstungskühlung (Adiabat)**: Die trockene, vorgekühlte Luft wird befeuchtet. Die Verdunstungswärme entzieht der Luft Energie $\to$ Luft wird **feucht und kalt** (gewünschter Zustand).
4.  **Regeneration**: Die Abluft wird (solar) erwärmt (auf ca. $60 \dots 80^\circ C$), um das Sorptionsrad im Gegenstrom zu trocknen ("regenerieren").

* **Vorteil**: Kältemittelfrei, nutzt Niedertemperaturwärme.
* **Einsatz**: Besonders in feucht-heißen Klimazonen sinnvoll, wo reine Verdunstungskühlung nicht funktioniert.

---

## 2. Passive Kühlung (Vermeidung & Senken)

Ziel: Minimierung aktiver Kältearbeit durch Nutzung natürlicher Senken.

### Lastreduktion (Prävention)
* **Verschattung**: Außenliegend (Jalousien) ist ca. 3x effektiver als innenliegend, da die Wärme gar nicht erst durchs Glas tritt.
* **Thermische Masse**: Schwere Bauweise dämpft Temperaturspitzen (Phasenverschiebung in die Nachtstunden).

### Natürliche Wärmesenken
1.  **Erdreich**: Erdreichwärmeübertrager (Luft oder Sole) nutzen die konstante Temperatur des Bodens ($10 \dots 12^\circ C$) zur Vorkühlung der Zuluft.
2.  **Nachtlüftung**: Freie Kühlung durch kühle Nachtluft (automatisierte Fensteröffnung). Effektiv bei hoher thermischer Gebäudemasse.
3.  **Verdunstung**: Adiabate Kühlung (z.B. Besprühung von Dächern oder Kühltürmen).
	1. Aufpassen Legionellen, Hygiene, Luftfeuchtigkeit

### Sky Cooling (Strahlungskühlung)
Nutzung des Weltalls als ultimative Kältesenke ($3~K$).
* **Prinzip**: Die Atmosphäre ist für Wärmestrahlung im Bereich **$8 \dots 13~\mu m$** ("Atmosphärisches Fenster") weitgehend transparent.
* **Nacht**: Abstrahlung funktioniert natürlich (Tau/Reif-Bildung auch bei Lufttemperatur $> 0^\circ C$).
* **Tag**: Problem ist die Sonneneinstrahlung.
    * **Lösung**: Spezielle photonische Folien/Materialien, die Sonnenlicht ($0,3 \dots 2,5~\mu m$) zu $>95\%$ reflektieren, aber im Infraroten ($8 \dots 13~\mu m$) stark emittieren.
    * Erlaubt passive Kühlung unter Umgebungstemperatur selbst bei praller Sonne.

---

## 3. Nutzung von Abkälte (Cold Recovery)

Kälte, die als Nebenprodukt in Prozessen entsteht, statt sie zu vernichten.

### Beispiel: LNG-Regasifizierung
Erdgas (LNG) wird bei $-162^\circ C$ flüssig transportiert und muss am Terminal wieder verdampft (erwärmt) werden.
* **Konventionell**: Erwärmung gegen Meerwasser ("Open Rack Vaporizer"). Die Exergie der Kälte wird vernichtet.
* **Cold Recovery**:
    1.  **Stromerzeugung (ORC)**: Nutzung des LNG als kalte Seite (Senke) eines Organic Rankine Cycles. Wärmequelle ist Meerwasser. Erhöht den Wirkungsgrad des Kraftwerks drastisch.
    2.  **Direkte Nutzung**: Fernkälte für Rechenzentren, Tiefkühllager oder Luftzerlegung in der Nähe des Terminals.

### Kennzahlen
* **Cold Recovery Ratio (CCR)**:
    Verhältnis von genutzter Energie zur theoretisch verfügbaren Enthalpiedifferenz.
    $$CCR = \frac{P_{net} + \dot{Q}_{cool}}{\dot{m}_{LNG} \cdot (h_{0} - h_{LNG})}$$
    * $P_{net}$: Netto-Stromerzeugung
    * $\dot{Q}_{cool}$: Genutzte Kälteleistung
* **Exergetischer Wirkungsgrad**: Berücksichtigt die Qualität der Energie (Kälte bei -160°C ist wertvoller als bei -10°C).