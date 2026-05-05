# 13 Supraleitung E - Magnete & Anwendungen

**Vorlesung**: [[191 Kryotechnik]]
**Datum**: 09.12.2025
**Topics**: [[Magnete]], [[Bitter-Magnet]], [[Quenchschutz]], [[SMES]], [[Magnetscheidung]]
[[Supraleitung+E_Magnete.pdf]]
---
# Erzeugung hoher Magnetfelder

## 1. Resistive Magnete (Normalleitend NL)
Klassische Spulen aus Kupfer oder Aluminium.
* **Prinzip**: Strom fließt durch Leiter $\to$ Magnetfeld.
* **Problem**: Der elektrische Widerstand erzeugt massive Joule'sche Wärme ($P_{el} = R \cdot I^2$ mit $R = \frac{\rho \cdot l}{A}$).
* **Bitter-Magnete** (nach Francis Bitter):
    * Aufbau aus gestapelten Kupferscheiben ("Bitter Plates") mit Isolationszwischenlagen und Kühlkanälen.
    * **Beispiel (Nijmegen)**: 20 Tesla Feld benötigen **6 MW** elektrische Leistung und **300 $m^3/h$** Kühlwasser!
    * Einsatz: Nur in Hochfeldlaboren für kurze Experimente sinnvoll.

## 2. Supraleitende Magnete (SL Magnete)
* **Vorteil**: $R=0$ $\to$ Keine Joule'sche Wärme im Betrieb ($P_{el} = 0$). Nur Kühlleistung für den Kryostaten nötig.
* **Grenzen**: Limitiert durch die kritische Fläche ($T_c, B_{c2}, j_c$).
* **Feldstärke**: 
    * Eisenjoch sättigt bei ca. 2 T.
    * Supraleitende Spulen (Luftspulen) schaffen heute > 23 T (mit HTS sogar mehr).
    * Hybrid-Magnete (Resistiv + SL): bis 45 T.

# Magnetbau & Betrieb

## Spulenformen
* **Solenoid**: Zylinderspule, erzeugt homogenes Feld im Inneren.
* **Toroid**: Ringspule (z.B. Tokamak für Fusion), Feldlinien im Kreis geschlossen.

## Induktivität & Energie
Eine Spule speichert Energie im Magnetfeld:
$$W = \frac{1}{2} L \cdot I^2$$
* Bei großen Magneten (z.B. LHC, ITER) sind das Gigajoule (GJ)!
* **Selbstinduktion**: Beim Laden/Entladen entsteht eine Gegenspannung $U_{ind} = -L \cdot \frac{dI}{dt}$.

## Betriebsmodi
1.  **Laden**: Netzgerät liefert Spannung (z.B. +5V), Strom steigt.
2.  **Dauerstrom (Persistent Mode)**: Supraleitender Schalter überbrückt die Anschlüsse. Netzgerät wird abgeklemmt. Strom kreist widerstandsfrei "ewig".
3.  **Entladen**: Spannung umpolen oder Energie in Widerstand verheizen.

# Quenchschutz (Protection)
Wenn ein Magnet quencht (plötzlich normalleitend wird), wird die gesamte magnetische Energie ($1/2 L I^2$) in Wärme umgewandelt.
* **Gefahr**: Lokale Überhitzung (Hotspots), Spannungsüberschläge, Verdampfen des Heliums (Druckanstieg).
* **Schutzmaßnahmen**:
    * **Quench-Detektion**: Überwachung der Spannung an der Spule.
    * **Dump Resistor**: Externe Lastwiderstände, um die Energie aus dem Kryostaten herauszuholen.
    * **Dioden & Heater**: Unterteilung der Spule in Sektionen mit Bypass-Dioden; aktive Heizer verteilen den Quench schnell auf die ganze Spule, um die Wärmeenergie großflächig zu verteilen.

# Anwendungen

## 1. SMES (Superconducting Magnetic Energy Storage)
Speicherung von Strom direkt als magnetische Energie.
* **Vorteil**: Extrem schnelle Reaktionszeit (ms), hoher Wirkungsgrad.
* **Nachteil**: Teuer, geringe Energiedichte im Vergleich zu chemischen Speichern.
* **Einsatz**: Netzstabilisierung (Flicker-Kompensation, Sekundenreserve), USV für sensible Fabriken (Chipindustrie).

## 2. Magnetscheidung (Magnetic Separation)
Trennung von Materialien aufgrund ihrer magnetischen Eigenschaften (para-/diamagnetisch).
* **OGMS (Open Gradient Magnetic Separation)**: Material fällt durch ein inhomogenes Feld (z.B. seitlicher Abzug).
* **HGMS (High Gradient Magnetic Separation)**:
    * Filtermatrix aus Stahlwolle im Magnetfeld erzeugt extrem hohe lokale Feldgradienten.
    * Anwendung: Reinigung von Kaolin (Porzellanerde) von roten Eisenverunreinigungen oder Abwasserreinigung.