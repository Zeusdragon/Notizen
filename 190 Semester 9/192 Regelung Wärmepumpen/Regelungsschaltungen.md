**Vorlesung**: [[Regelung von Wärmepumpen]]
**Datum**: 16.10.2025
**Topics**: #Lastabwurf/Lastverschiebung #Residuallast #Abtauung #Regelstrategien



>[!imporant] 🪧Hinweis 
>Hydraulik Kreis zuerst anmachen dann erst um Maschine kümmern
> 

# Residuallast
Stromverbrauch unabhängig von Sonne und wind

Last =  bedarf - Reg. Erzeugung

# Entscheidung Schalthandlung
![[Pasted image 20251016170706.png]]

# Regelungsvarianten
Schauen wenn komposition Verdichter expansionsventil
Verdichter sollte abgepasst zum Expansionsorgan sein da sonst imbalanz 
## Heißgasbypassschaltung
Magentventil für shutdown
Verdampfungsdruckhaltung ist Ziel
Heißgasleitung vor verdampfer und nach verdichter 
bypass and 4 zum einspritzen um Verdampfungsdruck zu halten falls verdampfungsdruck zu weit sinkt

### Vorteile
+ im gesamten Regelbereich nutzbar
+ kostengünstig
+ selbsttatig
### Nachteile
+ sehr ineffizient 
+ mehr heizgas notwendig
+ nicht nutzen !!!

## Verdampfungsdruckregler
VerdDruck halten bei keine Verdichterregelung
Absenkung des Drucks auf 1* und erhöhte technische Arbeit

### Vorteile
+ Kostengünstig
+ Verdichter keine adaption nötig
+ leicht installierbar
+ nutzbar mehere Verdampfer

### Nachteile
+ Regelbereich begrenzt
+ erhöhte Blastung des Verdichters durch erhöhten Hub 
+ niedirgiere Energieeffizienz da techn. Arbeit größer

## Taktbetrieb
Ziel; Reduzierung mittlerer Volumenstrom

Methode: zeitweise an und Ausschalten

### Vorteile 
+ einfach

### Nachteile 
+ diskontinuirlicher Betrieb
+ Belastung Verdichter erhöht
+ Energieeffzienz
+ Ungenaue Regelgüte

## Verdichterhubvolumen ändern
Ziel: Voluemenstrom reduzieren
Methode: Hubraum ändern man kann bänke abschaleten oder vorher auslass öffnen

### Vorteile
+ sehr großer Regelberich
+ Energieeffizient
### Nachteile 
+ Mechanisch komplex
+ wenn kolben bei hubkolben abgeschaltet potenzielle disbalanz  lebenszeit verkleinerrung und hörbar

## Drehzahlregelung
### Vorteile
TODO

## Qualitativer Vergleich
![[Pasted image 20251030152642.png]]

# Abtauung
Problem bei Luft Sekundär seite
Einfluss Luftfeuchtigkeit
Verschlechterung Wärmeübergang

## Möglichkeiten
+ Elektrisch Abtauuen
+ Umluftabtauung
+ Warmwasserabtauung
+ Heigasabtauung mit und ohne Kreislaufumkehr

## Heißgasabtauung
Dreickschaltung nach verdichter und vor verdampfer zwischen bypass und heißes gas in Verdampfer rein um Abtauung zu machen

## Kaltgasabtauung
nach Kondensator bei Sammler Gas output
vor Verdampfer bypass 
Problem weniger Überhitzungswärme dauert vielleicht länger als heißgas

## Kreislaufumkehr
4wege umschalt 

Achtung bidirektional ventile anschauen oder schaltung umlegen und zwei ventile pro litung setzen
![[Pasted image 20251030153813.png]]

sehr viel schneller als alles andere
ist Energetisch effizient
bei umschaltung kann es sein das bei umkehr die qärmequelle auf einmal wärmespeicher von pufferspeicher ist auslegung das kunde nichts merkt das es abgezwackt wird

