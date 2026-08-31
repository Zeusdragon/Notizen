---
title: "Dymola & TIL Library"
type: konzept
status: fertig
tags:
  - simulation
  - modelica
  - thermodynamics
  - digital-twin
  - engineering
erstellt: 2025-12-05
---

# Dymola & TIL Library

**Software:** Dassault Systèmes Dymola
**Library:** TLK-Thermo TIL Suite

---

## 1. Was ist Dymola?
Dymola (Dynamic Modeling Laboratory) ist eine Simulationsumgebung basierend auf der Sprache **Modelica**.
* **Akausal:** Anders als in Simulink (Signalfluss) werden hier physikalische Gleichungen modelliert (Energieerhaltung, Massenbilanz). Der Datenfluss ergibt sich automatisch.
* **Multi-Domain:** Mechanik, Elektrik, Thermodynamik und Regelungstechnik können in einem Modell kombiniert werden.
* **Objektorientiert:** Komponenten (z.B. ein Ventil) werden einmal definiert und können wiederverwendet werden.



## 2. Die TIL Suite (TLK-Thermo)
Die TIL Library ist eine spezialisierte Add-on Bibliothek für **thermodynamische Systeme**. Sie ist der Standard für die Simulation von Wärmepumpen, Kältekreisläufen und Klimatisierung.

### Hauptkomponenten
* **TIL.HeatExchangers:** Plattenwärmetauscher, Rohr-in-Rohr, Fin-and-Tube.
* **TIL.VLEFluidTypes:** Kältemittel (R134a, R290, CO2 etc.) und Öle. Berechnet Realgaseigenschaften.
* **TIL.Components:** Verdichter (Compressors), Pumpen, Ventile.

> **Wichtig:** TIL ist sehr detailliert. Die Simulationen sind genau, können aber rechenintensiv sein (wichtig für RL-Training!).

## 3. Workflow: Vom Modell zur AI
Wie kommt das physikalische Modell in dein PyTorch-Projekt?

### A. Modellierung in Dymola
1.  **Drag & Drop:** Komponenten aus dem Library-Browser in das Diagramm ziehen.
2.  **Verbinden:** Ports (Fluidports, Heatports) verbinden.
3.  **Parametrieren:** Geometriedaten (Länge, Durchmesser) und Startwerte eingeben.
4.  **Check & Simulate:** Modell kompilieren und testen.

### B. Export via FMI (Functional Mock-up Interface)
Um das Modell in Python zu nutzen, exportierst du es als **FMU (Functional Mock-up Unit)**.
* **Model Exchange:** Python übernimmt den Solver (langsamer, aber flexibel).
* **Co-Simulation (Empfohlen):** Dymola liefert den Solver in der FMU mit. Schnellere Ausführung.



## 4. Integration mit Python (PyTorch/RL)
Das Dymola-Modell dient als **Environment** für den RL-Agenten.

**Tools zur Anbindung:**
* **FMPy:** Um FMUs in Python zu laden und zu simulieren.
* **PyFMI:** Alternativbibliothek für komplexe Interaktionen.

**Der Loop (Pseudocode für RL):**
```python
# Initialisierung
fmu = load_fmu("Waermepumpe.fmu")

# Training Step
def step(action):
    # 1. Aktion an FMU übergeben (z.B. Verdichterdrehzahl)
    fmu.set("compressor.speed", action)
    
    # 2. Zeitschritt simulieren
    fmu.do_step(current_time, step_size)
    
    # 3. Zustand auslesen (z.B. Temperatur)
    state = fmu.get("sensor.T")
    
    return state