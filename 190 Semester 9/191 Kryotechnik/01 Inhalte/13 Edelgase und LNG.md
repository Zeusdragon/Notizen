**Vorlesung**: [[191 Kryotechnik]]
**Datum**: 2026-01-06
**Topics**: [[Edelgase]], [[LNG]], [[Isolierglas]], [[Luftzerlegung]]

---
![[13 Edelgase.pdf]]
# 13a Edelgase

> [!INFO] Übersicht
> Edelgase sind wertvolle Nebenprodukte der Luftzerlegung. Aufgrund ihrer speziellen physikalischen Eigenschaften (Reaktionsträgheit, niedrige Wärmeleitfähigkeit, Leuchterscheinungen) haben sie Nischenanwendungen. Zudem wird Erdgas (LNG) als kryogener Brennstoff immer wichtiger.

## 1. Die Edelgase (Gewinnung & Anwendung)
Gewinnung fast ausschließlich durch **kryogene Rektifikation** von Luft (außer Helium).

| Gas | Anteil in Luft | $T_s$ [K] | Besonderheit | Anwendung |
| :--- | :--- | :--- | :--- | :--- |
| **Helium** ($He$) | 5,2 ppm | 4,2 | Aus Erdgas (nicht Luft!) | Kühlung (Supraleitung), Ballongas |
| **Neon** ($Ne$) | 18 ppm | 27,1 | Teuer | Leuchtröhren (Rot), Excimer-Laser, Kältemittel |
| **Argon** ($Ar$) | **0,93 %** | 87,3 | Häufig & billig | **Schutzgas** (Schweißen MIG/WIG), Metallurgie, Inertisierung |
| **Krypton** ($Kr$) | 1 ppm | 119,8 | Schweres Gas | Isolierfenster (besser als Ar), Lampen |
| **Xenon** ($Xe$) | 0,08 ppm | 165 | Extrem teuer/rar | Auto-Scheinwerfer, **Anästhesie** (Narkose), Ionenantriebe (Raumfahrt) |

### Isolierglas-Füllungen
Um die Wärmeleitung im Scheibenzwischenraum (SZR) zu minimieren, werden schwere Edelgase statt Luft genutzt.
* **Grund**: Wärmeleitfähigkeit $\lambda$ sinkt mit steigender Molmasse (schwere Atome bewegen sich langsamer).
* **Vergleich**:
    * Luft: $\lambda \approx 26 \, mW/(m \cdot K)$
    * Argon: $\lambda \approx 17 \, mW/(m \cdot K)$ (Standard für 2-fach/3-fach Verglasung).
    * Krypton: $\lambda \approx 9 \, mW/(m \cdot K)$ (Für Passivhäuser/dünne SZR, aber teuer).

### Xenon-Narkose
Xenon ist ein "ideales Anästhetikum" (flutet schnell an und ab, nicht toxisch, kreislaufstabil).
* **Problem**: Der Preis (> 1000 €/m³).
* **Lösung**: Kreislaufsysteme ("Recycling" der Ausatemluft), um das teure Gas wiederzuverwenden.

# 13b Brenngase (LNG, CNG, LPG)
Unterscheidung der fossilen Gase nach Zusammensetzung und Speicherung.

### Aufgabe Kryotechnik
Vorkommen meißt an Orten wo es nicht gebraucht wird in den Mengen
Aufgabe der Kryotechnik Tranport und Lagerung durch Verflüssigung
Kälte oft irrelevant eher störend
### Begriffsdefinitionen
1.  **LPG (Liquefied Petroleum Gas)**: "Autogas" / Campinggas.
    * Gemisch aus **Propan** ($C_3H_8$) und **Butan** ($C_4H_{10}$).
    * Verflüssigung bereits bei geringem Druck (5...10 bar) bei Raumtemperatur möglich.
    * $\rho \approx 400 - 500 \, kg/m^3$.
2.  **CNG (Compressed Natural Gas)**: "Erdgas".
    * Hauptsächlich **Methan** ($CH_4$).
    * Gasförmig gespeichert bei hohem Druck (200...250 bar).
    * Geringe Energiedichte im Vergleich zu flüssigen Kraftstoffen.
3.  **LNG (Liquefied Natural Gas)**:
    * Tiefkalt verflüssigtes Erdgas ($CH_4$) bei ca. **111 K (-162 °C)**.
    * Drucklos (1 bar).
    * Hohe Dichte ($\approx 450 \, kg/m^3$) $\to$ Reichweite für LKW/Schiffe.
    * **Boil-Off**: Wie bei $LH_2$ entsteht Verdampfungsgas, das genutzt oder rückverflüssigt werden muss.

> [!NOTE] Methanzahl
> Analog zur Oktanzahl beim Benzin beschreibt die **Methanzahl** die Klopffestigkeit von Gasen. Reines Methan hat MZ 100 (sehr klopffest), Butan hat MZ 10 (klopffreudig). LNG schwankt je nach Herkunft (Libyen vs. Norwegen) stark in der Zusammensetzung ("Heavy" vs. "Light" LNG), was Motorenprobleme verursachen kann.


> [!INFO] Definition
> Erdgas ist ein fossiles Gemisch, das hauptsächlich aus **Methan ($CH_4$)** besteht. In der Kryotechnik ist vor allem die verflüssigte Form (**LNG** - Liquefied Natural Gas) relevant, da hier die Energiedichte für Transport und Speicherung maximiert wird.

## 1. Zusammensetzung und Qualitäten
Erdgas ist kein Reinstoff. Die Zusammensetzung schwankt je nach Lagerstätte stark.

* **Hauptbestandteil**: Methan ($CH_4$, $T_s = 111,6~K$).
* **Begleitstoffe**: Ethan, Propan, Butan, Stickstoff, $CO_2$, Schwefelwasserstoff ($H_2S$), Helium.
* **Reinigung**: Vor der Verflüssigung müssen $H_2O$, $CO_2$ und $H_2S$ ("Sauergas") entfernt werden, da diese ausfrieren und die Anlage verstopfen würden.

### LNG-Qualitäten ("Light" vs. "Heavy")
Da die Siedepunkte der Komponenten unterschiedlich sind, ändert sich das Verhalten:
* **Light LNG** (z.B. aus Norwegen/Nordsee): Fast reines Methan (> 98%). Hohe Methanzahl, brennt sauber.
* **Heavy LNG** (z.B. aus Libyen/Algerien): Enthält höhere Anteile an Ethan/Propan/Butan.
    * **Höherer Heizwert**, aber...
    * **Niedrigere Methanzahl** (klopffreudiger im Motor).
    * **Alterung (Aging)**: Während des Transports verdampft bevorzugt das leichte Methan (Boil-Off). Das verbleibende LNG wird "schwerer" und ändert seine Qualität.

> [!WARNING] Methanzahl (MZ)
> Maß für die **Klopffestigkeit** bei Gasmotoren (analog zur Oktanzahl).
> * Reines Methan: MZ = 100 (sehr klopffest).
> * Reines Butan: MZ = 10 (neigt zur Selbstzündung/Klopfen).
> * Problem: Motoren sind auf eine bestimmte MZ ausgelegt. Schwankende LNG-Qualitäten erfordern adaptive Motorsteuerungen ("Klopfregelung").

## 2. Speicherformen im Vergleich

| Eigenschaft | **CNG** (Compressed Natural Gas) | **LNG** (Liquefied Natural Gas) | **LPG** (Liquefied Petroleum Gas) |
| :--- | :--- | :--- | :--- |
| **Zustand** | Gasförmig, komprimiert | Tiefkalt verflüssigt | Flüssig unter Druck |
| **Druck / Temp.** | 200 ... 250 bar / Umgebung | 1 bar / ca. -162 °C (111 K) | 5 ... 10 bar / Umgebung |
| **Hauptkomponente** | Methan ($CH_4$) | Methan ($CH_4$) | Propan ($C_3H_8$) / Butan ($C_4H_{10}$) |
| **Dichte** | ~ 160 $kg/m^3$ | ~ 450 $kg/m^3$ | ~ 540 $kg/m^3$ |
| **Faktor Volumen** | 1 : 200 | **1 : 600** | 1 : 250 |
| **Anwendung** | PKW, Stadtbusse, Haushalt | LKW (Langstrecke), Schiffe | Camping, PKW ("Autogas") |

## 3. Die LNG-Prozesskette

### A. Verflüssigung (Liquefaction)
Da die kritische Temperatur von Methan (190 K) weit unter Umgebungstemperatur liegt, reicht bloßer Druck nicht aus (im Gegensatz zu LPG). Man braucht Kältemaschinen.
* **Energiebedarf**: Theoretisch min. $0,3~kWh/kg$, real ca. **$0,85~kWh/kg$**.
* **Verfahren**:
    1.  **Kaskaden-Prozesse**: Mehrere Kältekreisläufe hintereinander (z.B. Propan-Vorkühlung $\to$ Ethylen-Kreis $\to$ Methan-Kreis).
    2.  **Mixed Refrigerant (MR)**: Ein Kältemittelgemisch, das über einen weiten Temperaturbereich verdampft und so die Abkühlkurve des Erdgases ideal "anschmiegt" (minimale Exergieverluste). Standard in Großanlagen (C3MR-Prozess).

### B. Transport & Boil-Off
* Transport in riesigen Tankern ($> 200.000~m^3$).
* Trotz guter Isolierung (Perlit/Schaum) verdampfen ca. **0,1 ... 0,15 % pro Tag** (Boil-Off-Gas, BOG).
* **Nutzung BOG**:
    * Früher: Abfackeln (Verschwendung/Umwelt).
    * Heute: Nutzung als Treibstoff für den Schiffsmotor oder Rückverflüssigung (Reliquefaction) an Bord.

### C. Regasifizierung & "Cold Recovery"
Am Zielhafen (Import-Terminal) wird das LNG wieder verdampft und ins Gasnetz eingespeist.
* **Standard**: Verdampfung gegen Meerwasser ("Open Rack Vaporizer"). Die wertvolle Exergie der Kälte (bei 111 K) wird vernichtet (ins Meer gespült).
* **Cold Recovery (Kälterückgewinnung)**: Intelligente Nutzung der Kälte.
    1.  **Stromerzeugung**: Nutzung des LNG als "kalte Quelle" für einen ORC-Prozess (Organic Rankine Cycle) $\to$ Wirkungsgradverbesserung.
    2.  **Luftzerlegung**: Nutzung der Kälte,