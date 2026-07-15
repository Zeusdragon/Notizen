- FMU performanter als Dymola selber 
- 150000 steps ein step simuliert ca 100 sekunden und dann interagiert die RL wieder mit der FMU
- 150000 Steps sind ca. 170 Simulierte Trainingstage
	- Konvergiert potenziell auch schon nach 50.000-70.000 schritten
	- bei Klingebiel hatte sich nach 40.000 schritten Konvergiert
	- Indikator ist wenn die Belohnung anfängt zu stagnieren und ein Plateau bildet.
	- Belohnung hier über $\frac{COP}{COP_{Carnot}}$
- An Wetterprofilen muss man trainieren dafür gibt es es eine Dymola lib namens AIXLIB um Wetter daten zu machen und Gebäudeprofile zu simulieren. 
	- **Wetterprofil suchen** was ist ein geeigneter Tag 
		- rene schickt test jahre
	- Potenziell nicht mal aixlib nötig ich fütter einfach in python mit den Zeitreihen von Temperaturen und Luftfeuchtigkeit meine Inputs für das WP Modell sind Tair, phiair, TLiqInlet, VFlowLiq
- Algo potenziell [[QR-DQN (Quantile Regression DQN)]] nutzen als erweiterung zu [[DQN (Deep Q-Network)]]
- Hyperparameter optimierung Optuna nutzen vgl. [[Klingebiel - Bedarfsgesteuerte Abtauung von Luft-Wärmepumpen durch Reinforcement Learning.pdf|Klingebiel]] 
- Eine Episode sollte ein Tag sein
- und dann Simulationstest muss außerhalb der Testbatch sein
- wie kann ich perfekten Abtauzeitpunkt ermitteln? 
	- eher rausnehmen zu viel zeitaufwand das zu bestimmen
	- muss experimental und simulativ in einer parameterstudie gemacht werden für verschiedene Lasten
- brauche einen Trainings und validierungs datensatz
- Action Space nur abtauen ja/nein (Bool)
- State Space wird nur Tair und TVerdampfung sein sowie wie a-1

# Erkentnisse
## Hyperparameter nicht gleich bei änderungen der timesteps
wenn ich hyperparameter habe wo gesagt wird ab wann Lernen anfängt und training äufhört macht das ein unterschied weil die zahl relativ ist also man sagt nach der hälfte hört er auf zufall zu machen wenn ich nun.

Wichtig Trainings_frequnz und Gradienten Update an Prozess Anzahl koppeln damit. genug Updates des Netzwerkes stattfinden.