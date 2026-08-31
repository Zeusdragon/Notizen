---
title: "05 Elektrische Antriebstechnik in der Kälte"
type: vorlesung
tags:
  - Antriebstechnik
  - Asynchronmaschine
  - Frequenzumrichter
  - SternDreieck
erstellt: 2026-05-05
---

**Vorlesung**: [[192 Regelung von Wärmepumpen]]
**Datum**: 2026-01-08

![[Kompendium_ElAnriebstechnik.pdf]]

---

# 05 Elektrische Antriebstechnik in der Kälte

> [!INFO] Zusammenfassung
> Verdichter werden fast ausschließlich durch Elektromotoren angetrieben. Das Verständnis der **Drehstrom-Asynchronmaschine** (DAM) und der **Frequenzumrichter** (FU) ist essentiell, um Regelungskonzepte (Inverter) und Sicherheitsaspekte (Anlaufströme, Motorschutz) zu verstehen.

## 1. Die Drehstrom-Asynchronmaschine (DAM)
Der "Arbeitssel im Maschinenraum". Robust, wartungsarm, kostengünstig.

### Funktionsprinzip
* **Stator:** Erzeugt ein magnetisches Drehfeld, das mit der **Synchrondrehzahl $n_s$** rotiert.
  $$n_s = \frac{f \cdot 60}{p}$$
  *($f$: Netzfrequenz, $p$: Polpaarzahl)*
* **Rotor (Kurzschlussläufer):** Das Drehfeld induziert eine Spannung im Rotor $\to$ Strom fließt $\to$ Lorentzkraft entsteht.
* **Schlupf $s$:** Damit Induktion stattfindet, muss der Rotor *langsamer* drehen als das Feld.
  $$s = \frac{n_s - n}{n_s} \approx 3 \dots 8\%$$

> [!NOTE] Drehzahl-Beispiel (50 Hz Netz)
> * **2-polig ($p=1$):** $n_s = 3000 min^{-1}$ $\to$ $n_{nenn} \approx 2900 min^{-1}$ (Schnellläufer, z.B. Schraube).
> * **4-polig ($p=2$):** $n_s = 1500 min^{-1}$ $\to$ $n_{nenn} \approx 1450 min^{-1}$ (Standard Hubkolben).

## 2. Anlaufverfahren
Der direkte Anlauf eines Motors zieht den **5- bis 8-fachen Nennstrom** ($I_A \approx 5\dots8 \cdot I_N$). Das führt zu Netzspannunngseinbrüchen (Lichtflackern).

### A. Direktstart (DOL - Direct On Line)
* Motor wird hart ans Netz geschaltet.
* **Anwendung:** Nur für kleine Verdichter zulässig.
* **Moment:** Sehr hohes Anlaufmoment (gut für Start unter Last).

### B. Stern-Dreieck-Anlauf (Y-$\Delta$)
Der Klassiker zur Strombegrenzung.
1.  **Stern-Stufe (Y):** Spannung an der Wicklung um Faktor $\sqrt{3}$ reduziert ($400V \to 230V$).
    * Strom sinkt auf 1/3.
    * **Aber:** Drehmoment sinkt auch auf 1/3! (Verdichter muss druckentlastet anlaufen).
2.  **Umschaltung:** Nach Hochlaufzeit wird auf Dreieck ($\Delta$) umgeschaltet (volle Spannung).

### C. Softstarter (Sanftanlaufgerät)
* **Prinzip:** Phasenanschnittsteuerung mittels Thyristoren.
* Spannung (und damit Moment) wird stetig hochgefahren ("Rampe").
* **Vorteil:** Verschleißarmer Start, kein Umschaltstromstoß wie bei Y-$\Delta$.
* **Nachteil:** Keine Drehzahlregelung im Betrieb möglich (nur Start/Stopp).

## 3. Frequenzumrichter (FU)
Ermöglicht die variable [[Drehzahlregelung]] ([[Verdichterregelung]]).

### Aufbau
1.  **Gleichrichter:** Macht aus AC (Netz) $\to$ DC.
2.  **Zwischenkreis:** Glättet die Gleichspannung (Kondensatoren).
3.  **Wechselrichter:** Erzeugt aus DC eine neue AC-Spannung mit variabler Frequenz $f$ und Amplitude $U$ (PWM - Pulsweitenmodulation).

### U/f-Kennlinie
Um das Drehmoment konstant zu halten, muss der magnetische Fluss $\Phi$ konstant bleiben. Da $\Phi \sim U/f$, muss bei sinkender Frequenz auch die Spannung gesenkt werden.
* **Nennpunkt (50 Hz):** Volle Spannung (400V).
* **Feldschwächbereich (> 50 Hz):** Spannung kann nicht weiter steigen (begrenzt durch Netzspannung). Bei steigender Frequenz sinkt nun der Fluss $\Phi$.
    * $\to$ Drehmoment nimmt ab ($M \sim 1/f$).
    * $\to$ Leistung bleibt konstant ($P = const$).

> [!WARNING] Kühlung bei niedrigen Drehzahlen
> Eigenbelüftete Motoren (Lüfterrad auf Welle) überhitzen bei kleinen Drehzahlen (< 25-30 Hz), da der Luftstrom fehlt.
> **Lösung:** Fremdlüfter installieren oder Kaltleiter-Überwachung (PTC).

## 4. Motorschutz
Schutz vor thermischer Zerstörung der Wicklungsisolierung.

1.  **Motorschutzschalter (MSS):** Bimetall löst aus, wenn Strom dauerhaft zu hoch (Schutz vor Überlast & Phasenausfall). Reagiert träge.
2.  **Kaltleiter (PTC/Thermistor):** Temperaturfühler direkt *in* der Wicklung.
    * Widerstand springt bei Nennansprechtemperatur (NAT) sprunghaft an.
    * **INT69**: Standard-Auswertegerät in der Kältetechnik (Kriwan). Schaltet Verdichter bei Überhitzung sofort ab.

## 5. Effizienzklassen (IE)
Vorschrift für Motoren (Ökodesign-Richtlinie).
* **IE1 / IE2:** Veraltet (Standard / High Eff.).
* **IE3:** Premium Efficiency (Heute Standard in der EU ab 0,75 kW).
* **IE4 / IE5:** Super Premium (meist nur mit Permanentmagnet-Motoren / EC-Technik erreichbar).