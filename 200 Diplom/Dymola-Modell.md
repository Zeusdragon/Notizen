# Dymola Modell folgendes anzeigen
Kreislauf auswirkung eines gefrosteten Verdampfer 

![[Dymola_model.png]]

Outputs relevant: 
Belohnung:  $\frac{COP}{COP_{Carnot}}$ 
**Observation**: $T_{Luft}$ , $T_{Verdampfer}$ , $\varphi_{Luft}$ 

Inputs:
+ Störgrößen
	+ Wetter: phi, T, Heizlast Q kann eingegeben werden
+ Stellgrößen (Aktuator):
	+ Boolscher: Abtauung an/aus (reverseCycle)
	+ Compressor Hubvolumen mit limit zwischen 0.1 und 1
	+ EXV Öffnungsgrad zwischen 0.1 und 1 wird dann umgerechnet mit kator 1.5e-3
	+ Compressor und EXV nun nicht mehr durch FMU Steuerbar werden innerhalb fmu selbst durch PI Regler und konstanten gestellt. 

# Kompressor
- Scrollverdichter üblich in Wärmepumpen
- Displacement = 114e-6 m³ Referenz "Bitzer GSP60120ZL Scroll"
- Pi regelung über Vorlauftemperatur am Kondensator 
	- Heizkurve bei 35-20°C für Fußbodenheizung
	- 20-100 Hz Frequnz Modulation

# Pumpe
- Fördervolumen von Pumpe
- 