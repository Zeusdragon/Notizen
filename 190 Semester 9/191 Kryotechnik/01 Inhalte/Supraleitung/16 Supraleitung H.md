**Vorlesung**: [[191 Kryotechnik]]
**Datum**: 2026-01-20
**Topics**: [[Strombegrenzer]], [[Kabel]], [[Schwungrad]], [[Magnetlager]], [[Fusion]]

---

![[Supraleitung H_Anwendungen.pdf]]
# 16 Supraleitung H: Anwendungen in der Energietechnik

> [!INFO] Übersicht
> Neben Magneten (MRT, Forschung) gibt es zunehmend Anwendungen in der Energietechnik, getrieben durch den Bedarf an Effizienz und Netzstabilität bei steigender Einspeisung Erneuerbarer Energien.

## 1. Supraleitende Kabel
Ersatz für konventionelle Kupferkabel oder Hochspannungsleitungen.
* **Vorteil**: Extrem hohe Stromdichte, kaum Übertragungsverluste, keine thermische Erwärmung des Bodens (gut für Innenstädte).
* **Typ**: Meist HTS (Hochtemperatur-Supraleiter, $LN_2$-gekühlt).
* **Projekte**:
    * **AmpaCity (Essen)**: 1 km Kabel im Realbetrieb (ersetzt 110 kV Leitung durch 10 kV HTS-Kabel $\to$ Umspannwerk gespart).
    * **München**: Pläne für innerstädtische HTS-Trassen.

## 2. Supraleitende Strombegrenzer (FCL - Fault Current Limiter)
Eine Art "selbstrückstellende Sicherung" für Hochspannungsnetze.
* **Problem**: Im Kurzschlussfall steigen Ströme extrem an. Konventionelle Schalter müssen riesig dimensioniert sein.
* **Lösung**: Ein supraleitendes Element ist in Reihe geschaltet.
    * *Normalbetrieb*: $R=0$, kein Verlust.
    * *Kurzschluss*: Strom $I > I_c$. Der Supraleiter quencht (wird schlagartig normalleitend).
    * *Folge*: Der Widerstand springt auf einen hohen Wert, begrenzt den Kurzschlussstrom sofort.
* **Typen**:
    * **Resistiv**: Der Leiter selbst wird hochohmig.
    * **Induktiv**: Eisenkern wird durch sl Spule gesättigt (komplexer).

## 3. Rotierende Maschinen
* **Generatoren (Windkraft)**: HTS-Generatoren sind bei gleicher Leistung viel kleiner und leichter (ca. 50% Masse). Wichtig für Offshore-Anlagen (weniger Turmlast).
* **Motoren (Schiffsantrieb)**: Z.B. für U-Boote oder Eisbrecher (hohes Drehmoment bei kleiner Baugröße).

## 4. Magnetlager & Speicher
Nutzung der Flussverankerung (Pinning) in HTS-Bulk-Materialien.

### Magnetlager
* Ein HTS-Block wird (feldgekühlt) über einem Permanentmagneten positioniert.
* **Effekt**: Stabile Schwebe-Position ohne Regelungselektronik (Passiv stabil).
* **Anwendung**: Berührungsfreie Lager für Zentrifugen, Vakuumpumpen oder Transportbänder.

### Schwungradspeicher (Flywheel)
Speicherung von Strom als kinetische Energie ($E = 1/2 J \omega^2$).
* **Problem konventionell**: Lagerreibung und Selbstentladung.
* **Lösung**: Supraleitende Magnetlager reduzieren die Reibung fast auf Null.
* **Ziel**: Kurzzeitspeicher für Netzstabilität (Frequenzhaltung).

## 5. Kernfusion (Tokamak)
Ohne Supraleitung unmöglich.
* **ITER**: Der größte Tokamak der Welt (in Bau).
    * Benötigt riesige Magnetfelder zum Einschluss des Plasmas.
    * **Toroid-Feld**: 18 Spulen aus $Nb_3Sn$ (11,8 T).
    * **Poloid-Feld**: Spulen aus NbTi.
    * **Zentral-Solenoid**: $Nb_3Sn$ (13 T).
* **Kühlung**: CICC (Cable-in-Conduit), durchflossen von überkritischem Helium bei 4,5 K.