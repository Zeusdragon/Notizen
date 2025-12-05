**Vorlesung**: [[193 Sensorik]]
**Datum**: 05.12.2025
**Topics**: [[Durchflussmessung]]

![[03_VL_PMTS_DFM4.pdf]]

# Grundprinzip
Voraussetzungen:
+ Stoff muss freie ladungsträger besiitzen
+ Es kann also kein Gas gemessen werden

Es wird ein Magentfeld über dem Rohrquerschnitt aufgebaut wenn Fluid mit freine Ladungsträgern nun dadurch stömt wird eine Spannung induziert die man messen kann . Es baut sich eine Lorentzkraft aus nach Rechter bzw Linker Handregel je nach Ladung der Ladungsträger. 

Lorentzkraft: $\vec{F_L} = q \cdot [\vec{v} \times \vec{B}]$  

Coloumbkraft: $\vec{F_E} = q \cdot \vec{E}$
$\Phi$ : Elektrisches Potential (Spannung)

$$\vec{F_L} = \vec{F_E}$$
$$\vec{E} = \vec{v} \times \vec{B}$$
$$div \space \vec{E} = div(\vec{v} \times \vec{B})$$
$$div \space \nabla \Phi = div(\vec{v} \times \vec{B})$$
>[!Endformel]
>Lösung für Spannung ergibt $U_M = D \cdot B_y \cdot \overline{v}$  für homogene ideale Sensoren und $U_M = k \cdot D \cdot B_y \cdot \overline{v}$ für reale Sensoren mit leicht inhomogenen Magnetfeld


Nicht jedes Volumenelement erzeugt gleichen BEitrag für Spannung. Höheres Gewicht bei VE in der Nähe der Elektroden je weiter Weg desto weniger Gewicht

Um das zu berücksichtigen ist eine Symmetrische Strömung wichtig sonst hohe Messsunsicherheit

# Signalbildung

Um vernünftig auflösen zu können ist im man bei der Auflösung im $\mu V$ Bereich schwer zu messen. Abschirmung deswegen wichtig um Störung zu minimieren. Schaltung werden innerhalb des Sensors so gut wie möglich analog gehalten.

Aus Messkreis haben noch zwei Störspannung einen Eintrag.
Störpannung durch Spulen induktion die ist 90° phasenverschoben. Dann noch ein Netzbrummen im mV Bereich die ist nicht klar mit Phasenlage durch kapazitve und galvanische Einkopplung.

>[!Merke]
>50Hz Wechselfeld Messung deswegen nicht ideal

Andere Möglichkeiten 25 Hz Wechselfeld machen
Was standard ist Gleichfeld was pulsiert mit 25 Hz an aus dadurch keine Induktion von Spule. Allerdings muss Netzbrummen noch gefixt werden.

Möglichkeiten:
+ pulsierendes Gleichfeld mit umschaltbetrieb dadurch kann man einfach 2 $U_M$ messen 
+ phasensynchrone Differenzbildung bei An und Ausbetrieb
+ Integration über Netzspannungsperiode

# Elektrodenaufbau
+ Kapazitativ (kein direkter Kontakt Kondensator in Isolationsmaterial aufgebaut)
+ Galvanisch direkter Kontakt mit Fluid

# Fazit
+ Sensor nicht so genau
+ nur für Flüssigkeiten nutzbar
+ einfach aufgebaut
+ wird gerne mit aggresiven Korrosiven Medien genutzt
+ oft in Chemie Industrie genutzt
