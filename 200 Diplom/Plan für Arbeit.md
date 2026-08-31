---
title: "Plan für Arbeit"
type: diplom
erstellt: 2026-05-05
---

# Schritt 1: Wärmepumpenmodell 1 Woche ✔
[[Dymola-Modell]] für die Nutzung anpassen und Randbedingungen setzen
- [x] Welche Verdampfer Geometrie nehme ich?
	- Aktuell Geometrie mit Standardwerten aus TIL und Dimensionierungsanpassungen von Makroparametern der Verdampfergeometrie um die Leistugnsklasse von 10kW zu erreichen
- [x] Passt das modell zu dem Use Case?
- [x] Wie mache ich es mit Wetterdaten und Wärmebedarf
- [x] FMU Inputs: reverseCycle,
- [x] FMU Outputs: COP, $T_{Luft}$, $T_{Verdampfer}$
	- Modell kann aktuell zur untersuchung fassst alles als output geben für RL wird aber nur $T_{Luft}$, $T_{Verdampfer}$ und letzte Aktion gezeigt
- [x] Potenziell doch Komplexer als erwartet maybe muss Verdampfer Modell angepasst werden das wär semi nice
	- Muss nicht angepasst werden
- [x] mal gucken ob die Abtau abbildung von dem WP Modell ausreicht

# Schritt 2: Aus FMU eine Gym Umgebung machen 1-2 Wochen ✔
Implementierung von fmugym um eine Interaktion des Modells mit RL-Agenten zu ermöglichen Wird eine Python Datei und es muss Drop in möglich sein FMU rein zu laden

# Schritt 3: RL Training 1-2 Wochen
- [ ] Algorithmus aussuchen aktuell [[QR-DQN (Quantile Regression DQN)]] am sinnvollsten 
	- Wird auch genutzt Test zwischen QRDQN und DQN fehlt noch
- [x] Trainingzeitfestlegen wie viele Zeitschritte und wie viele Episoden
	- Aktuell 150000 Schritte
- [x] Abbruchkriterium wann wird episode truncated
	- maximal wenn ein til error entsteht ansonsten kein truncated
- [ ] wie will ich Exploration und Explotation machen damit das modell ein wenig austesten kann und dann am ende fein ist. Nochmal [[Klingebiel - Bedarfsgesteuerte Abtauung von Luft-Wärmepumpen durch Reinforcement Learning.pdf|Paper von Klingebiel]] angucken
	- Aktuell Hyperparameter auf basis von Klingebiel 
- [x] CPU Parallelisierung um TrainingsZeit zu verkürzen
	-  Aktuell 16 Umgebungen gleichzeitig am laufen trainingszeit ca. 5-6 Stunden
- [x] am Ende model speichrn wird eine zip Datei die ich mit model.load funktion laden kann und dann nutzen kann um für ein bis zwei Episoden zu testen

# Schritt 4: RL Austesten 1-2 Tage
- [x] auf Referenzmodell gespeichertes RL Modell für zwei Episoden spielen lassen 
- [x] Auswerten mit Vergeleich für Bedarfsgesteurte und Zeitgesteurte Abtauung
- [x] Was soll meine Parameter sein die ich mir angucke potenziell elektrische Leistung, COP, Heizleistung, Verdampfertemperaturen
	-  Zyklusdauer, Abtauprozesse pro tag, SCOP alles als Median und Mittelwert 
- [x] Mein Seed für Temperatur und Wärmebedarfprofil muss gleich sein für die referenz simulationen
- [x] Vergleich durch SCOP = $\frac{\dot{Q}_c - \dot{Q}_{abtau}}{P_{el}}$
	- Test kann auch grafiken erstellen von Temperaturverläufen, Heizleistung, COP und Eismasse

# Schritt 5: Transfer andere FMU 2-3 Wochen
- [ ] wie viele FMU's will ich machen wie diskret ist die Auflösung 
- [ ] Welcher Parameter genau soll angeguckt werden 
	- Gibt es Literatur oder eigene Simulation machen und gucken wo die größte Varianz mit der Abtaudynamik im Punkte $P_{el}$ und COP
	- Erste Tests 
- [ ] Min und Max wert festlegen [[Design of Experiment]]
- [ ] gespeichertes Modell auf andere FMU setzen und wieder simulieren 



