# Gutachten und strategische Handlungsempfehlung: Wissenschaftliche Ausarbeitung und technologische Implementierung im Kontext des N5GEH

## 1. Einleitung und wissenschaftliche Einordnung

Die vorliegende Analyse dient als umfassendes Gutachten und detaillierter Leitfaden zur Finalisierung der wissenschaftlichen Arbeit mit dem Arbeitstitel „Funktionsvergleich der FIWARE-Plattform für verschiedene Datenmodelle am Beispiel eines Anwendungsfalls in einem hybriden Wärmeversorgungssystem“. Der Entwurf (`IPA_N5GEH.DOCX`) sowie die zugehörige Aufgabenstellung (`Aufgabenstellung_PA_Feizi.pdf`) wurden einer tiefgehenden Prüfung unterzogen. Der Kontext der Arbeit ist im Rahmen des **National 5G Energy Hub (N5GEH)** angesiedelt, einem Forschungsprojekt, das die Digitalisierung der Energiewende durch den Einsatz moderner Kommunikationsinfrastrukturen (5G) und cloudbasierter Plattformtechnologien vorantreibt.1

Die Transformation des Energiesektors hin zu einem dezentralen, fluktuierenden System erfordert eine intelligente Vernetzung von Erzeugern, Speichern und Verbrauchern. Hybride Heizsysteme, die beispielsweise die Grundlast über eine Wärmepumpe (Power-to-Heat) und die Spitzenlast über einen Biomassekessel abdecken, stellen hierbei ideale Flexibilitätsoptionen dar. Die Herausforderung – und damit der Kern dieser wissenschaftlichen Arbeit – liegt nicht in der Hardware, sondern in der informationstechnischen Abbildung und Orchestrierung dieser Komponenten. Die Wahl des Datenmodells innerhalb der genutzten Middleware (FIWARE) ist dabei keine triviale Syntax-Entscheidung, sondern bestimmt maßgeblich die Interoperabilität, Skalierbarkeit und Zukunftsfähigkeit des Gesamtsystems.3

Der aktuelle Entwurf der Arbeit zeigt ein solides Grundverständnis der Thematik, weist jedoch in den Bereichen der konkreten technologischen Implementierung, der semantischen Tiefe des Datenmodellvergleichs (NGSI-v2 vs. NGSI-LD) und der algorithmischen Umsetzung der Regelungsstrategie signifikante Lücken auf. Ziel dieses Dokuments ist es, diese Lücken durch fundiertes Fachwissen zu schließen, die theoretischen Grundlagen massiv zu erweitern und einen präzisen Fahrplan für die verbleibenden drei Monate zu liefern.

---

## 2. Systemanalyse: Thermodynamik und Hydraulik des hybriden Clusters

Ein valides Datenmodell kann nur entwickelt werden, wenn die physikalischen Prozesse des zu modellierenden Systems vollständig durchdrungen sind. Der Entwurf beschreibt die Komponenten bisher eher phänomenologisch. Für eine ingenieurwissenschaftliche Arbeit ist eine tiefergehende Analyse der Betriebscharakteristika zwingend erforderlich, da diese die Anforderungen an die Attribute und Metadaten der FIWARE-Entitäten diktieren.

### 2.1 Der Pelletkessel: Trägheit und Emissionsverhalten

Der im System verbaute Pelletkessel (referenziert als Solarvent-System, z.B. iQ 3.0 Serie) fungiert als Hochtemperatur-Erzeuger.1 Im Gegensatz zu Gas- oder Ölheizungen, die nahezu instantan Wärme liefern und beliebig takten können, unterliegen Biomassekessel komplexen thermodynamischen Restriktionen.

Der Verbrennungsprozess von Holzpellets gliedert sich in Phasen: Zündung, Flammstabilisierung, Volllastbrand und Ausbrand. Eine zu kurze Laufzeit (Taktung) führt dazu, dass der Kessel die optimale Verbrennungstemperatur nicht oder nur kurz erreicht. In den Anfahr- und Abfahrphasen sind die Emissionen von Kohlenmonoxid (CO) und Feinstaub sowie unverbrannten Kohlenwasserstoffen (OGC) signifikant erhöht.4 Zudem führt häufiges Zünden (via elektrischem Glühzünder) zu erhöhtem Stromverbrauch und Verschleiß am Zündelement sowie thermischem Stress im Kesselstahl.

Implikation für das Datenmodell und die Regelung:

Die im Entwurf genannte Mindestlaufzeit von 40 Minuten ist kein willkürlicher Wert, sondern ein harter physikalischer Constraint zur Sicherstellung der Anlageneffizienz und Langlebigkeit.1 Das Datenmodell muss daher zwingend Zustandsattribute abbilden, die über ein einfaches On/Off hinausgehen. Es werden Attribute wie operationState (z.B. "Ignition", "Stabilization", "Modulation", "Burnout") und Zähler für startsToday benötigt, um im Regelalgorithmus ("Service") eine Hysterese zu implementieren, die ein Abschalten während der Stabilisierungsphase aktiv verhindert.6

### 2.2 Die Wärmepumpe: Exergie und COP-Variabilität

Die Wärmepumpe (WP) nutzt elektrische Energie, um Umweltwärme (Luft, Wasser, Erde) auf ein höheres Temperaturniveau zu heben. Ihre Effizienz wird durch den Coefficient of Performance (COP) beschrieben, der maßgeblich von der Temperaturspreizung (Hub) zwischen Wärmequelle (Außentemperatur) und Wärmesenke (Vorlauftemperatur) abhängt.8

In einem hybriden System ist die WP der bevorzugte Erzeuger für niedrige Temperaturniveaus. Sinkt die Außentemperatur jedoch unter den sogenannten Bivalenzpunkt (z.B. -5°C), sinkt der COP drastisch, und der elektrische Energiebedarf steigt überproportional an. Ab diesem Punkt wird der Betrieb ökonomisch und ökologisch ineffizient im Vergleich zum Pelletkessel.9

Implikation für das Datenmodell:

Das Datenmodell der Wärmepumpe darf nicht statisch sein. Es muss dynamische Eingangsgrößen verarbeiten. Essenziell sind Attribute für returnTemperature (Rücklauf) und supplyTemperature (Vorlauf), da diese die momentane Effizienz bestimmen. Für die ökonomische Optimierung (Teilaufgabe 1b) muss der Algorithmus den aktuellen COP schätzen können. Dies erfordert entweder eine hinterlegte Kennlinie (als statisches Attribut-Array in der Entity) oder den Zugriff auf externe Wetterdaten (als separate Entity WeatherObserved), um die Quelltemperatur zu kennen.

### 2.3 Der Pufferspeicher: Hydraulische Entkopplung und Schichtung

Der Pufferspeicher dient als hydraulische Weiche und Energiespeicher. Er entkoppelt die zeitliche Diskrepanz zwischen Wärmeerzeugung (z.B. erzwungener 40-Minuten-Lauf des Pelletkessels) und Wärmeverbrauch (Heizlast des Gebäudes).

Die Effizienz des Speichers hängt von seiner Schichtung ab. Heißes Wasser (geringere Dichte) sammelt sich oben, kaltes unten. Eine Durchmischung (Entropieerzeugung) muss vermieden werden. Der Entwurf erwähnt Anschlüsse "11a" bis "11c".1 Dies deutet auf eine gezielte Einschichtung hin.

Implikation für das Datenmodell:

Ein einzelner Temperaturwert bufferTemp ist nutzlos. Das FIWARE-Modell muss die vertikale Temperaturverteilung abbilden. Dies kann durch multiple Attribute (tempTop, tempMid, tempBottom) oder durch eine 1:n-Beziehung zu mehreren Sensor-Entitäten gelöst werden. Der "State of Charge" (SoC) ist keine direkte Messgröße, sondern eine abgeleitete Größe (virtueller Sensor), die aus der Temperaturmatrix berechnet werden muss. Dies ist ein perfektes Anwendungsbeispiel für das Context Processing in der Cloud: Rohdaten kommen an, der SoC wird berechnet und als neues Attribut in die Speicher-Entity zurückgeschrieben.11

---

## 3. Die N5GEH-Architektur: Vom Sensor zur Cloud

Um die geforderte "Analyse der IT-Infrastruktur" (Teilaufgabe 2) auf Expertenniveau zu leisten, muss die Architektur präzise beschrieben werden. Das N5GEH-Projekt nutzt eine Referenzarchitektur basierend auf FIWARE Generic Enablers, die speziell für Energiedaten optimiert wurde.2

### 3.1 Die Northbound-Southbound-Schnittstelle

Die Architektur unterscheidet strikt zwischen der Feldebene (Southbound) und der Anwendungsebene (Northbound).

- **Southbound (Feldebene):** Hier agieren die physischen Geräte. Im vorliegenden Fall kommuniziert der WAGO PFC Controller (z.B. Serie 750-8212) via MQTT. Die Sensoren (z.B. Diehl Sharky 775 Wärmemengenzähler) sind oft via M-Bus an die WAGO angeschlossen. Die WAGO-SPS fungiert als Gateway und übersetzt die lokalen Feldbussignale in MQTT-Nachrichten (JSON-Payloads).15
    
- **IoT Agent:** Dies ist die kritische Komponente für die Datenaufnahme in FIWARE. Da der Context Broker (Orion) nur NGSI (HTTP) spricht, muss der IoT Agent (z.B. `iot-agent-json` oder `iot-agent-ultralight`) die MQTT-Nachrichten empfangen, parsen und in NGSI-Update-Requests konvertieren. Hier passiert das Mapping: Ein MQTT-Topic wie `/wago/plant1/temp_boiler` wird auf das Attribut `temperature` der Entity `Device:Boiler001` abgebildet.17
    

### 3.2 Context Management und Persistenz

- **Orion Context Broker:** Er hält den _aktuellen_ Zustand des Systems (Digital Twin). Er weiß, dass der Kessel _jetzt_ 60°C hat. Er weiß nicht, was vor 5 Minuten war. Orion ist transient.
    
- **QuantumLeap & CrateDB:** Da energetische Analysen (wie die geforderte Taktungsanalyse) historische Daten benötigen, ist diese Komponente unverzichtbar. Orion sendet bei jeder Änderung eine Benachrichtigung (Notification) an QuantumLeap. QuantumLeap transformiert die NGSI-Daten in ein Zeitreihenformat und speichert sie in CrateDB (einer skalierbaren SQL-Datenbank). Ohne diesen Pfad ist Teilaufgabe 3b (Vergleich der Anfahrvorgänge) technisch nicht lösbar.13
    

---

## 4. Tiefenvergleich: NGSI-v2 vs. NGSI-LD

Dies ist das theoretische Kernstück der Arbeit. Der Vergleich darf sich nicht auf Syntax beschränken, sondern muss die systemtheoretischen Implikationen beleuchten.

### 4.1 NGSI-v2: Das pragmatische Silo

NGSI-v2 (Next Generation Service Interface version 2) ist ein REST-basiertes API, das Daten als JSON-Objekte repräsentiert. Es ist einfach zu implementieren und hat geringen Overhead.

Struktur:

Das Modell basiert auf dem Tripel: Entity ID - Attribute - Value.

Ein Attribut kann Metadaten haben.

**Beispiel (NGSI-v2 Payload):**

JSON

```
{
  "id": "Boiler:001",
  "type": "PelletBoiler",
  "temperature": {
    "value": 75.5,
    "type": "Number",
    "metadata": {
      "unit": { "value": "Celsius", "type": "Text" }
    }
  }
}
```

Kritik:

Die Semantik ist implizit. Ein externer Entwickler oder ein Algorithmus weiß nicht, was "Boiler" oder "temperature" bedeutet. Ist es die Abgastemperatur? Die Wassertemperatur? Die Einheit "Celsius" ist nur ein String. Um dieses System zu integrieren, benötigt man eine externe Dokumentation (PDF, Wiki). Dies führt zu Datensilos, da Systeme nicht ohne manuellen Aufwand interagieren können.20 Beziehungen zwischen Entitäten werden oft über Attribute wie refPuffer simuliert, die für die Maschine aber nur bedeutungslose Textstrings sind.21

### 4.2 NGSI-LD: Das semantische Netz (Linked Data)

NGSI-LD (Linked Data) ist die Evolution hin zum Semantic Web, standardisiert durch ETSI ISG CIM. Es nutzt JSON-LD.

Struktur: Der Property Graph

NGSI-LD modelliert die Welt als Graphen. Entitäten sind Knoten, Beziehungen (Relationships) sind Kanten. Ein entscheidender Unterschied zu v2 ist, dass Beziehungen echte Objekte sind, die selbst Eigenschaften haben können ("Properties of Relationships").

Beispiel: Eine Beziehung connectedTo vom Kessel zum Puffer kann das Attribut flowRate (Durchfluss) besitzen. Dies ermöglicht eine topologisch korrekte Abbildung des Hydraulikschemas als Graph.21

Semantik via @context:

Das mächtigste Feature ist der @context. Er verlinkt die lokalen Begriffe auf globale Ontologien (Wörterbücher).

**Beispiel (NGSI-LD Payload):**

JSON

```
{
  "id": "urn:ngsi-ld:Boiler:001",
  "type": "Boiler",
  "temperature": {
    "type": "Property",
    "value": 75.5,
    "unitCode": "CEL",
    "observedAt": "2023-10-27T10:00:00Z"
  },
  "@context": [
    "https://smart-data-models.github.io/dataModel.Energy/context.jsonld",
    "https://uri.etsi.org/ngsi-ld/v1/ngsi-ld-core-context.jsonld"
  ]
}
```

Durch den `@context` wird definiert, dass `Boiler` z.B. der Definition in der SAREF-Ontologie (Smart Appliances REFerence ontology) entspricht. Eine KI oder ein Aggregator, der SAREF versteht, kann diesen Kessel steuern, ohne jemals zuvor mit diesem spezifischen Hersteller zu tun gehabt zu haben. Die Einheit "CEL" ist ein standardisierter UN/CEFACT-Code, den jede Maschine parsen kann.3

### 4.3 Quantitative und Qualitative Gegenüberstellung

Um den wissenschaftlichen Anspruch zu erfüllen, sollte der Vergleich in der Arbeit anhand definierter Kriterien erfolgen.

|**Kriterium**|**NGSI-v2**|**NGSI-LD**|**Relevanz für N5GEH/Wärmeversorgung**|
|---|---|---|---|
|**Interoperabilität**|Gering (proprietäre Modelle)|Hoch (durch Ontologien/Standardmodelle)|Kritisch für Sektorkopplung (Strommarkt $\leftrightarrow$ Gebäude).|
|**Komplexität**|Niedrig (Flat JSON)|Hoch (Verschachtelung, URIs, Context)|LD erfordert steilere Lernkurve für Entwickler.|
|**Payload-Größe**|Kompakt|Verbose (lange URIs)|LD erzeugt mehr Traffic; relevant bei NB-IoT/LoRaWAN.|
|**Beziehungen**|Simuliert (`ref...`)|Nativ (`Relationship` als Graph-Kante)|LD bildet hydraulische Netze besser ab.|
|**Reifikation**|Über Metadaten (limitiert)|Nativ (Properties of Properties)|LD erlaubt z.B. Zeitstempel pro Attribut (`observedAt`).|

---

## 5. Implementierung des Services: Der Cloud-Regler

Das Kapitel "Service" (Kapitel 5 im Entwurf) ist aktuell leer. Dies ist der Teil, in dem die Ingenieursleistung (Teilaufgabe 3) erbracht wird. Es wird dringend empfohlen, die **FiLiP (Fiware Library for Python)** zu nutzen, die im N5GEH-Umfeld entwickelt wurde.25 Sie abstrahiert die komplexe HTTP-Kommunikation mit dem Context Broker.

### 5.1 Regelungsalgorithmus (Economic Dispatch)

Die Aufgabenstellung fordert eine Kostenminimierung. Dies kann nicht durch statische Schwellwerte erreicht werden. Der Service muss ein Python-Skript sein, das als Docker-Container parallel zur Plattform läuft.

**Logik-Entwurf:**

1. **Data Ingestion (Read):**
    
    - Abfrage der aktuellen Entitäten `Boiler` (Status), `HeatPump` (Status), `Buffer` (Temperaturen) und `Weather` (Außentemperatur) über den Context Broker.
        
2. **State Assessment:**
    
    - Berechnung des Speicher-Ladezustands (SoC).
        
    - Prüfung der Constraints: Ist die Mindestlaufzeit des Kessels (40 min) abgelaufen?.1 Wenn nein $\rightarrow$ `Keep_On`.
        
3. **Economic Calculation:**
    
    - Berechnung der Wärmegestehungskosten für die nächste Stunde.
        
    - $K_{Pellet} = \frac{\text{Preis}_{Pellet}}{\eta_{Kessel}}$ (nahezu konstant).
        
    - $K_{WP} = \frac{\text{Preis}_{Strom}}{COP(T_{Außen}, T_{Vorlauf})}$.
        
    - Der COP ist hochgradig variabel. Eine einfache Approximation ist: $COP = \eta_{Carnot} \cdot \frac{T_{Vorlauf}}{T_{Vorlauf} - T_{Außen}}$ (Temperaturen in Kelvin).
        
4. **Decision Making:**
    
    - Wenn $K_{WP} < K_{Pellet}$ und $T_{Außen} > T_{Bivalenz}$ $\rightarrow$ Wähle WP.
        
    - Sonst $\rightarrow$ Wähle Pelletkessel.
        
5. **Actuation (Write):**
    
    - Senden eines `PATCH`-Requests an das Attribut `command` der jeweiligen Entität. Der IoT Agent leitet dies als MQTT-Befehl an die WAGO-Steuerung weiter.
        

### 5.2 Code-Struktur mit FiLiP

Der folgende Pseudocode illustriert, wie die Implementierung in der Arbeit dargestellt werden sollte:

Python

```
from filip.clients.ngsi_v2 import ContextBrokerClient
from filip.models.ngsi_v2.context import ContextAttribute

def control_loop():
    # Verbindung zum Broker
    client = ContextBrokerClient(url="http://localhost:1026")
    
    # 1. Daten abrufen
    boiler_entity = client.get_entity("urn:ngsi-ld:Boiler:001")
    buffer_entity = client.get_entity("urn:ngsi-ld:Buffer:001")
    
    # Attribute extrahieren
    temp_top = buffer_entity.get_attribute("temperature_top").value
    boiler_status = boiler_entity.get_attribute("status").value
    runtime = boiler_entity.get_attribute("currentRuntime").value
    
    # 2. Logik (Beispiel: Mindestlaufzeit-Constraint)
    command = "OFF" # Default
    
    if boiler_status == "ON":
        if runtime < 40: # Minuten
            command = "ON" # Zwangslauf (Constraint)
            print("Constraint Active: Minimum Runtime not reached.")
        elif temp_top > 75: # Speicher voll
            command = "OFF"
        else:
            # Hier ökonomische Entscheidung einfügen
            pass 
            
    # 3. Befehl senden (nur bei Änderung)
    if command!= boiler_status:
        client.update_attribute_value(entity_id="urn:ngsi-ld:Boiler:001", 
                                      attr_name="command", 
                                      value=command)
```

Dieser Code-Ausschnitt demonstriert die praktische Anwendung der Theorie und erfüllt die Anforderung der "praktischen Umsetzung".26

---

## 6. Handlungsempfehlung und Zeitplan (3-Monats-Sprint)

Der aktuelle Status der Arbeit entspricht einem Rohbau (ca. 30%). Um in drei Monaten eine verteidigungsfähige, exzellente Arbeit vorzulegen, ist strikte Disziplin erforderlich.

### Phase 1: Konsolidierung & "Hello World" (Monat 1)

- **Woche 1-2: Plattform-Setup.** Nutzen Sie das `n5geh.platform` Repository auf GitHub.14 Installieren Sie Docker und starten Sie den Stack (Orion, MongoDB, IoT Agent). Ziel: Ein `curl`-Befehl muss eine Entität anlegen und abrufen können.
    
- **Woche 3: Datenmodell-Definition.** Schreiben Sie die JSON-Strukturen für Kessel, WP und Puffer final nieder – einmal für v2, einmal für LD. Nutzen Sie die _Smart Data Models_ 29 als Vorlage, um nicht bei Null anzufangen.
    
- **Woche 4: Konnektivität.** Verbinden Sie (falls vorhanden) die reale WAGO-Steuerung oder schreiben Sie einen Simulator (Python-Skript), der MQTT-Daten sendet. Prüfen Sie, ob die Daten in der CrateDB landen (via Grafana visualisieren).
    

### Phase 2: Service-Entwicklung & Experiment (Monat 2)

- **Woche 5-6: Coding.** Implementieren Sie den Regel-Algorithmus in Python unter Nutzung von FiLiP. Fangen Sie simpel an (Hysterese), dann fügen Sie die Kostenkomponente hinzu.
    
- **Woche 7: Das Experiment.** Lassen Sie das System (oder die Simulation) in zwei Modi laufen:
    
    - _Modus A:_ Klassische thermische Führung (Referenz).
        
    - _Modus B:_ N5GEH-Optimierte Führung (Kosten/Taktung).
        
    - Loggen Sie alle Daten! Sie brauchen diese Plots für die Auswertung.
        
- **Woche 8: NGSI-LD Implementierung.** Setzen Sie das gleiche Szenario mit Orion-LD auf. Dokumentieren Sie die Unterschiede: War es schwieriger? Lief es langsamer? Wie sehen die Payloads aus?
    

### Phase 3: Synthese & Niederschrift (Monat 3)

- **Woche 9:** Schreiben des Kapitels "Methodik" und "Implementierung". Dokumentieren Sie den Code und die Architekturdiagramme.
    
- **Woche 10:** Auswertung. Erstellen Sie Vergleichsgraphen (z.B. "Anzahl Kesselstarts pro Tag" in Modus A vs. B). Berechnen Sie die theoretische Kostenersparnis.
    
- **Woche 11:** Schreiben der Diskussion (Kapitel 6). Bewerten Sie kritisch: Ist NGSI-LD den Mehraufwand wert? (Antwort: Für ein EFH vielleicht nein, für ein Smart Grid ja).
    
- **Woche 12:** Finalisierung. Einleitung, Zusammenfassung, Formatierung, Quellenverzeichnis.
    

---

## 7. Kritische Anmerkungen zum Entwurf

1. **Fehlende Abbildungen:** Der Entwurf enthält Platzhalter für Grafiken. Diese müssen professionell erstellt werden (keine Handskizzen oder Fotos). Nutzen Sie Tools wie draw.io oder Visio für das Hydraulikschema und UML-Diagramme für das Datenmodell.
    
2. **Quellenarbeit:** Platzhalter wie `[quelle]` sind in diesem Stadium kritisch. Ersetzen Sie diese sofort. Primärquellen sind die ETSI-Spezifikationen für NGSI-LD, die FIWARE-Dokumentation und Paper aus dem N5GEH-Umfeld.
    
3. **Präzision:** Die Bezeichnung "WAGO Sharky" im Entwurf ist technisch unsauber. Der Sharky ist von Diehl Metering. Die WAGO ist der Controller. Solche Ungenauigkeiten mindern die technische Glaubwürdigkeit. Korrekt: "Wärmemengenzähler Diehl Sharky 775, angebunden via M-Bus Modul an WAGO PFC200, Gateway-Funktion via MQTT".15
    
4. **Argumentation:** Die These, NGSI-LD sei "nicht maschinenlesbar ohne Context", muss korrigiert werden. Es ist _gerade_ für Maschinen gemacht. Der Context ist der Schlüssel, nicht das Hindernis.
    

## 8. Fazit

Die Arbeit adressiert ein hochaktuelles und relevantes Thema an der Schnittstelle von Energietechnik und Informatik. Das Potenzial für eine sehr gute Bewertung ist vorhanden, hängt aber kritisch von der _praktischen_ Durchführung des Vergleichs ab. Ein reiner Literaturvergleich reicht nicht aus. Die Implementierung des Services (Python/FiLiP) und die Messung der Ergebnisse (Taktung/Kosten) sind der Beweis für die Anwendbarkeit der Theorie. Fokussieren Sie sich jetzt auf das "Tun" – die Plattform muss laufen, damit Sie Daten generieren können.