**Vorlesung**: [[192 Regelung von Wärmepumpen]]
**Datum**: 16.10.2025
**Tags**: #Lastabwurf #Residuallast #Abtauung #Regelstrategien #Wärmepumpe

---

> [!important] 🪧 Goldene Regel
> **Hydraulik-Kreis zuerst anmachen, dann erst um die Maschine kümmern!**

# 1. Residuallast & Entscheidung
* **Residuallast** = Bedarf - Regenerative Erzeugung (Sonne/Wind).
* Stromverbrauch ist unabhängig von Erzeugung -> WP als Flexibilität.

**Entscheidung Schalthandlung:**
![[Pasted image 20251016170706.png|400]]
*(Entscheidungshilfe für Schalthandlung)*

---

# 2. Regelungsvarianten (Verdichter)
*Ziel:* Verdichter muss an Expansionsorgan angepasst sein, sonst Imbalanz.

## A. Heißgasbypassschaltung
* **Funktion:** Magnetventil zur Verdampfungsdruckhaltung. Heißgas wird von *nach Verdichter* zu *vor Verdampfer* (an Punkt 4) eingespritzt.
* **Status:** 🛑 Veraltet (Nicht nutzen!)

| Vorteile ✅                       | Nachteile ❌           |
| :------------------------------- | :-------------------- |
| Im gesamten Regelbereich nutzbar | **Sehr ineffizient**  |
| Kostengünstig & Selbsttätig      | Benötigt mehr Heißgas |

## B. Verdampfungsdruckregler
* **Funktion:** Hält Verdampfungsdruck ohne Verdichterregelung. Absenkung des Drucks auf $p_0^*$ -> Erhöhte technische Arbeit.

| Vorteile ✅ | Nachteile ❌ |
| :--- | :--- |
| Kostengünstig | Regelbereich begrenzt |
| Keine Verdichter-Adaption nötig | Erhöhte Belastung (Hub) |
| Für mehrere Verdampfer nutzbar | **Niedrige Energieeffizienz** |

## C. Taktbetrieb (On/Off)
* **Ziel:** Reduzierung mittlerer Volumenstrom durch zeitweises Abschalten.

> [!warning] Problematik
> Führt zu diskontinuierlichem Betrieb und ungenauer Regelgüte.

## D. Verdichterhubvolumen ändern (Zylinderabschaltung)
* **Methode:** Bänke abschalten oder Auslassventil offen halten (bei Hubkolben).

| Vorteile ✅ | Nachteile ❌ |
| :--- | :--- |
| Sehr großer Regelbereich | Mechanisch komplex |
| Energieeffizient | Potenzielle Disbalanz (Vibration) |
| | Lebenszeitverringerung |

---

# 3. Abtauung
**Problem:** Luftfeuchtigkeit friert am Verdampfer (Luft-Seite) an -> Wärmeübergang verschlechtert sich.

## Methoden im Vergleich

### Heißgasabtauung
* **Prinzip:** Dreieckschaltung. Heißes Gas direkt in den Verdampfer.
* **Vorteil:** Schnell.

### Kaltgasabtauung
* **Prinzip:** Gas nach Kondensator (aus Sammler) vor Verdampfer leiten.
* **Nachteil:** Weniger Überhitzungswärme verfügbar -> dauert länger als Heißgas.

### Kreislaufumkehr (4-Wege-Ventil) 🏆
* **Prinzip:** Der Prozess wird umgedreht. Der Verdampfer wird zum Kondensator (heizt).
* **Achtung:** Bidirektionale Ventile nötig oder Umschaltung der Leitungen.

> [!success] Bewertung
> * **Sehr viel schneller** als andere Methoden.
> * **Energetisch effizient.**
> * *Risiko:* Wärmequelle für Abtauung ist kurzzeitig der Pufferspeicher -> Kunde darf Abkühlung nicht merken.

![[Pasted image 20251030153813.png|500]]