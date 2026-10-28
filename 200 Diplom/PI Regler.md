---
tags: [diplomarbeit, kapitel3, regelung, modellierung]
---

# Parametrierung der PI-Regler

## Was geregelt wird
- **EXV** → Öffnungsgrad auf konstante Unterkühlung
- **Verdichter** → Drehzahl auf Vorlauftemperatur
- **Pumpe** → Drehzahl auf Spreizung im Sekundärkreis
- Alle drei wirken auf dasselbe System → Kopplung
  (Öffnungsgrad verschiebt Verdampfungsdruck → Arbeitspunkt Verdichter)

## Vorgehen
- Manuelle Parametrierung, **kein** systematisches Verfahren (kein Ziegler-Nichols o. Ä.)
- Prüffall: **Wiedereinschwingen nach einem Abtauvorgang**
  - größte Störung im Betrieb: Kreislaufumkehr dreht Druckniveaus + Richtung der Wärmeströme
  - wer das beherrscht, beherrscht auch jeden Lastwechsel
- Prinzip: **Zeitskalentrennung**
  - EXV schnell ≫ Verdichter ≫ Sekundärkreis
  - schnellere Kreise wirken auf langsamere quasistationär → kein Aufschaukeln
  - entspricht dem Standardgedanken der Kaskadenregelung

## Abbruchkriterium
Vier Größen, alle nach Abtauende schwingungsfrei im stationären Verlauf:

| Größe                  | Rolle                             |
| ---------------------- | --------------------------------- |
| Unterkühlung           | Regelgröße EXV                    |
| Verdichterfrequenz     | **Stellgröße** Verdichterregler   |
| Verdampfungstemperatur | Kopplung beider Kreise            |
| Heizleistung           | Ausgangsgröße / Wirkung auf Senke |
- Warum auch die **Stellgröße**: Stellgrößenschwingungen treten oft auf, bevor die Regelgröße sichtbar schwingt → früheres Warnsignal 
- Warum **Verdampfungstemperatur** besonders wichtig: 
	- geht in bedarfsgesteuerte Abtauregelung ein (ΔT-Schwellwert) 
	- geht in den Zustandsraum des RL-Agenten ein 
	- → Schwingung hier stört beide Abtauregler direkt 
	- Systematik: **je Regelkreis eine Größe + Ausgangsgröße**, nicht willkürlich gewählt

## Nachweis über den Betriebsbereich
- Stabilität bestätigt über 4 °C / 0 °C / −4 °C aus der Modelltreueuntersuchung
- ⚠️ vorher Verläufe prüfen, ob das wirklich für alle drei gilt

## Für die Grenzen
> Parameter auf stabiles Betriebsverhalten eingestellt, **nicht** auf Regelgüte optimiert.
> Alle Abtaustrategien nutzen dieselbe Anlagenregelung → Vergleich zwischen ihnen unberührt.

- gleiche Argumentationsfigur wie beim Verflüssiger (systematischer Fehler, wirkt auf alle gleich)


- ✅ Kriterium + Hierarchie nennen

## Offen
- [ ] Reihenfolge der Zeitskalen gegen eigene Werte prüfen
	- [ ] Ja es ist EXV,Verdichter und dann Pumpe
- [ ] Reglerparameter in Anhang aufnehmen