**Vorlesung**: [[191 Kryotechnik]]
**Datum**: 2026-01-27
**Topics**: [[Cryocooler]], [[Gifford-McMahon]], [[Pulsrohrkühler]], [[Kryopumpe]], [[Vakuum]]

---
![[20 Cryocooler und Kryovakuumpumpen.pdf]]
# 20 Cryocooler und Kryovakuumpumpen

> [!INFO] Definition Cryocooler
> Kryogene Kleinkühler ("Refrigerator") stellen Kälteleistung im Bereich von **Milliwatt bis einigen Watt** bei Temperaturen zwischen **4 K und 100 K** bereit. Sie arbeiten im geschlossenen Kreislauf (Closed Loop) und benötigen meist Helium als Arbeitsgas.

## 1. Cryocooler Technologien

### A. Peltier-Kühler (Thermoelektrisch)
Nutzt den **Peltier-Effekt** an Halbleiterübergängen (n-Typ/p-Typ, z.B. $Bi_2Te_3$).
* **Vorteil**: Keine bewegten Teile, lautlos, sehr billig, extrem kompakt.
* **Nachteil**: Miserabler Wirkungsgrad ($\eta \approx 1\%$ von Carnot). Geringer Temperaturhub pro Stufe.
* **Limit**: Mit mehrstufigen Kaskaden sind ca. **130 ... 170 K** erreichbar (nicht tiefkalt genug für Supraleitung).

### B. Regenerative Gaskältemaschinen
Nutzen Kompression und Expansion von Gas (meist Helium) mit einem Regenerator (Wärmespeicher).

1.  **Gifford-McMahon (GM-Kühler)**
    * **Der "Arbeitsesel"** im Labor.
    * **Aufbau**: Getrennter Kompressor (laut) und Coldhead (leiser). Im Coldhead bewegt sich ein Verdränger kolbenartig.
    * **Leistung**: Zweistufig $\to$ 1. Stufe ~40-80 K, 2. Stufe ~4 K.
    * **Nachteil**: Bewegte Teile im Kalten $\to$ Verschleiß (Dichtungen) und **Vibrationen**.

2.  **Stirling-Kühler**
    * Kompakter Aufbau (Kompressor oft integriert). Hocheffizient.
    * **relevante Verlustfaktoren:**
	    * mechanische Reibung
	    * Druckverlsute 
		    * Durchströmen von Regenarator
		* Längswärmeleitung Außenzylinder
		* Wärmelasten (Kaltteil)
		* Bypass
		* Shuttleverlust
		* Antriebverlust
    * **Einsatz** oft in Infrarot-Kameras (taktisch) oder Satelliten.
    * Carnot: $\frac{T_u - T_0}{T_0}$, real: $\frac{Antriebsleistung}{Kälteleistung}$ 

3.  **Pulsrohrkühler (Pulse Tube Cooler)**
    * **Modernste Variante** (Weiterentwicklung des Stirling/GM).
    * **Prinzip**: Der mechanische Verdränger im Kalten wird durch eine "Gassäule" (akustisches Prinzip) ersetzt. Phasenverschiebung durch Drosseln/Inertanzrohre.
    * **Vorteil**: **Keine bewegten Teile im Kalten** $\to$ extrem vibrationsarm (wichtig für Mikroskopie/Sensoren) und langlebig (Raumfahrt).

---

## 2. Kryovakuumpumpen

Kryopumpen sind **speichernde Vakuumpumpen**. Sie entfernen Gas nicht aus dem Rezipienten, sondern frieren es an kalten Flächen fest.

### Funktionsprinzipien
1.  **Kryokondensation**: Gasteilchen treffen auf eine Fläche, deren Temperatur unter ihrem Dampfdruck liegt $\to$ Eisbildung (für $H_2O, N_2, Ar$).
2.  **Kryosorption**: Für leichte Gase ($H_2, He, Ne$), die bei 10-20 K noch nicht kondensieren. Sie werden an poröser **Aktivkohle** (riesige Oberfläche) physikalisch gebunden (Van-der-Waals-Kräfte).

### Bauarten

#### A. Badkryopumpen
Ein Behälter mit $LHe$ oder $LN_2$ hängt direkt im Vakuum.
* **Vorteil**: "Sauberstes" Vakuum, keine Vibrationen, extrem hohe Saugvermögen.
* **Einsatz**: CERN (LHC Strahlrohr), Weltraumsimulationskammern.

#### B. Refrigerator-Kryopumpen (Standard)
Ein Cryocooler (meist GM-Kühler) kühlt die Pumpflächen ("Cryo-Panels").

* **Aufbau (Zweistufig)**:
    1.  **Stufe 1 (60 - 100 K)**: Äußeres Baffle (Strahlungsschild). Kondensiert **Wasserdampf** (Hauptlast im Vakuum) und $CO_2$.
    2.  **Stufe 2 (10 - 20 K)**: Inneres Panel. Kondensiert **Luft** ($N_2, O_2, Ar$).
        * *Innenseite des Panels:* Beschichtet mit Aktivkohle für Wasserstoff ($H_2$).

> [!WARNING] Betriebsweise: Batch-Betrieb
> Da Kryopumpen das Gas speichern, sind sie irgendwann "voll" (Sättigung).
> **Regeneration**: Die Pumpe muss periodisch erwärmt werden (auf Raumtemperatur), das Gas verdampft und wird von einer Vorpumpe abgepumpt. Danach wieder abkühlen.

### Vor- und Nachteile
* **Pro**: Höchstes Saugvermögen aller Pumpenarten (besonders für Wasserdampf: $> 10.000 \, l/s$). Absolut ölfreies Vakuum.
* **Con**: Muss regeneriert werden (Prozessunterbrechung). Gefährlich bei Stromausfall (schlagartiger Druckanstieg durch verdampfendes Gas).