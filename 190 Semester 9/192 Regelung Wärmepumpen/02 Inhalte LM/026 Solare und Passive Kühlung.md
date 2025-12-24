**Vorlesung**: [[Lastmanagement von Wärmepumpen]]
**Thema**: [[LM_VO_06_Solare-passive-Kühlung.pdf]]
**Tags**: #Solar #Adsorption #DEC #LNG

---

# 05 Solare und Passive Kühlung

Ziel: Nutzung von Wärmeüberschüssen (Sommer) zur Kälteerzeugung. "Kühlen, wenn die Sonne scheint".

## 1. Thermisch angetriebene Verfahren
Nutzung von Solarthermie oder Abwärme (KWK) statt Strom.

### Geschlossene Verfahren (Kaltwassererzeugung)
* **Absorptionskältemaschine (AKM):** Flüssiges Sorptionsmittel (z.B. $H_2O/LiBr$). Kontinuierlicher Prozess. Hohe COPs möglich, aber empfindlich bei Rückkühlung.
* **Adsorptionskältemaschine (AdKM):** Festes Sorptionsmittel (z.B. Silikagel/Wasser). Diskontinuierlicher Prozess (Zyklus: Adsorption $\leftrightarrow$ Desorption).
    * **Vorteil:** Kann mit niedrigeren Antriebstemperaturen ($55...90^\circ C$) arbeiten $\to$ Gut für flache Solarkollektoren.

### Offene Verfahren (Luftkonditionierung)
* **DEC (Desiccant Evaporative Cooling):** Klimatisierung direkt durch Luftbehandlung.
    1.  **Trocknung:** Luft strömt durch Sorptionsrad (Silikagel) $\to$ wird trocken und warm.
    2.  **Rückkühlung:** Wärmerückgewinnung kühlt Luft vor.
    3.  **Verdunstung:** Adiabate Befeuchtung kühlt Luft auf Zulufttemperatur.
    4.  **Regeneration:** Sorptionsrad wird mit Solarwärme "getrocknet".

## 2. Nutzung von "Abkälte" (Cold Recovery)
Nutzung von Prozessen, bei denen Kälte als "Abfallprodukt" entsteht.

### Beispiel: LNG-Regasifizierung
LNG (Liquefied Natural Gas) kommt bei $-162^\circ C$ an und muss verdampft werden.
* **Standard:** Verdampfung gegen Meerwasser (Kältevernichtung).
* **Nutzung:** Kälte auskoppeln für Fernkältenetze oder zur Stromerzeugung (ORC-Prozess nutzen: LNG als Senke).
* [cite_start]**Effizienz:** Cold Recovery Ratio (CCR)[cite: 35].
  $$CCR = \frac{P_{net} + \dot{Q}_{cool}}{\dot{m}_{LNG} (h_0 - h_{LNG})}$$

## 3. Passive Maßnahmen (Vermeidung von Kühllast)
Bevor gekühlt wird, sollte die Last reduziert werden.
* **Verschattung:** Außenliegend (effektiver) vs. innenliegend.
* **Nachtlüftung:** Nutzung der Gebäudemasse als Speicher.
* [cite_start]**Sky Cooling:** Nutzung des "Strahlungsfensters" der Atmosphäre (8-13 µm), um Wärme in den Weltraum abzustrahlen (passive Strahlungskühlung, funktioniert auch tagsüber mit speziellen Folien)[cite: 44].