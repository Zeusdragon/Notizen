**Vorlesung**: [[191 Kryotechnik]]
**Datum**: 2026-01-06
**Topics**: [[Stoffdaten]], [[Sieden]], [[Kapitza-Widerstand]], [[Real-Gas-Faktor]]

---

# 12 Fluiddaten und Wärmeübergang
![[12 Fluiddaten_Wärmeübergang.pdf]]

> [!INFO] Zusammenfassung
> Verhalten kryogener Fluide (Zustandsgleichungen) und die Besonderheiten des Wärmeübergangs bei tiefen Temperaturen (Siedekurven, Filmsieden).

## 1. Thermodynamische Grundlagen
### Zustandsdiagramme
* **p,v-Diagramm**: Isothermenverlauf.
    * Kritischer Punkt (Wendepunkt der Isotherme).
    * Unterscheidung: Nassdampfgebiet, überkritischer Bereich, Sublimationsgebiet.
* **Real-Gas-Faktor $Z$**:
    Korrektur der idealen Gasgleichung ($p \cdot v = R \cdot T$) für reale Gase, besonders bei hohen Drücken/tiefen Temperaturen.
    $$Z(p,T) = \frac{p \cdot v}{R \cdot T}$$
    * Für Helium bei 200 bar / 15 °C: $Z \approx 1,09$. $\Rightarrow$  9% weniger Inhalt als ideal Gas veraussagt

### Zustände

|                       | $LHe$    | $LH_2$   | $LN_2$   |
| --------------------- | -------- | -------- | -------- |
| Verdampfungsenthalpie | 2,55 kJ  | 32 kJ    | 162 kJ   |
| äquivalente Gasmenge  | 751 l    | 842 l    | 691 l    |
| cp                    | ~660 J/K | ~900 J/K | ~850 J/K |
| Q0                    | ~190 kJ  | ~230 kJ  | ~184 kJ  |


## 2. Wärmeübergang an siedende Flüssigkeiten
Der Wärmeübergang von einer Festkörperwand an ein kryogenes Bad (z.B. LHe) ist stark nichtlinear (**Nukiyama-Kurve**).

$$\dot{q} = \alpha \cdot \Delta T$$

### Die Regimes der Siedekurve (log-log Auftragung $\dot{q}$ über $\Delta T$)
von kleinen $\Delta T$ bis höheren $\Delta T$
1.  **Freie Konvektion**: Flüssigkeit erwärmt sich, steigt auf (keine Blasen).
2.  **Blasensieden (Nucleate Boiling)**:
    * Bildung von Dampfblasen an Keimstellen 
	    * Keimstellen entstehen bei Kerben und Kratzen besser.
    * Sehr guter Wärmeübergang ($\alpha$ hoch).
    * Anstieg: $\dot{q} \propto \Delta T^{2,5}$.
3.  **Kritischer Wärmestrom (Burn-out / Peak Flux)**:
    * Maximale Wärmestromdichte, bei der die Oberfläche noch benetzt ist.
    * Bei LHe (4,2 K): ca. **$0,8 \dots 1 \, W/cm^2$**.
4.  **Filmsieden (Film Boiling)**:
    * Ein isolierender Dampffilm trennt Heizfläche und Flüssigkeit (Leidenfrost-Effekt).
    * Drastischer Abfall des Wärmeübergangs $\to$ Temperatur der Wand springt schlagartig an ("Burn-out").

> [!WARNING] Hysterese
> Beim Zurückfahren der Leistung bleibt der isolierende Gasfilm lange stabil. Man muss die Leistung sehr weit reduzieren, um vom Filmsieden wieder ins Blasensieden "zurückzuspringen" (Recovery).


## 3. Kapitza-Widerstand 
>[!WARNING] Dies ist nicht besonders relevant

Ein spezielles Phänomen an der Grenzfläche zwischen Festkörper (z.B. Kupfer) und **suprafluidem Helium (He-II)**.
* Obwohl He-II extrem gut wärmeleitend ist, entsteht an der Grenzfläche ein Temperatursprung.
* Ursache: "Akustische Missmatch" (Reflexion von Phononen an der Grenzfläche).
* Relevant bei $T < 2 \, K$.