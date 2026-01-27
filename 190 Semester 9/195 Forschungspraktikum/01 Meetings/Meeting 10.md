
# Ergebnisse
+ Encodapy nicht trivial umschreibbar für Linked Data
	+ Dependicy Problem mit fillip Lib die für v2 gut ist aber nicht sicher wie ausgereifft für ld
+ Aktuell eigener Service noch nicht sicher wie ich die fiware daten abfragen einfach durch requests sowie cratedb daten muss ich auch abfragen
+ Node-Red funktioniert ganz gut nur klappt das noch nicht ganz mit Mapping sodass ich ein fertiges Entity habe
+ allgemein für LD ein großes Integrations Problem in bestehende Strukturen da dependicies oft noch nicht soweit dafür (Struktur von Abfragen sachen wie Metadata etc.)
+ Mockup Service mit API Requests und kleiner Linearen Regression für Außentemperatur zukunft zu potenzieller Wärmeverbrauch mapping und anschließend SOC iterieren.
	+ Allerdings Regression noch Test noch nicht trainiert durch historische Daten
	+ allgemein unsicher wie das System aktuell Betrieben wird und man wirklich ein Verhalten daraus lesen kann?
	+ Aktuell noch keine Gedanken gemacht wie genau die SOC abgezogen wird durch projezierten Verbrauch von Verbrauchern da TWE $\neq$ HK in SOC wegen Temperaturschichtung etc.   

# Fragen
+ wenn regler da ich kann ja nicht publishen wie soll ich daten generieren die dann das verhalten abbilden?

+ was ist maximaler Verbrauch vom Heizkris und Trinkwasser

+ Wie viel Wärmeenergie kann pufferspeicher halten?

+ Geolocation für Wärmepumpe für Wetterdaten einführen.
	+ Vorlauf wird doch wichtig wegen COP berechnung für Wetterdaten mit zugehörigen Vorlauf sowie Verdichter Verbrauch etc.

+ Idee eine Loop die jede Stunde einmal zukunft abfragt und dann einmal einer der alle 15 minuten aktuellen Status nur abfragt und guckt ob wir überlaufen oder kritisch unterwegs sind als sicherheit.

+ Sharky_TW hat werte die eher für HZ sprechen und vice versa?
	+ Trinkwasser irrelevant

+ zugang zu Intranet doch weg?

+ Funktion aktueller Regler nachbauen?

+ Punkte Graphdatenbanken mit context eingehen welche Punkte wirklich genutzt werden

+ Welche hindernisse aktuelle entwicklung hat 

+ Daten Mapping Aufwand einbringen 

+ An Maximilian SChema schicken mit Datenpunkten die ich brauche

+ Context Server angucken mit Graph Tree  



