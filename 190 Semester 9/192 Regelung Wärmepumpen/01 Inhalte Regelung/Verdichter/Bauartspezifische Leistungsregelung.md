**Vorlesung**: [[192 Regelung von Wärmepumpen]]
**Thema**: [[Verdichterregelung]]
**Tags**: #Hubkolben #Schraube #Scroll #Turbo

---

# Bauartspezifische Leistungsregelung

Mechanische Eingriffe in den Verdichtungsprozess.

## 1. Hubkolbenverdichter: Zylinderabschaltung
Bei Mehrzylinder-Verdichtern (z.B. 4, 6, 8 Zylinder) werden einzelne Zylinderbänke "leer" mitlaufen gelassen.
* **Technik:** Ein Magnetventil sperrt den Saugkanal des Zylinders oder blockiert das Saugventil in offener Stellung. Der Kolben bewegt sich, verdichtet aber nicht.
* **Regelung:** Stufig (z.B. 100% - 66% - 33%).
* **Effizienz:** Gut, da Reibungsverluste zwar bleiben, aber keine Verdichtungsarbeit geleistet wird.

## 2. Schraubenverdichter: Regelschieber
Der **Steuerschieber** ist ein bewegliches Bauteil im Gehäuse der Schraube.
* **Funktion:** Er verschiebt den Punkt, an dem die Kompression beginnt (wirksame Schraubenlänge wird verkürzt).
* **Vorteil:** Erlaubt eine stetige Regelung von ca. 25% bis 100%.
* **$V_i$-Verstellung:** Moderne Schrauben verstellen parallel das interne Volumenverhältnis ($V_i$), um Teillast-Verluste durch Über-/Unterkompression zu vermeiden.

## 3. Scrollverdichter: Digital Scroll
Eine Lizenztechnologie (Copeland/Emerson).
* **Prinzip:** PWM (Pulsweitenmodulation).
* **Mechanik:** Die feststehende Spirale wird axial um ca. 1 mm angehoben ("Lift-off"). In diesem Zustand strömt das Gas ohne Verdichtung durch (0% Last).
* **Zyklus:** Ein Zeitintervall (z.B. 15 Sekunden) wird aufgeteilt.
    * Z.B. 5s "Zusammen" (Last) + 10s "Auseinander" (Leerlauf) = 33% Kapazität.
* **Vorteil:** Sehr weiter Regelbereich (10-100%), einfachere Ölversorgung als beim Inverter.

## 4. Turboverdichter: Drallregelung
Einsatz von **Vorleitgittern (Inlet Guide Vanes - IGV)** vor dem Laufrad.
* Die Schaufeln geben dem Gas einen Drall *in* Drehrichtung des Laufrads mit.
* Dadurch sinkt die Relativgeschwindigkeit am Laufradeintritt $\to$ Druckaufbau und Massenstrom sinken.
* Verhindert das "Pumpen" (Surge) des Verdichters im Teillastbereich.