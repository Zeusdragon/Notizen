**Vorlesung**: [[Lastmanagement von Wärmepumpen]]
**Thema**: [[LM_VO_04_Kältespeicher_Netze_Träger_OPAL.pdf]]
**Tags**: #Speicher #Eisspeicher #PCM #Stratifikation

---

# 04a Kältespeicher Technologien

Speicher sind das zentrale Element zur Entkopplung von Kälteerzeugung und -bedarf. Sie ermöglichen Spitzenlastmanagement (Peak Shaving) und die Nutzung volatiler Strompreise.

## 1. Klassifizierung der Speicher

### A. Sensible Kältespeicher (Wasser)
Nutzen die fühlbare Wärme ($Q = m \cdot c_p \cdot \Delta T$).
* **Medium:** Wasser (hohe Wärmekapazität $4,19 \, kJ/kgK$, billig, ungiftig).
* **Bauform:** Schichtenspeicher (Stratified Tank).
    * **Funktionsprinzip:** Trennung von warmem Rücklauf (oben) und kaltem Vorlauf (unten) durch Dichteunterschiede.
    * **Kennzahl:** **Richardson-Zahl $Ri$**. Beschreibt das Verhältnis von Auftrieb zu Mischungsimpuls. [cite_start]Hohe $Ri$ = stabile Schichtung[cite: 3].
* **Diffusoren:** Entscheidend für die Einströmung, um Turbulenzen (Mischung) zu vermeiden. Radiale Diffusoren reduzieren die Geschwindigkeit $v_{in}$.

### B. Latente Kältespeicher (PCM)
Nutzen die Schmelzenthalpie beim Phasenwechsel ($Q = m \cdot \Delta h_{fus}$).
* **Vorteil:** Hohe Energiedichte bei geringer Temperaturspreizung (isotherme Beladung/Entladung).
* **Nachteil:** Oft geringe Wärmeleitfähigkeit des Speichermediums (benötigt große Übertragerflächen).

#### 1. Eisspeicher (Phasenwechsel Wasser/Eis)
Der "Klassiker" der Latentspeicher.
* [cite_start]**Schmelzenthalpie:** $333 \, kJ/kg$[cite: 3].
* **Systemarten:**
    1.  **Ice-on-Coil (Eisaufbau an Rohren):**
        * Kältemittel/Sole fließt durch Rohrbündel im Wasserbecken.
        * **Beladen:** Eis wächst am Rohr (Wärmewiderstand steigt mit Eisdicke $\to$ COP sinkt am Ende der Ladung).
        * **Entladen:** Abschmelzen von innen (Internal Melt) oder außen (External Melt).
    2.  **Ice Harvester (Eiserzeuger):**
        * Eis wird an Platten erzeugt und periodisch "abgesprengt" (Hot Gas Defrost).
        * Eis fällt in einen Silo (Slurry).
        * **Vorteil:** Trennung von Erzeuger-Leistung und Speicher-Kapazität. Sehr hohe Entladeleistung möglich.
    3.  **Verkapseltes Eis (Kugelspeicher):**
        * Wasser in Kunststoffkugeln ("Dimple Balls"), die von Sole umströmt werden.
        * Großflächiger Wärmeübergang, einfaches Nachrüsten in Tanks.

#### 2. Eutektische Salze & Paraffine
Für Temperaturen $\neq 0^\circ C$.
* **Eutektika:** Salzhydrate. Punktgenauer Schmelzpunkt (z.B. $-10^\circ C$ oder $+8^\circ C$).
* **Paraffine:** Organische PCMs. Kein Supercooling, chemisch stabil, aber brennbar und teurer.

## 2. Betriebsstrategien
Wie wird der Speicher in das Lastmanagement integriert?

1.  **Vollspeicherung (Full Storage):**
    * Die gesamte Tageskühllast wird in der Nacht (Niedertarif/Windstrom) erzeugt.
    * Kältemaschine (KM) läuft tagsüber **nicht**.
    * **Nachteil:** Sehr großer Speicher und große KM nötig.
2.  **Teilspeicherung (Partial Storage) - Lastnivellierung:**
    * KM läuft 24h durchgängig auf mittlerer Last.
    * Tagsüber deckt Speicher die Spitzen; Nachts wird Speicher geladen.
    * **Vorteil:** Kleinste KM-Auslegung (Investitionskosten $\downarrow$).
3.  **Teilspeicherung - Bedarfsbegrenzung:**
    * KM deckt Grundlast, Speicher deckt nur die Spitzen ("Spitzenkappung").

--- 
**Vorlesung**: [[Lastmanagement von Wärmepumpen]]
**Thema**: [[LM_VO_04_Kältespeicher_Netze_Träger_OPAL.pdf]]
**Tags**: #Fernkälte #Hydraulik #Kälteträger #IceSlurry

---

# 04b Kältenetze und Kälteträger

## 1. Fernkälte (District Cooling)
Versorgung mehrerer Gebäude/Liegenschaften mit zentral erzeugter Kälte.

### Netz-Topologien
* **Direkte Einspeisung:** Kälteträger des Netzes fließt direkt durch die Gebäude-Kühler (selten, da hohe Drücke im Hausnetz).
* **Indirekte Einspeisung:** Hydraulische Trennung über Plattenwärmeübertrager (Heat Exchanger HEX).
    * **Vorteil:** Drucktrennung, Leckagesicherheit, klare Eigentumsgrenzen.
    * **Nachteil:** Temperaturverlust am HEX (ca. $1...2\,K$ $\to$ Vorlauf muss kälter sein).

### Vorteile der Fernkälte
* **Gleichzeitigkeitsfaktor:** Nicht alle Kunden rufen Maximallast gleichzeitig ab $\to$ Zentrale kann kleiner ausgelegt werden.
* **Effizienz:** Großkältemaschinen (Turboverdichter) haben bessere COPs ($> 5...7$) als kleine Split-Geräte.
* [cite_start]**Synergien:** Nutzung von "Free Cooling" (Flusswasser, Nachtauskühlung) oder Abwärme (Absorptionskälte) im großen Stil möglich[cite: 3].

## 2. Kälteträger (Sekundärfluide)
Das Fluid, das die Kälte im Netz transportiert. Anforderungen: Hohe Wärmekapazität, niedrige Viskosität, Korrosionsschutz.

### Typen
1.  **Wasser:** Bester Träger, aber nur bis $0^\circ C$ nutzbar.
2.  **Glykol-Gemische (Ethylen/Propylen):**
    * Frostschutz bis $-20^\circ C$ oder tiefer.
    * **Nachteil:** Viskosität steigt stark an $\to$ Pumpleistung steigt ($P_{pump} \sim \dot{V} \cdot \Delta p$). Wärmekapazität sinkt ($c_p < c_{p,Water}$).
3.  **Salzsolen (Calciumchlorid etc.):**
    * Sehr billig, sehr tiefe Temperaturen möglich.
    * **Nachteil:** Extrem korrosiv (nur mit Inhibitoren und speziellen Materialien wie Titan-HEX oder Kunststoffrohren).
4.  **Ice Slurry (Binäres Eis):**
    * Gemisch aus Wasser, Gefrierpunktverbesserer und kleinen Eiskristallen.
    * **Phase:** Flüssig/Fest (pumpfähig).
    * **Vorteil:** Nutzt latente Enthalpie des Eises *während* des Transports. Energiedichte 3-4x höher als Wasser $\to$ Kleinere Rohrdurchmesser oder Pumpen.

## 3. Hydraulische Schaltungen
* **Drosselschaltung (Variable Flow):** 2-Wege-Ventile beim Verbraucher. Netzpumpe ist drehzahlgeregelt (hält $\Delta p$ konstant). Energiesparend.
* **Umlenkschaltung (Constant Flow):** 3-Wege-Ventile. Volumenstrom im Netz konstant, Rücklauftemperatur schwankt ("Kältetod" bei schlechter Auslegung). Veraltet.