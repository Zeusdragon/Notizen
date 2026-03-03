# Fragen
+ Wo ist dann die Fiware Plattform 
+ Encodapy wo
+ Würde erstmal das encodapy bei mir testen und dann rüberholen
+ Was genau dann testen
+ Strompeis woher?
+ erzeuger status welches topic bzw wohin der befehl?

# Ergebnisse
+ Ergebniss
+ Strompreis als Variable Konstante
+ Command MQTT 
+ Daten die ich brauche 
	+ Aktuell Außentemperatur
	+ Ist Temperatur TWE/HK
	+ Status als Commands 
	+ wir lassen modes erstmal raus


# Plan 
+ Mapping der MQTT bridge
+ Setup n5geh.plattform stack
+ Mosquitto config
+ Entität 
	+ Strompreis Als extern hier als Konstante Variable
	+ Weather (Temperatur Mqtt)
	+ Wärmepumpe (Status)
	+ Pufferspeicher (IsttemperaturHeiz, IstTemperaturTWE Mqtt)
	+ Pelletkessel (Status, Ignitions, Runtime)