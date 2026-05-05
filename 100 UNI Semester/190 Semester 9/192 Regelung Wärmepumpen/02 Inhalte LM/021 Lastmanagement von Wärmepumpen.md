# ⚡ Lastmanagement von Wärmepumpen

**Modul:** [[192 Regelung von Wärmepumpen]]
**Tags:** #Lastmanagement #Residuallast #SmartGrid #Flexibilität
**Status:** 🟡 In Bearbeitung

---

## 1. Lastprofilberechnung & Grundlagen
*Hier kommen die spezifischen Inhalte aus deiner Datei [[Lastprofilberechnung]] hin.*

* **Ziel:** Vorhersage des thermischen Bedarfs (Heizlast + Warmwasser) über die Zeit.
* **Relevanz:** Nur wenn das Lastprofil bekannt ist, kann man Abweichungen (Flexibilität) planen.

---

## 2. Residuallast & Flexibilität
Das Lastmanagement orientiert sich oft an der **Residuallast** des Stromnetzes (oder des eigenen Hauses mit PV).

$$\text{Residuallast} = \text{Strombedarf} - \text{Regenerative Erzeugung (Sonne/Wind)}$$

* **Strategie:**
    * **Positive Residuallast (Wenig Strom):** Lastabwurf (WP aus/drosseln).
    * **Negative Residuallast (Strom-Überschuss):** Lastverschiebung (WP an, Speicher überhitzen).

**Entscheidungsfindung:**
Die Schalthandlung basiert auf dem Abgleich von *Bedarf* vs. *Verfügbarkeit*.
![[Pasted image 20251016170706.png|400]]

---

## 3. Technische Umsetzung (Aktorik)
Wie kann die Wärmepumpe konkret auf Lastanforderungen reagieren?

### A. Lastabwurf (Abschaltung)
* **Methode:** Hartes Abschalten (Sperrzeit) oder Taktbetrieb.
* **Nachteil:** Komfortverlust, Verschleiß durch Takten, Wiederanlauf erfordert Energie.

### B. Leistungsanpassung (Modulation)
* **Drehzahlregelung (Inverter):** Eleganteste Methode. Leistung wird exakt dem verfügbaren Strom oder Bedarf angepasst.
* **Verdichter-Hubvolumen:** Zylinderbank-Abschaltung (bei großen Hubkolbenverdichtern).
* **Bypass-Regelung:** Energetisch ineffizient, daher für Lastmanagement ungeeignet.

---

## 4. Anlagen-Voraussetzungen
Damit Lastmanagement funktioniert, muss die Anlage **flexibel** und **steuerbar** sein.

* **Systemträgheit:** Da Kälte/Wärme nicht "additiv" sofort erzeugt werden kann (wie bei einem Heizstab), muss das System (Gebäudemasse, Pufferspeicher) als Puffer dienen.
* **Steuerungstechnik:**
    * **Analog:** Ungeeignet (kann keine externen Signale/Prognosen verarbeiten).
    * **Digital (SPS/Controller):** Notwendig für Kommunikation mit Smart Grid / PV-Anlage.

---
# Map of Content

# Lastmanagement von Kälte- und Klimaanlagen (Master-Note)

> [!INFO] Übersicht
> Zentrale Anlaufstelle für das Modul Lastmanagement. Hier laufen alle Themenstränge zusammen.

## Struktur der Vorlesung

### Grundlagen & Bedarf
* **[[023 Lastprofile und Lastbestimmung]]**: Lastzusammensetzung (VDI 2078), Simultaneität, Nutzungsarten.
* **[[022 Flexible Nutzung von Kälte- und Klimaanlagen]]**: Residuallast, Bivalente Systeme (Power-to-Cold), Regelungsstrategien.

### Technologien & Infrastruktur (Deep Dive)
* **[[024 Speichertechnologien und Netze]]**: Detaillierte Betrachtung von sensiblen und latenten Speichern (Eis, PCM). Physik & Bauarten. 
* Fernkälte, Hydraulik, Kälteträgerfluide (Sole, Ice-Slurry) und Netztopologien.

### Erweiterte Konzepte
* **[[025 Potentiale und Hemmnisse]]**: Wirtschaftlichkeit, Zwiebel-Modell der Potentiale.
* **[[026 Solare und Passive Kühlung]]**: Sorptionsgestützte Klimatisierung, DEC-Anlagen, Nutzung von LNG-Kälte.

---
## Wichtige Formeln & Kennzahlen

* [cite_start]**Cold Recovery Ratio (LNG):** $CCR = \frac{P_{net} + \dot{Q}_{cool}}{\dot{m}_{LNG} (h_0 - h_{LNG})}$ [cite: 35]
* **Energiedichte Eis:** $q_{lat} = 333 \, kJ/kg$ (vs. Wasser $\Delta T=6K \approx 25 \, kJ/kg$).