**Vorlesung**: [[021 Lastmanagement von Wärmepumpen]]
**Thema**: [[023 Lastprofile und Lastbestimmung]]
**Tags**: #Lastprofil #VDI2078 #Kühllast

---

# 01 Lastprofile und Lastbestimmung

Um Lastmanagement zu betreiben, muss man den Bedarf (das Lastprofil) exakt kennen. Nur wer den Bedarf vorhersagen kann, kann Abweichungen (Flexibilität) planen.

## 1. Zusammensetzung der Kühllast
[cite_start]Die Gesamtlast setzt sich aus inneren und äußeren Lasten zusammen[cite: 60].

### Äußere Lasten (Meteorologisch abhängig)
* **Transmission**: Wärmestrom durch Wände/Dach (abhängig von $\Delta T$ Außen/Innen).
* **Strahlung**: Sonneneinstrahlung durch Fenster.
* **Lüftung**: Abkühlung/Entfeuchtung von Frischluft ([[305 Lüftung]]).

### Innere Lasten (Nutzungsabhängig)
* [cite_start]**Personen**: Wärmeabgabe je nach Aktivitätsgrad (50...210 W/Person)[cite: 60].
* **Beleuchtung**: Elektrische Leistung wird zu Wärme.
* **Maschinen/Geräte**: PC, Server, Produktionsmaschinen.
* **Stoffdurchsatz**: Abkühlung von Waren (z.B. im Logistiklager oder Supermarkt).

> [!NOTE] Supermarkt-Beispiel
> [cite_start]Im Supermarkt ist die **Klimatisierung** oft untergeordnet, da die **Kühlmöbel** den Raum bereits stark kühlen (Kältebedarf für TK/NK ist dominierend und ganzjährig hoch)[cite: 60].

## 2. Gleichzeitigkeit (Simultaneität)
Nicht alle Lastspitzen treten gleichzeitig auf.
* **Beispiel**: Die Beleuchtung ist oft an, wenn die Sonne *nicht* scheint.
* Die **Gebäudeträgheit** dämpft Spitzen (Speichermasse der Wände).

## 3. Standardlastprofile vs. Simulation
* **VDI 2078**: Berechnung der Kühllast und Raumtemperaturen.
* **Simulation**: Für komplexe Systeme nötig, um den dynamischen Verlauf über das Jahr (8760 h) abzubilden.