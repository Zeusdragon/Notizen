# 🧪 Forschungspraktikum: N5GEH & FIWARE

**Thema:** Funktionsvergleich der FIWARE-Plattform für verschiedene Datenmodelle (NGSI-v2 vs. NGSI-LD)
**Kontext:** Hybrides Wärmeversorgungssystem (Wärmepumpe + Pelletkessel)
**Status:** 🟡 In Bearbeitung
**Deadline:** [Datum einfügen]

---

## 🎯 Hauptziele (Roter Faden)

### 1. Theoretische Fundierung
- [x] **Systemanalyse:** Thermodynamik & Hydraulik verstehen (Kesselträgheit, COP der WP, Schichtung im Puffer) [[Arbeitsstrategie#2. Systemanalyse Thermodynamik und Hydraulik des hybriden Clusters|Link]]
- [ ] **Architektur:** N5GEH Referenzarchitektur (Southbound/Northbound, IoT Agents) [[Arbeitsstrategie#3. Die N5GEH-Architektur Vom Sensor zur Cloud|Link]]
- [ ] **Datenmodelle:** Tiefenvergleich NGSI-v2 vs. NGSI-LD (Semantik, Graphen, Linked Data) [[Arbeitsstrategie#4. Tiefenvergleich NGSI-v2 vs. NGSI-LD|Link]]

### 2. Praktische Implementierung
- [x] **Setup:** Docker-Stack aufsetzen (Orion, MongoDB, IoT Agent)
- [ ] **Modellierung:** JSON-Strukturen für `Boiler`, `HeatPump`, `Buffer` definieren (v2 & LD).
- [ ] **Service-Entwicklung:** Python-Skript (FiLiP) für Economic Dispatch (Kostenminimierung).
- [ ] **Experiment:** Vergleichsmessung (Modus A: Thermisch vs. Modus B: Optimiert).

### 3. Synthese & Writing
- [ ] Auswertung der Taktung und Kostenersparnis.
- [ ] Diskussion: Ist NGSI-LD den Mehraufwand wert?
- [ ] Finalisierung der Arbeit. 📅 2026-03-10 

---

## 📂 Struktur der Arbeit (Gliederung)

### 1. Einleitung
* Motivation: Sektorkopplung & Digitalisierung.
* Problemstellung: Datenmodelle als Kern der Interoperabilität.

### 2. Use Case: Das Hybride Cluster
* **Komponenten:**
	* Pelletkessel (Trägheit, Emissionen bei Start).
	* Wärmepumpe (COP-Abhängigkeit von $T_{Außen}$ und $T_{Vorlauf}$).
	* Pufferspeicher (Schichtung, SoC-Berechnung).
* **Vereinfachung:** Welche Parameter sind für die Cloud relevant?

### 3. Die Plattform (FIWARE / N5GEH)
* Aufbau: Southbound (WAGO/MQTT) $\to$ IoT Agent $\to$ Context Broker $\to$ Northbound.
* Zeitreihen: QuantumLeap & CrateDB für historische Analyse.

### 4. Datenmodelle im Vergleich
* **NGSI-v2:** Aufbau, JSON-Payload, Vor/Nachteile (Silos).
* **NGSI-LD:** Linked Data, `@context`, Ontologien (SAREF), Property Graphs.
* **Vergleich:** Interoperabilität vs. Komplexität vs. Payload-Größe.

### 5. Implementierung & Service
* **Datenpipeline:** Wie kommen die Daten rein? (MQTT Topics $\to$ Attribute).
* **Der Service (Cloud-Regler):** * Python mit FiLiP Library.
	* Algorithmus: $K_{WP}$ vs. $K_{Pellet}$ unter Berücksichtigung der 40min Mindestlaufzeit.

### 6. Ergebnisse & Diskussion
* Vergleich der Regelgüte.
* Fazit: Wann lohnt sich LD?

---

## 📝 Notizen & Ressourcen
* [[Meeting 1]] - Erste Absprachen
* [[Arbeitsstrategie]] - **Wichtiges Gutachten & Guide**
* [[Difference NGSI V2 and NGSI LD]]
* [[NGSI in 30 min concepts]]
* [[Notes]]

## Meetings
[[Meeting 1]], [[Meeting 2]], [[Meeting 3]], [[Meeting 4]], [[Meeting 5]], [[Meeting 6]], [[Meeting 7]]





