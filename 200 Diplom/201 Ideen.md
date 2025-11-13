# Gedankensammlung für Diplom Arbeit

Dymola modell soll Abtaudaten liefern bei bestimmten Bedingungen sowie blackbox wo randbed reingegeben werden und ich Zustandspunkte bekomme
INputs sollten sein wie befehle Abtauung starten oder irgend ein steurungsbbefehl vielleicht externe sollwerte

Motivationsidee echte Vereisungssensoren schlecht störanfällig etc es mit soft sensor Ki versuchen ob besser und worauf zu achten

Modell kriegt wetter infos also umgebungstemperatur und Luftfeuchte

Implementierung mit eine Gymnasium Umgebung mit der FMU aus Dymola Modell

FMU GYM Frauenhofer


GAb es schon alles Jonas Klingebiel DIssertation 
Fokus REinforcement Training mit anderen Belohnugsmöglichkeiten und anderes modell performt vielleicht mit pufferspeicher.

# Arbeitsschritte
## 1 Fundament 
+ Dymola Modell muss Abtaudynamik abgebildet haben
+ FMU (Functional Mock UP) definieren:
	+ Inputs: Abtau befehl, Sollwerte
	+ Outputs: Zustandspunkte Eismasse COp und Leistungen
		+ Reward aus Output mit Wert aus Eismasse COP und Heizleistung
	+ FMU Export
		+ Erste Einrichtung des Gym Umgebung FMU laden und in 10 Schritten erstmal werte anzeigen lassen die aus dem modell kommen

## 2 Implementierung
Python Umgebung definieren 
Reward definieren also formel setzen was mehr belohnt werden sollte und was besser klappt

## 3 Validierung
Vergleich wie mien Modell abschneidet im Vergleich zu Jonas

## 4 Test im echten Leben an Prüfstand (optional)
Hardware in the loop modell gibt an Regler system Sollwerte und Bool Abtauung
UNd dann Messen wie performt in Vergleich zu Standard regler