**Vorlesung**: [[192 Regelung von Wärmepumpen]] (Regelung von Kälteanlagen)
**Datum**: 2026-01-15
**Thema**: Regelungsverfahren, VRF-Systeme, Supermarkt-Anwendungen
**Tags**: #Regelung #VRF #Multisplit #Supermarkt #PID

![[RKA_05_Regelungverfahren.pdf]]

---

# 05 Regelungsverfahren in der Kältetechnik

> [!INFO] Kerninhalte
> Diese Einheit behandelt die praktische Umsetzung von Regelkreisen in komplexen Kälteanlagen (z.B. VRF, Supermarkt) und Strategien zur Effizienzsteigerung (VRT, Wärmerückgewinnung).

## 1. Regelungsaufgaben & Standard-Regelkreise

In modernen Kälteanlagen und Wärmepumpen laufen mehrere Regelkreise parallel. Die Herausforderung ist die **Entkopplung** dieser Kreise, um Schwingungen zu vermeiden.

### Die wichtigsten Regelgrößen
* **Volumenstromregelung (Sekundärseite):**
    * **Ziel:** Einhaltung der Vorlauftemperatur ($T_{Vorlauf}$) oder Spreizung.
    * **Stellglied:** Pumpe (Drehzahl) oder Ventil.
* **Überhitzungsregelung (Verdampfer):**
    * **Ziel:** Schutz des Verdichters vor Flüssigkeitsschlägen und optimale Füllung des Verdampfers.
    * **Führungsgröße:** Überhitzung $\Delta T_{überh}$ (typisch 5-7 K).
    * **Stellglied:** Elektronisches Expansionsventil (EXV).
* **Kapazitätsregelung (Leistung):**
    * **Ziel:** Anpassung der Kälteleistung an den Bedarf (Teillast).
    * **Stellglied:** Verdichterdrehzahl (Inverter).

> [!WARNING] Stabilität (MSS-Kurve)
> Verdampfer neigen bei zu kleiner Überhitzung zum Schwingen ("Hunting").
> * **MSS (Minimum Stable Signal):** Die minimal mögliche Überhitzung, bei der der Regelkreis noch stabil arbeitet. Moderne Regler versuchen, dieser Kurve zu folgen, ohne sie zu unterschreiten.

---

## 2. PID-Regler in der Praxis

Obwohl theoretische Modelle (Laplace) existieren, werden Regler in der Praxis oft **empirisch** oder mit **Faustformeln** eingestellt.

* **Faustformeln:** Methoden wie *Ziegler-Nichols* oder *Chien, Hrones & Reswig* dienen als Startwerte.
* **Einstell-Reihenfolge:**
    1.  **P-Anteil:** Geschwindigkeit (Vorsicht: zu hoch $\to$ Überschwingen) .
	    1. Je Höher desto mehr Schwingen aber näher am sollwert am Ende 
	    2. Zeit bis Sollwert Erreicht Stabil dauert lange
    2.  **I-Anteil:** Beseitigt bleibende Regelabweichung (Vorsicht: macht das System träge/instabil).
    3.  **D-Anteil:** Reagiert auf Änderungen (selten in der Kältetechnik genutzt, da rauschempfindlich).
+ Verfahren:
	1. Erst P-Anteil machen
	2. I-Anteil dazu
	3. D-Anteil dazu

---

## 3. Multi-Split und VRF-Systeme

**VRF (Variable Refrigerant Flow)** bzw. **VRV** (Variable Refrigerant Volume - Daikin) sind Direktverdampfungssysteme, die ein Außengerät mit vielen Innengeräten verbinden.

### Konzepte
1.  **2-Leiter-System:**
    * Alle Innengeräte müssen entweder **Kühlen ODER Heizen**.
    * Wechsel über Kreislaufumkehr (4-Wege-Ventil).
2.  **3-Leiter-System (Heat Recovery):**
    * Gleichzeitiges **Kühlen UND Heizen** in verschiedenen Räumen möglich.
    * **Funktion:** Ein Raum wird gekühlt, die entzogene Wärme wird nicht nach außen abgegeben, sondern über eine "Hot Gas"-Leitung in einen anderen Raum zum Heizen transportiert (Verschiebung von Energie im Gebäude).
    * **Effizienz:** Sehr hoch, da Abwärme genutzt wird.

### Effizienzsteigerung: VRT (Variable Refrigerant Temperature)
Anstatt die Verdampfungs- ($T_0$) und Verflüssigungstemperatur ($T_c$) konstant auf "Worst-Case"-Werten zu halten (z.B. $6^\circ C / 45^\circ C$), werden diese **gleitend an die Last angepasst**.
* **Teillast:** Die Verdampfungstemperatur wird angehoben (z.B. auf $10^\circ C$), da die Wärmetauscherflächen relativ zur Last "größer" sind.
* **Vorteil:** Der Verdichterhub sinkt $\to$ **COP steigt massiv**.

### Hybride VRF-Systeme (HVRF)
Kombination aus Kältemittel-Außengerät und wassergeführten Innengeräten.
* **Vorteil:** Kein Kältemittel in den Hotelzimmern/Büros (Sicherheit, keine EN 378 Prüfung im Raum nötig).
* **HBC-Controller:** Tauscht Wärme zwischen Kältemittel und Wasser zentral aus.

---

## 4. Kältemittel & Sicherheit (R-410A vs. R-32)

Der Markt bewegt sich von R-410A (hohes GWP > 2000) zu **R-32** (GWP 675).

* **R-32 (Difluormethan):**
    * **Klasse A2L:** "Schwer entflammbar" (Low Flammability).
    * **Sicherheitsmaßnahmen:** Bei VRF-Systemen mit R-32 müssen in kleinen Räumen oft Sensoren und Absperrventile installiert werden, um bei Leckage die Zündgrenze nicht zu überschreiten.
* **Alternative R-290 (Propan):** Wegen Brennbarkeit (A3) in Multi-Split-Systemen im Gebäude kaum einsetzbar.

---

## 5. Supermarkt-Anwendungen (Verbundanlagen)

*(Basierend auf den Folien am Ende der Präsentation)*

Supermärkte nutzen oft **$CO_2$-Booster-Anlagen** oder Verbundsysteme.

* **Herausforderung Winterbetrieb:**
    * Wenn der Markt z.B. umgebaut wird oder die Abwärme der Kühlmöbel im Winter nicht reicht, muss die Anlage heizen.
    * **Lösung:** Die Kälteanlage wird als **Luft-Wärmepumpe** betrieben (Verdampfer außen nimmt Wärme auf, Abwärme wird in den Markt gepumpt).
* **Wärmerückgewinnung (WRG):** Die Abwärme der Kälteanlage (Enthitzung/Kondensation) wird fast immer zur Beheizung des Marktes oder für Warmwasser genutzt.