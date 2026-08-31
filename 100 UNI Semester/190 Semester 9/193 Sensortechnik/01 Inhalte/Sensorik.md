---
title: "Sensorik"
type: vorlesung
tags:
  - Sensorik
  - Messmethoden
  - Klausur
erstellt: 2026-05-05
---

# Organisatorisches 
# NICHT KLAUSUR RELEVANT !!!

Im Januar Hausaufgabe Datenanalyse für Messinstrumenten sowie Besichtigung Rossendorf
Mündliche Prüfung 20 min Thema Prozessmesstechnik 

# 📡 Sensortechnik Master-Note

**Modul:** [[Sensorik]] [[Signalverarbeitung]]
**Prüfung:** Mündlich (20 min)

---

## 1. Temperaturmessung
*Definition:* Mittlere kinetische Energie des Systems (keine Bewegung = 0 K).

### Sensortypen

**A. Widerstandsthermometer**
* Nutzt Widerstandsänderung von Metallen (meist **Pt100** / Platin).
* **Messbereich:** -200 bis +850 °C.
* **Achtung:** Eigenerwärmung durch Messstrom beachten! (Vierleiterschaltung nutzen für Genauigkeit).

**B. Thermoelemente (Seebeck-Effekt)**
* Zwei Drähte aus versch. Materialien (z.B. Nickel-Chrom).
* Misst Spannung an der Kontaktstelle.
* *Problem:* Man braucht eine Referenzstelle (Kaltstellenkompensation).

**C. Rauschthermometer**
* Nutzt das thermische Rauschen (Boltzmann-Konstante). Absolutmessverfahren.

---

## 2. Druckmessung
Meist basierend auf **Differenzdruck**.

* **U-Rohr-Manometer:** Hydrostatisches Gleichgewicht (linearer Zusammenhang).
* **Rohrfeder (Bourdon):** Ringförmiges Rohr biegt sich bei Druck auf $\to$ bewegt Zeiger.
* **Piezoresistiv (Modern):**
    * Membran biegt sich durch Druck.
    * Piezokristalle ändern ihren Widerstand (Wheatstone-Brücke zur Kompensation).
    * *Unterschied:* Piezo-Element = Spannung; Piezo-Resistiv = Widerstandsänderung.

---

## 3. Durchflussmessung

> [!important] ⚠️ Klausurrelevant
> Funktionsprinzipien der Durchflussmesser machen **60-70% der Klausur** aus! Genauigkeit sollte ca. 0,5% betragen.

**Wichtige Verfahren:**
1.  [[Wirkdruck Durchflussmessung]] (Blenden, Venturi)
2.  [[Coriolis Durchflussmessung]] (Massenstrom direkt)
3.  [[Wirbel Durchflussmessung]] (Vortex)
4.  [[Magnetisch induktive Durchflussmessung]] (MID, nur leitfähige Fluide)
5.  [[Akustische Durchflussmessung]] (Ultraschall)

**Basisgrößen:** $\dot{V} = A \cdot \bar{v}$ und $\dot{m} = \rho \cdot \dot{V}$.

--- 
## 4. Faseroptische Messung 
[[Faseroptische Sensoren]]
[[Prozessanalysetechnik (PAT)]]

---

## 5. Signalverarbeitung
*Relevant für Vorlesung: Skript bis Seite 73.*

[[Signalverarbeitung]]
