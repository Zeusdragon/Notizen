# Dymola Modell folgendes anzeigen
Kreislauf auswirkung eines gefrosteten Verdampfer 


Outputs relevant: 
Belohnung:  $\frac{COP}{COP_{Carnot}}$ 
**Observation**: $T_{Luft}$ , $T_{Verdampfer}$ 

Inputs:
+ Störgrößen
	+ Wetter: phi, T
+ Stellgrößen (Aktuator):
	+ Boolscher: Abtauung an/aus (reverseCycle)
	+ Compressor Hubvolumen mit limit zwischen 0.1 und 1
	+ EXV Öffnungsgrad zwischen 0.1 und 1 wird dann umgerechnet mit kator 1.5e-3
	+ 