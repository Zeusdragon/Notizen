# Schritt 1: Wärmepumpenmodell 1 Woche
[[Dymola-Modell]] für die Nutzung anpassen und Randbedingungen setzen
- Welche Verdampfer Geometrie nehme ich?
- Passt das modell zu dem Use Case?
- Wie mache ich es mit Wetterdaten und Wärmebedarf
- FMU Inputs: reverseCycle,
- FMU Outputs: COP, $T_{Luft}$, $T_{Verdampfer}$

# Schritt 2: Aus FMU eine Gym Umgebung machen 1-2 Wochen
Implementierung von fmugym um eine Interaktion des Modells mit RL-Agenten zu ermöglichen Wird eine Python Datei und es muss Drop in möglich sein FMU rein zu laden

# Schritt 3: RL Training 1-2 Wochen
- Algorithmus aussuchen aktuell [[QR-DQN (Quantile Regression DQN)]] am sinnvollsten 
- Trainingzeitfestlegen wie viele Zeitschritte und wie viele Episoden
- Abbruchkriterium wann wird episode truncated 
- wie will ich Exploration und Explotation machen damit das modell ein wenig austesten kann und dann am ende fein ist. Nochmal [[Klingebiel - Bedarfsgesteuerte Abtauung von Luft-Wärmepumpen durch Reinforcement Learning.pdf|Paper von Klingebiel]] angucken
- CPU Parallelisierung um TrainingsZeit zu verkürzen
- am Ende model speichrn wird eine zip Datei die ich mit model.load funktion laden kann und dann nutzen kann um für ein bis zwei Episoden zu testen

# Schritt 4: RL Austesten 1-2 Tage
- auf Referenzmodell gespeichertes RL Modell für zwei Episoden spielen lassen 
- Auswerten mit Vergelich für Bedarfsgesteurte und Zeitgesteurte Abtauung
- Was soll meine Parameter sein die ich mir angucke potenziell elektrische Leistung, COP, Heizleistung, Verdampfertemperaturen
- Mein Seed für Temperatur und Wärmebedarfprofil muss gleich sein für die referenz simulationen

# Schritt 5: Transfer andere FMU 2-3 Wochen
- wie viele FMU's will ich machen wie diskret ist die Auflösung 
- Welcher Parameter genau soll angeguckt werden 
	- Gibt es Literatur oder eigene Simulation machen und gucken wo die größte Varianz mit der Abtaudynamik im Punkte $P_{el}$ und COP
	- Erste Tests 
- Min und Max wert festlegen [[Design of Experiment]]
- gespeichertes Modell auf andere FMU setzen und wieder simulieren 



