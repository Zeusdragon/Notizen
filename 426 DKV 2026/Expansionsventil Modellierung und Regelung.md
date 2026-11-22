
# Normale Modellierung
Blendengleichung per Bernoulli


# Was macht aachen 
Die Testen ein Expansionsventil und habe eine Faktor per Polynom Kalibriert


# Regelung von EEV

Grund Idee Wir gehen von Pi Regelung weg und gehen zu MPC 
und dann ein regler welcher Drehzahl und Öffnungsgrad beide Stellgrößen gibt und INputs sind beide Regelgrößen

Kopplung Gebäudemodell und WP Modell per Python und WEtterdaten zum Training

MPC ist wohl einiges Stabiler

## Fragen
Reifbildungs berücksichtigung und wurde da geregelt während abtauung wenn überhaupt abtaubetrieb abgebildet wurde

- Training über 24h Daten nur Heizbetrieb
- Simulationszeit ca. 2h je nach Komplexität des Modells
- MPC nur während heizen genutzt
- Robustheits test gemacht die Hält die Anlage aus