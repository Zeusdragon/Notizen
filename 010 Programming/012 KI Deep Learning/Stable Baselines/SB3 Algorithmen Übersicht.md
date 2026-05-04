---
tags:
  - MOC
  - ReinforcementLearning
  - StableBaselines3
---
# Stable Baselines 3 - Algorithmen Übersicht

## On-Policy Algorithmen
*Lernen nur aus den aktuell gesammelten Daten der jetzigen Strategie.*

### [[PPO (Proximal Policy Optimization)]]
- **Kurz:** Der Goldstandard. Limitiert Strategie-Updates, um katastrophales Vergessen zu verhindern.
- **Vorteile:** Extrem stabil, einfach zu konfigurieren, für diskrete & kontinuierliche Räume.
- **Nachteile:** Geringere Dateneffizienz (braucht viele Interaktionen). Zu langsam
- **Use Case:** Allzweckwaffe. Eignet sich für fast alles (Spiele, Robotik, Wirtschaft), wenn genug Rechenleistung/Simulationszeit vorhanden ist.

### [[A2C (Advantage Actor Critic)]]
- **Kurz:** Basis-Actor-Critic Methode, die den "Advantage" (Vorteil) einer Aktion bewertet.
- **Vorteile:** Stabil, unterstützt alle Aktionsräume.
- **Nachteile:** PPO ist fast immer überlegen.
- **Use Case:** Als schnelle Baseline zum Testen oder bei stark limitierten Hardwareressourcen.

---
## Off-Policy Algorithmen
*Lernen dateneffizient aus einem Speicher (Replay Buffer) vergangener Erfahrungen.*

### [[SAC (Soft Actor-Critic)]]
- **Kurz:** Maximiert Belohnung UND Entropie (Zufälligkeit) für extrem robuste Strategien.
- **Vorteile:** Sehr hohe Dateneffizienz, findet sichere und "breite" Lösungswege.
- **Nachteile:** Rechenintensiv, primär für kontinuierliche Räume.
- **Use Case:** Reale Robotik, autonome Fahrzeuge und komplexe physikalische Simulationen (kontinuierliche Steuerung).

### [[DQN (Deep Q-Network)]]
- **Kurz:** Sagt den exakten Wert (Q-Wert) jeder möglichen Aktion in einem Zustand voraus.
- **Vorteile:** Sehr dateneffizient, stark bei klassischen Problemen.
- **Nachteile:** Nur für diskrete Aktionen, tendiert zu Overestimation Bias.
- **Use Case:** Atari-Spiele, Brettspiele, Trading-Entscheidungen (Kaufen/Verkaufen/Halten) - alles mit klar abgegrenzten, diskreten Handlungsmöglichkeiten.

# [[QR-DQN (Quantile Regression DQN)]]
- **Kurz:** Eine "distributionelle" Erweiterung von DQN. Schätzt nicht nur den Durchschnittswert einer Aktion, sondern die gesamte Wahrscheinlichkeitsverteilung der Belohnung. 
- **Vorteile:** Eliminiert das Überschätzungs-Problem (Overestimation Bias) von DQN fast komplett; wesentlich robuster und leistungsfähiger als Standard-DQN; extrem dateneffizient.
- **Nachteile:** Braucht minimal mehr Rechenleistung beim Netzwerk-Update als normales DQN; nur für diskrete Aktionsräume geeignet. 
- **Use Case:** Der beste Off-Policy Algorithmus für diskrete Aktionen (wie An/Aus-Schalter, Trading, Abtauung). Ideal, wenn die Simulation (z.B. eine FMU) sehr langsam ist und Dateneffizienz Priorität hat. *Achtung: Ist in `sb3-contrib` enthalten, nicht im Hauptpaket.*

### [[TD3 (Twin Delayed DDPG)]]
- **Kurz:** Verbessertes DDPG. Nutzt zwei Critics und verzögerte Updates zur Stabilisierung.
- **Vorteile:** Sehr effizient, löst die Instabilität von DDPG.
- **Nachteile:** Nur für kontinuierliche Räume.
- **Use Case:** Deterministische Umgebungen mit kontinuierlicher Steuerung, oft eine gute Alternative zu SAC.

### [[DDPG (Deep Deterministic Policy Gradient)]]
- **Kurz:** Wie DQN, aber für kontinuierliche Räume mit deterministischer Strategie.
- **Vorteile:** Verarbeitet kontinuierliche Aktionen dateneffizient.
- **Nachteile:** Sehr instabil im Training, empfindlich.
- **Use Case:** Wird heute meist durch TD3 oder SAC ersetzt. Historisch für kontinuierliche Steuerung genutzt.

---
## Erweiterungen (Buffer)

### [[HER (Hindsight Experience Replay)]]
- **Kurz:** Ein Replay Buffer, der aus Fehlern lernt, indem er verfehlte Endzustände als neues Ziel deklariert.
- **Vorteile:** Ermöglicht Lernen bei extrem seltenen Belohnungen ("Sparse Rewards").
- **Nachteile:** Benötigt Goal-Conditioned Environments.
- **Use Case:** Greifroboter (hat das Objekt gegriffen oder nicht) oder Navigation (Ziel erreicht oder nicht).