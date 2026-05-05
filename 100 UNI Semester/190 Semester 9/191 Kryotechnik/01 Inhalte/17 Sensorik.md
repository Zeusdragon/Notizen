**Vorlesung**: [[191 Kryotechnik]]
**Datum**: 2026-01-13
**Topics**: [[Thermometrie]], [[Füllstandsmessung]], [[Wärmeeintrag]], [[Cernox]]


![[17 Sensorik.pdf]]

---
# 17 Kryogene Sensorik

> [!INFO] Herausforderung
> Bei tiefen Temperaturen ist die Messtechnik oft die größte Wärmequelle.
>  Das Ziel ist: 
> + **Minimierung des Wärmeeintrags** durch die Zuleitungen bei gleichzeitiger Genauigkeit.
> + Schlechte Zugänglichkeit und keine direkte Beobachtung möglich

## 1. Temperaturmessung (Thermometrie)

### Temperaturskalen
+ **IPTS-68**: Fixpunkte Tripelpunkte und Sättigungstemperaturern 
* **ITS-90**: Internationale Temperaturskala von 1990 (Definiert durch Fixpunkte wie Tripelpunkte von $H_2, Ne, O_2$) insgesamt 17 Fixpunkte ab 0,65 K.
* **PLTS-2000**: Erweiterung für tiefste Temperaturen (0,9 mK ... 1 K) basierend auf dem Schmelzdruck von $^3He$.

### Primäre Temperaturmessung
+ **Gasthermometer**: geht auf ideal Gas zurück
+ **Dampfdruckthermometer**: genauso wie Gasthermometer allerdings nun im behälter ein Zweiphasengemisch nutzung der Dampfdruckkurve Messung zwischen Tripelpunkt und kritischen Punkt des Gemisches
	+ je nach Gemisch verschiedene Temperaturbereiche möglich
	+ Messung des Drucks und daraus kommt man auf Temperatur
	+ **Nachteil** : Lücken, Experimentier Aufbau begrenzter Temperaturbereiche
+ **weitere:** Magnet Thermometer, Rauschthermometer, Kernresonanz, akut. Thermometer
	+ oft nicht genutzt da hoher Messaufwand
### Sekundärthermometer (Widerstandsthermometer)
Man misst den elektrischen Widerstand $R(T)$.

| Typ            | Material                 | Bereich       | Eigenschaften                                                                         |
| :------------- | :----------------------- | :------------ | :------------------------------------------------------------------------------------ |
| **Metalle**    | **Pt100 / Pt1000**       | > 30 K        | Standard, linear, genormt. Unter 30 K unbrauchbar (Restwiderstand).                   |
|                | **RhFe** (Rhodium-Eisen) | 0,5 ... 300 K | Sehr stabil und präzise, aber teuer.                                                  |
| **Halbleiter** | **Si-Diode**             | 1 ... 400 K   | Standard-Sensor. Hohe Empfindlichkeit bei tiefen T. **Nachteil**: Magnetfeldabhängig! |
|                | **Cernox** (Zr-Nitrid)   | 0,1 ... 300 K | **Industriestandard Kryotechnik**. Kleiner Magnetfeldfehler, strahlungsresistent.     |
|                | **Germanium / RuO2**     | < 1 K         | Für tiefste Temperaturen da dort Linearer Zusammenhang Widerstand ~ Temperatur.       |

> [!WARNING] Thermische Verankerung (Heat Sinking)
> Da die Zuleitungsdrähte Wärme von Raumtemperatur (300 K) leiten, zeigt der Sensor oft eine zu hohe Temperatur an.
> **Lösung**: Die Drähte müssen vor dem Sensor fest auf das kalte Niveau thermisch angekoppelt werden (umwickeln von Kupferpfosten, Kleben mit Varnish). Abfangen mit 80K ca. halbierung wärmeeintrag
### wichtige Kriterien 
+ **Magnetfeldempfindlichkeit**
+ **Strahlungsempfindlichkeit**
## 2. Füllstandsmessung (Level Gauging)

### Methoden
1.  **Differenzdruck ($\Delta p$)**: Messung der hydrostatischen Säule.
    $$\Delta p = \rho_{fl} \cdot g \cdot h$$
    * *Problem*: Bei LHe sehr ungenau, da Dichte extrem gering ($125 kg/m^3$) und Gasdichte relativ hoch.
2.  **Kapazitiv**: Koaxialer Kondensator taucht in Flüssigkeit.
    * Nutzung der unterschiedlichen Dielektrizitätszahl $\varepsilon_r$ von Flüssigkeit und Gas.
3.  **Supraleitender Draht (Nur LHe)**:
    * Ein NbTi-Draht wird gespannt. Im Gas ist er normalleitend ($R > 0$, wird durch Strom geheizt), in der Flüssigkeit ist er supraleitend ($R=0$).
    * Der Gesamtwiderstand ist proportional zur Länge des Teils im Gas $\to$ Füllstand.
4.  **Wägung**: Wiegen des gesamten Dewars (Genauigkeit heute sehr gut).

## 3. Durchflussmessung
Schwierig bei kryogenen Fluiden (Zweiphasenströmung, Kalibrierung).
* [[Coriolis Durchflussmessung|Coriolis]]: Massenstrommessung (sehr genau, aber Druckverlust, teuer).
* [[Wirkdruck Durchflussmessung|Venturi/Oriface]]: Differenzdruck mit Venturi (Standard).
* **Turbine**: Nur für einphasige Flüssigkeiten ($LN_2, LNG$).