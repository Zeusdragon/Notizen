---
title: CFD-Simulation von Verschmutzung am Lamellenverdampfer
type: projektidee
tags:
  - waermepumpe
  - fouling
  - cfd
  - ansys
status: teilarbeitspaket
prioritaet: mittel
erstellt: 2026-08-24
verwandt:
  - "[[02 Verschmutzungserkennung WP]]"
---

# CFD-Simulation von Verschmutzung am Lamellenverdampfer

> [!warning] Einordnung
> Als eigenständiges Forschungsprojekt **nicht tragfähig** – das Feld ist seit den 1980ern besetzt. Als Arbeitspaket innerhalb von [[02 Verschmutzungserkennung WP]] dagegen sinnvoll und notwendig.

---

## 1 Was es schon gibt

- Experimentelle Fouling-Untersuchungen an Lamellenwärmeübertragern seit 1980; Ablagerung konzentriert sich fast ausschließlich in den ersten Millimetern der Anströmkante
- Standardisiertes Testprotokoll für luftseitiges Fouling (Purdue, 2023)
- Vollständige Fouling-Modelle in ANSYS Fluent mit DPM + Dynamic Mesh, eigenen Ablagerungs- und Ablösealgorithmen
- Gekoppelte CFD-DEM-Simulationen mit nicht-sphärischen und elastischen Partikeln
- Aufgeschlüsselte Depositionsmechanismen: Impaktion an Lamellenkanten, Gravitationssedimentation, turbulente Impaktion, Brownsche Diffusion

**Fazit:** "Wir simulieren Staubablagerung mit DPM" bekommt im Gutachten *bekannt*.

## 2 Tatsächliche Lücken

- [ ] **Fouling × Frosting-Kopplung** – nur vereinzelt bearbeitet, Befund: bezogen auf den Wärmeübergang wächst Reif auf verschmutzter Lamelle stärker
- [ ] **Reale Außenluft** statt ISO-Prüfstaub – Pollen, Blätter, Straßensalz-Aerosol, Ruß
- [ ] **Biologische und nicht-sphärische Partikel** – Adhäsionsdaten praktisch nicht vorhanden
- [ ] Kombinationsphänomene: Staub + Feuchte = Schlamm, Pollen + Reif

## 3 Technische Komplexität

| Aspekt | Problem |
|---|---|
| Zeitskalen | Fouling wächst über Monate, CFD-Zeitschritt im Millisekundenbereich → nur quasistationäre Schleife möglich |
| Fehlendes Modell | Fluent hat kein Fouling-Modell; DPM-Accretion ändert die Geometrie nicht |
| Haftkriterien | kritische Aufprallgeschwindigkeit, JKR/DMT-Adhäsion – Parameter für Pollen auf Alu bei 80 % r.F. existieren nicht |
| Vernetzung | Vollverdampfer nicht rechenbar → periodische Einheitszelle, CHT zwingend |
| Kältemittelseite | zweiphasige Verdampfung nicht koppelbar → konstante Wandtemperatur oder aufgeprägter α |
| Blätter | kein Partikelmodell mehr – 6-DOF-Körper oder reine Verblockungsstudie |

> [!note] Terminologie
> Das ist **FVM** (Fluent/CFX), nicht FEM. FEM wäre ANSYS Mechanical. In Anträgen und Gesprächen darauf achten.

## 4 Stufenmodell

| Stufe | Inhalt | Aufwand |
|---|---|---|
| 0 | Schmutz als Wärmeleitwiderstand + Querschnittsverengung auf saubere Geometrie | Tage–Wochen |
| 1 | DPM-Einweg: Wo landen Partikel? Abscheidegrad über Stokes-Zahl | Wochen–Monate |
| 2 | Quasistationäre Schleife mit Schichtwachstum, Remeshing | Monate |
| 3 | Phasenwechsel, Adhäsion, aufgelöste Blätter | Dissertation |

**Für [[02 Verschmutzungserkennung WP]] reicht Stufe 1.**

## 5 Konkrete Fragen, die CFD hier beantworten soll

- [ ] Wo lagert sich Schmutz bei dieser Lamellengeometrie zuerst ab? → **Sensorposition**
- [ ] Welche Wandschubspannung erfährt eine Ablagerung? Welcher Strahldruck löst sie? → **Reinigungsdüse**
- [ ] Erreicht ein Strahl die dritte Rohrreihe? → **Düsenanordnung**
- [ ] Wie bildet eine ungleichmäßige Verteilung auf ein integrales UA-/Δp-Signal ab? → **Kalibrierung des 1D-Modells**

## 6 Werkzeuge

- **Fluent + DPM** – Basis
- **Rocky DEM (gekoppelt)** – nicht-sphärische und flexible Partikel, Adhäsionsmodelle → für Pollen und Blätter deutlich besser
- **Fluent Icing / FENSAP-ICE** – wachsende Eisschicht inkl. Rauheit und CHT, aber auf Flugzeugvereisung kalibriert, nicht auf Schneeanwehung

- [ ] Lizenzverfügbarkeit prüfen – beides sind separate Lizenzen

## 7 Übungsprojekt zum Einstieg

Falls es primär ums Lernen geht, unabhängig vom Forschungsprojekt:

- [ ] Periodische Lamellen-Einheitszelle aufbauen
- [ ] CHT sauber rechnen
- [ ] Gegen publizierte Nu- und f-Korrelation validieren
- [ ] Gleichmäßige Schmutzschicht als Widerstand + Verengung aufprägen
- [ ] Kennlinien Δp und UA über Verschmutzungsgrad erzeugen

Zwei bis vier Wochen, deckt ~80 % der relevanten Technik ab, und ist die Basis für AP2.

> [!important] Grundsatz
> Verschmutzungssimulation ist ein **Vergleichs-, kein Vorhersagewerkzeug**. Ohne begleitende Versuche gibt es keine Absolutwerte, nur Trends ("Geometrie A verschmutzt 30 % langsamer als B"). Erwartungshaltung früh klarstellen.
