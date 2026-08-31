---
title: "02 Hauptsätze der Thermodynamik"
type: vorlesung
erstellt: 2026-05-05
---

# 02 Hauptsätze der Thermodynamik

**Vorlesung**: [[191 Kryotechnik]]
**Datum**: 
**Topics**: [[Thermodynamik]], [[Carnot]], [[Effizienz]]
![[02 Hauptsätze.pdf]]

---

# Die Hauptsätze
1. **0. Hauptsatz**: Thermisches Gleichgewicht.
2. **1. Hauptsatz**: Energieerhaltung ($U$, $H$).
3. **2. Hauptsatz**: Entropie $S$, Richtung von Prozessen.
4. **3. Hauptsatz**: Unerreichbarkeit des absoluten Nullpunkts ($S \to 0$ für $T \to 0$).

# Leistungszahl & Carnot
Der ideale Vergleichsprozess ist der **Carnot-Prozess**.

**Leistungszahl (COP - Coefficient of Performance):**
$$\epsilon = COP = \frac{\text{Nutzen}}{\text{Aufwand}} = \frac{\dot{Q}_0}{P}$$

**Idealer Carnot-COP:**
$$\epsilon_c = \frac{T_0}{T_u - T_0}$$
* $T_0$: Kältetemperatur
* $T_u$: Umgebungstemperatur (z.B. 300 K)

> [!important] Das Carnot-Diktat
> Je tiefer die Temperatur $T_0$, desto kleiner der ideale COP und desto höher der Energieaufwand.
> * Bei 80 K ($LN_2$) theoretisch 3 W Aufwand für 1 W Kälte.
> * Bei 4 K ($LHe$) theoretisch 70+ W Aufwand für 1 W Kälte.

# Effizienz (Gütegrad)
Verhältnis von realem COP zu idealem Carnot-COP:
$$\eta = \frac{COP_{real}}{COP_{ideal}}$$
* Typische Werte in der Kryotechnik: $\eta \approx 10 \dots 35 \%$.
* **Strobridge-Diagramm**: Zeigt, dass die Effizienz ("Percent Carnot") stark von der Anlagengröße abhängt (große Anlagen sind effizienter), weniger von der Temperatur.