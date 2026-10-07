# Modelltreue Test

## Testmatrix

| RunID | Stufe | Konfiguration | FinSideHT | TubeSideHT | FrostModel | Geometrie |
|---:|:---:|---|---|---|---|---|
| 1 | 1 | BASIS (aktueller Stand) | ConstantAlpha | ConstantAlpha | ConstPorosity_noRth | Referenz |
| 2 | 1 | nur Luftseite -> Haaf | Haaf | ConstantAlpha | ConstPorosity_noRth | Referenz |
| 3 | 1 | nur Luftseite -> FinSide_Alt3 | FinSide_Alt3 | ConstantAlpha | ConstPorosity_noRth | Referenz |
| 4 | 1 | nur Rohrseite -> Steiner | ConstantAlpha | Steiner...GnielinskiDittusBoelter | ConstPorosity_noRth | Referenz |
| 5 | 1 | nur Frost -> ConstPorosity_mitRth | ConstantAlpha | ConstantAlpha | ConstPorosity_mitRth | Referenz |
| 6 | 1 | nur Frost -> Frost_Alt3 | ConstantAlpha | ConstantAlpha | Frost_Alt3 | Referenz |
| 7 | 1 | nur Frost -> Frost_Alt4 | ConstantAlpha | ConstantAlpha | Frost_Alt4 | Referenz |
| 8 | 3 | EINFACH | ConstantAlpha | ConstantAlpha | ConstPorosity_noRth | Referenz |
| 9 | 3 | EINFACH | ConstantAlpha | ConstantAlpha | ConstPorosity_noRth | parallelTube=20.0 (min) |
| 10 | 3 | EINFACH | ConstantAlpha | ConstantAlpha | ConstPorosity_noRth | parallelTube=31.5 (max) |
| 11 | 3 | EINFACH | ConstantAlpha | ConstantAlpha | ConstPorosity_noRth | serialTube=15.0 (min) |
| 12 | 3 | EINFACH | ConstantAlpha | ConstantAlpha | ConstPorosity_noRth | serialTube=26.0 (max) |
| 13 | 3 | MITTEL | Haaf | ConstantAlpha | ConstPorosity_noRth | Referenz |
| 14 | 3 | MITTEL | Haaf | ConstantAlpha | ConstPorosity_noRth | parallelTube=20.0 (min) |
| 15 | 3 | MITTEL | Haaf | ConstantAlpha | ConstPorosity_noRth | parallelTube=31.5 (max) |
| 16 | 3 | MITTEL | Haaf | ConstantAlpha | ConstPorosity_noRth | serialTube=15.0 (min) |
| 17 | 3 | MITTEL | Haaf | ConstantAlpha | ConstPorosity_noRth | serialTube=26.0 (max) |
| 18 | 3 | VOLL | Haaf | Steiner... | ConstPorosity_mitRth | Referenz |
| 19 | 3 | VOLL | Haaf | Steiner... | ConstPorosity_mitRth | parallelTube=20.0 (min) |
| 20 | 3 | VOLL | Haaf | Steiner... | ConstPorosity_mitRth | parallelTube=31.5 (max) |
| 21 | 3 | VOLL | Haaf | Steiner... | ConstPorosity_mitRth | serialTube=15.0 (min) |
| 22 | 3 | VOLL | Haaf | Steiner... | ConstPorosity_mitRth | serialTube=26.0 (max) |

## Zu überprüfende Größen

| Nr. | Größe                           | Symbol    | Einheit | Bedeutung / Begründung                                                                                      | Toleranz |
| --: | ------------------------------- | --------- | ------- | ----------------------------------------------------------------------------------------------------------- | -------- |
|   1 | COP-Abweichung                  | ΔCOP      | – / %   | Hauptkriterium für die Effizienz                                                                            | tbd      |
|   2 | **Abtauintervall**              | t_int     | min     | Zentrale Zeitkonstante der Arbeit; gleiches ΔCOP bei unterschiedlichem Intervall → Übereinstimmung zufällig | tbd      |
|   3 | Reifmasse zum Auslösezeitpunkt  | m_frost   | kg      | Physikalische Größe hinter der Abtauentscheidung                                                            | tbd      |
|   4 | Mittlere Verdampfungstemperatur | T_evap,m  | °C      | Treibt sowohl Vereisungsrate als auch COP                                                                   | tbd      |
|   5 | Luftmassenstrom am Zyklusende   | ṁ_air,end | kg/s    | Maß für die Verblockung des Verdampfers                                                                     | tbd      |
|   6 | Abtaudauer                      | t_def     | min     | Kosten pro Zyklus (Zeit)                                                                                    | tbd      |
|   7 | Abtauenergie                    | Q_def     | kJ      | Kosten pro Zyklus (Energie)                                                                                 | tbd      |

### Ergebnisse

| RunID | ΔCOP | t_int | m_frost | T_evap,m | ṁ_air,end | t_def | Q_def | zulässig? |
|---:|---|---|---|---|---|---|---|:---:|
| 1 | | | | | | | | |
| 2 | | | | | | | | |
| 3 | | | | | | | | |
| 4 | | | | | | | | |
| 5 | | | | | | | | |
| 6 | | | | | | | | |
| 7 | | | | | | | | |
| 8 | | | | | | | | |
| 9 | | | | | | | | |
| 10 | | | | | | | | |
| 11 | | | | | | | | |
| 12 | | | | | | | | |
| 13 | | | | | | | | |
| 14 | | | | | | | | |
| 15 | | | | | | | | |
| 16 | | | | | | | | |
| 17 | | | | | | | | |
| 18 | | | | | | | | |
| 19 | | | | | | | | |
| 20 | | | | | | | | |
| 21 | | | | | | | | |
| 22 | | | | | | | | |

### Prüfkriterium (vorab festgelegt)

- Eine Vereinfachung gilt **nur dann als zulässig**, wenn **sowohl ΔCOP als auch das Abtauintervall** innerhalb der Toleranz bleiben.
- Ist ΔCOP gleich, das Abtauintervall aber deutlich verschoben (z. B. um 30 %), liegt ein **kompensierender Fehler** vor → das ist ein Ergebnis, kein Problem, und wird dokumentiert.
