Idee Messdaten anschließend durch ein Modell jagen und dann Output anmolie ja nein und wenn ja was genau



# Vortrag Wiesbaden
haben ein Model trainiert mit LTSM zur Vorhersage von normalen Betrieb und Loss wird bewertet  insgesamter Datensatz ist 32,5 Monate wobei für die Tests nur Alarm drin waren training und Validierung waren nur bei Gesunden Daten


## Aufwand
Riesen Aufwand beim Labeling

LLM braucht das weniger und macht das echt gut aber längere Prozessdauer 15s bis 1 min

# Use Case: Alarm erkennung für Supermeakt Kälte

Prinzip SAubere Label Daten werden in ein bspw LTSM geladen Ansatz war 5min Daten über 8h sätze also 100 Zeilen 

modelle getesete:
- [[Isolation Forest]]
- [[Anomaly Transformer]]
- [[CATCH]]
- [[FEDFormer]]

AM Ende ein Prädiktor welcher werte Vorhersagt und wenn unterschied zwischen real und prädiktet zu hoch dann Alarm

Abtauung sind für den Prädiktor allerdings anomalien 