# ⚙️ Regelung von Wärmepumpen Master-Note

**Modul:** [[Regelung von Wärmepumpen]]
**Tags:** #Wärmepumpe #Regelung #Ventile #Anlagentechnik
**Status:** 🟡 In Bearbeitung

---

## 1. Flexible Anlagen & Steuerung
Da Kälte nicht einfach "additiv" hinzugefügt werden kann (anders als Heizen mit E-Stab), müssen Anlagen für den **schlimmsten Lastfall** ausgelegt sein.

### Ventil-Arten im Vergleich

| Art | Funktionsweise | Merkmale |
| :--- | :--- | :--- |
| **Direktgesteuert** | E-Motor mit Magnet öffnet/schließt direkt. | Simpel. |
| **Vorgesteuert** | Servomotor nutzt Druckdifferenz zum Öffnen. | + Klein, leicht, günstig<br>+ Hohe Drücke möglich<br>- Braucht **Mindestdruckdifferenz** |
| **Zwangsgesteuert** | Mechanische Kopplung. | + Kein Mindestdruck nötig<br>+ Niedrigere Drücke<br>- Größer, schwerer, teurer |

### Steuerungstechnik
* **Schütze:** Schalter mit Steuerleistung (Motor An/Aus). Wie eine Sicherung, aber steuerbar.
* **Verarbeitung:**
    * **Analog:** Einfache Schütze/Schaltpläne. *Nachteil:* Keine Drehzahlregelung, keine Abtauerkennung.
    * **Digital:** Kleinsteuergeräte (µC, FPGA), SPS oder Computer (LabVIEW). Ermöglicht komplexe Logik.

---

## 2. Transientes Verhalten
Beschreibt das Verhalten der Anlage **während Zeitverläufen** (im Gegensatz zum stationären Zustand).

**Wichtige Phasen:**
* **Anfahrverhalten:** Was passiert im Kreislauf beim Start? (Druckaufbau, Ölrückführung).
* **Störgrößen:** Verhalten nach Lastwechseln, bis wieder Stationarität eintritt.

> [!warning] Simulation
> Numerische Simulation von Zweipunktreglern (An/Aus) ist schwierig, da Programme oft numerisch instabil werden, wenn Werte "sprunghaft" wechseln (in Realität fährt die Anlage langsam hoch/runter).