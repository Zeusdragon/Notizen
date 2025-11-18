# Service
1. Infos Abbonieren Sub beim Broker der Sendet daten per http post an service wenn fall eintritt beispiel temperatur zu hoch
2. Wenn Daten eintreffen und zustand eintrifft regler ausführen
3. Befehle senden HTTP-PATCH an API Orion Brocker um Daten Attribute zu ändern
4. Änderung an Gerät geben per MQTT Message

# Was sollte Entität sein
+ **Option A**:  *Entität*: Wärmespeicher hat 3 Devices
	+ Pufferspeicher:
		+ Sensor Temp ist ein device mit observed by
		+ Sensor Heat erzeugung ist ein device mit observed by
	+ Wärmepumpe
		+ Sensor Temp ist ein device mit observed by
		+ Sensor Heat erzeugung ist ein device mit observed by
	+ Pelletkessel
		+ Sensor Temp ist ein device mit observed by 
		+ Sensor Heat erzeugung ist ein device mit observed by


Prinizp Struktierung wenn ich was mache 
Vorgehen was mache ich und warum mache ich das was ich mache 
Erklären wieso randbedingungen gesetzt
danach zeigen was rauskommt und erklären was rauskommt
anschließend ergebnis was ist bewerten validiierne