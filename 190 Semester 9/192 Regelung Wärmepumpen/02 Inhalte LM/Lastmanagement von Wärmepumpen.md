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