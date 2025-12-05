
---
# 🎓 Exposé: Neural MPC für Wärmepumpen-Abtauung

## Arbeitstitel
**Entwicklung und simulative Validierung einer datengetriebenen Model Predictive Control (MPC) Strategie zur bedarfsgerechten Abtauung von Luft-Wasser-Wärmepumpen**

## 1. Hintergrund & Motivation

> [!SUMMARY] Kontext
> Die Vereisung von Verdampfern bei Luft-Wasser-Wärmepumpen beeinträchtigt die Energieeffizienz (COP) massiv. Konventionelle Strategien (starre Zeitintervalle) sind oft ineffizient. Aktuelle Forschung (Jonas Klingebiel, 2025) setzt auf **Reinforcement Learning (RL)**.
> 
> **Problem bei RL:** "Black-Box"-Charakter, instabiles Training, schwer nachweisbare Sicherheit.
> **Lösung dieser Arbeit:** Nutzung von **[[Model Predictive Control]] (MPC)** als robuste, physikalisch motivierte Alternative. Da das physikalische Modell ([[Dymola]]) für Echtzeit-Regelung zu langsam ist, wird ein **"Neural MPC"** Ansatz verfolgt (Surrogate Model in [[PyTorch]]).

## 2. Zielsetzung
Entwicklung eines MPC-Reglers, der den **optimalen Startzeitpunkt für die Abtauung** bestimmt Abtauung selber Zeitgesteuert mit fixer Zeit oder Stellgröße Zustand Abtauuen/Heizen .
* **Optimierungsziel:** Maximierung der Heizleistung vs. Minimierung der Abtau-Energie.
* **Methode:** Datengetriebenes MPC unter Nutzung eines [[Neuronale Netze|Neuronalen Netzes]] als schnelles Vorhersagemodell.
* **Basis:** Detailliertes Dymola-Modell als "Ground Truth" (Ersatz für reale Hardware).

---

## 3. Geplante Arbeitspakete

### AP 1: Literatur & Grundlagen
- [ ] Recherche: Physik der Reifbildung & Abtauung.
- [ ] Vergleich: Regelbasiert vs. RL vs. MPC.
- [ ] Deep Dive: "Neural MPC" und Surrogate Modeling in der Regelungstechnik.
- [ ] Paper-Sammlung anlegen (z.B. in Zotero/Obsidian).

### AP 2: Systemanalyse & Daten (Dymola)
- [ ] Einarbeitung in das bestehende [[Dymola-Modell]].
- [ ] Kostenfunktion für MPC entwickeln 