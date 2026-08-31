---
title: Frostbewusster prädiktiver Lastmanager für WP-Anlagen
type: projektidee
tags:
  - waermepumpe
  - mpc
  - regelungstechnik
  - lastmanagement
status: idee
prioritaet: mittel
erstellt: 2026-08-24
verwandt:
  - "[[02 Verschmutzungserkennung WP]]"
---

# Frostbewusster prädiktiver Lastmanager für WP-Anlagen

> [!warning] Einordnung
> Als Standard-MPC **massiv überbesetzt**. Prädiktive WP-Regelung gehört zu den meistpublizierten Themen der Gebäudeenergietechnik. "MPC schlägt Heizkurve um 10–30 %" ist seit über einem Jahrzehnt belegt (Dänemark 2013: ~7 % Kostenersparnis, Obergrenze 12 %). Es existieren Reviews über Reviews.

---

## 1 Wo die Lücke wirklich liegt

Nicht im Algorithmus, sondern in der **Inbetriebnahme**:

- Hohe Anforderungen an Hardware, Software und Know-how verhindern die Kommerzialisierung → regelbasierte Regler bleiben Stand der Technik
- Komplexe manuelle Konfiguration muss von Regelungsexperten durchgeführt werden → Kosten, keine Skalierung
- Fehlende adaptierbare und robuste Modelle über unterschiedliche Gebäudetypen hinweg

**Die offene Frage:** Wie bringt man MPC auf hunderttausende Bestandsanlagen, ohne pro Anlage ein Gebäudemodell von Hand zu identifizieren?

## 2 Der eigentlich interessante Winkel

> [!tip] Kern der Idee
> Praktisch jede MPC-Formulierung modelliert die WP als **COP-Kennfeld über der Außentemperatur**. Abtauverluste kommen darin nicht vor.

Dabei gilt:

- Reifbildung tritt in einem engen, aus dem **Wetterbericht vorhersagbaren** Fenster auf (ca. −5…+7 °C bei hoher r.F.)
- Ein Abtauzyklus kostet Wärme, Kompressorstarts, Komfort
- Ein prädiktiver Regler könnte die Gebäudemasse **vor** Eintritt in dieses Fenster vorladen und während der Reifphase gedrosselt fahren
- Der Verschmutzungszustand verschiebt diese Grenze → Kopplung an [[02 Verschmutzungserkennung WP]]

**Arbeitstitel:**
> Vereisungs- und degradationsbewusste prädiktive Regelung für Luft-Wasser-Wärmepumpen — Nutzung von Wetterprognosen zur Antizipation von Abtauzyklen und Kopplung an eine Online-Zustandsschätzung des Außenwärmeübertragers.

Damit ist es kein weiterer MPC-Beitrag, sondern ein prädiktiver Regler mit **selbstadaptierendem Anlagenmodell**.

## 3 Deutschlandspezifische Treiber

| Treiber | Bedeutung für die Regelung |
|---|---|
| §14a EnWG | Netzbetreiber darf WP drosseln → **stochastische Zwangseingriffe antizipieren**, Gebäudemasse vor wahrscheinlichem Dimmfenster vorladen |
| Dynamische Tarife | erstmals flächendeckendes Preissignal, an dem Optimierung sich lohnt |
| Smart-Meter-Rollout | Datenschnittstelle |

- [ ] Aktuellen Stand von §14a prüfen – Ausgestaltung ändert sich laufend

## 4 Methodischer Pflichtteil

> [!danger] Standardfehler
> Eigenes Simulationsgebäude bauen und gegen die eigene Heizkurve vergleichen. Das ist ein Strohmann und wird sofort erkannt.

- [ ] **BOPTEST** als Benchmark verwenden – vordefinierte regelbasierte Regler und KPIs, WP-Testfall vorhanden, reproduzierbar
- [ ] Vergleich gegen regelbasierte Referenz, nicht gegen selbstgebaute Heizkurve
- [ ] Prognoseunsicherheit explizit propagieren (stochastische MPC)

## 5 RL ja oder nein?

| Aspekt | Bewertung |
|---|---|
| RL für Erkennung | **nein** – Klassifikationsproblem, kein sequentielles Entscheidungsproblem |
| RL für Entscheider | denkbar – POMDP, Belohnung = JAZ − Reinigungskosten − Abtauverluste |
| Erste Gutachterfrage | "Warum nicht MPC?" |
| Antwort nötig | MPC auf dem 1D-Modell ist zertifizierbar und erklärbar. RL nur rechtfertigen, wenn es MPC in einem konkreten Punkt schlägt (z. B. nicht modellierte Wetterstochastik) |

→ Als ehrliches Teilarbeitspaket, nie als Projektkern.

## 6 Konkurrenz

- **RWTH Aachen, EBC** – sehr aktiv: mehrzielige MPC für Luftwärmepumpen (Effizienz + Geräusch), datengetriebene MPC mit realen Felddaten
- **KIT, IAI** – verteilte MPC, experimentelle Demand-Response-Vergleiche

> [!note] Strategie
> Nicht frontal antreten. Diese Gruppen haben Prüfstände, Felddaten und zehn Jahre Vorlauf. Der **Vereisungs- und Degradationsaspekt** kommt aus der Kältetechnik, nicht aus der Regelungstechnik – dort ist die Nische frei.

## 7 Schnelltest

- [ ] BOPTEST WP-Testfall aufsetzen
- [ ] COP-Kennfeld um einfaches empirisches Abtaumodell erweitern (Wirkungsgradeinbruch im Reiffenster + periodische Abtauverluste)
- [ ] Einmal mit, einmal ohne Berücksichtigung im Optimierer rechnen
- [ ] Differenz auswerten

Wenn die frostbewusste Variante nennenswert besser abschneidet: Argument in einer Grafik. Wochen, keine Monate.
