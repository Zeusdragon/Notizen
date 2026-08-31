---
title: "📝 Prüfungsvorbereitung Kryotechnik"
type: notiz
status: offen
tags:
  - Kryotechnik
  - Prüfung
  - Fragenkatalog
erstellt: 2026-05-05
---

# 📝 Prüfungsvorbereitung Kryotechnik


---

## 1. Grundlagen & Fluide
+ Nennen Sie **5 kryogene Fluide**, deren Normalsiedetemperatur ($T_s$) und typische Anwendungen. [[01 Definition und Historie]] 
+ Zeichnen Sie das **$\log(p)$-$T$-Diagramm von $^4He$**.
    + Phasen benennen (He I, He II, Gas, Fest...).
    + Wichtige Punkte (Kritischer Punkt, Lambda-Punkt) mit Werten angeben.
+ **Siedelinse Luft**: Zeichnen Sie das Diagramm und zeigen Sie die Zusammensetzung von Luft bei Abkühlung (Rektifikation).
+ **Wasserstoff-Szenario**: Ein $H_2$-Tank ($T_u$, $p=700$ bar) wird über eine Drossel entspannt.
    + Welche Zustandsänderung liegt im Tank vor?
    + Welche Zustandsänderung liegt im ausströmenden Gas vor?
    + Wie entwickeln sich die Temperaturen (Tank vs. Gasstrahl)?

## 2. Thermodynamik & Kreisprozesse [[03 Kälteerzeugung]]
+ **Carnot-Prozess**:
    + Definition und Bedeutung.
	    + isenthalpe Drosselung und Verdichtung und isotherme Energiezufuhr und abgabe kann man runterrechnen in $\frac{T_u}{T_u-T_0}$ 
    + Berechnung der minimal erforderlichen Kühlleistung (COP).
+ Zeichnen Sie folgende Kreisläufe als **Fließbild** und im **$T-s$-Diagramm**:
    + Joule-Thomson-Prozess (Linde).
    + Brayton-Prozess.
    + Claude-Prozess.
+ **T-s-Diagramm $^4He$** (gegeben):
    + Inversionskurve einzeichnen.
    + Isentrope vs. isenthalpe Entspannung einzeichnen (Startpunkt und Enddruck gegeben).
+ **Exergie-Diagramm $H_2$** (gegeben): Bestimmen Sie graphisch:
    + Mindestenergieaufwand für vollständige Verflüssigung.
    + Energiebedarf für o-p-Umwandlung (Ortho-Para).
    + Einsparung bei Vorkühlung auf 80 K ($LN_2$-Schild).

## 3. Materialeigenschaften [[14 Materialeigenschaften]]
+ Zeichnen Sie das **$c_p$-$\vartheta_D$-Diagramm** (Debye-Temperatur).
+ Zeichnen Sie das **$c_p$-$T$-Diagramm** qualitativ für zwei verschiedene Materialien.
+ Zeichnen Sie das **$\rho_{el}$-$T$-Diagramm** (elektrischer Widerstand) für:
    + Metalle (reine Metalle vs. Legierungen).
    + Halbleiter.
    + Supraleiter.
+ Nennen Sie **2 Materialien**, die bei tiefer Temperatur aufgrund von Versprödung (Cold Embrittlement) **nicht** eingesetzt werden dürfen.

## 4. Wärmeübertragung & Isolation
+ Nennen Sie 3 unvermeidbare **Wärmeübertragungsmechanismen** bei einem Dewarbehälter und die jeweiligen **Gegenmaßnahmen**.
	1. Strahlung 
		1. Gegenmaßnahme: Spieglung
	2. Wärmeleitung
		1. Isoltationvakuum oder Isolation allgemein
	3. 
+ **Berechnungsaufgabe**: Wärmeleitung über ein Halsrohr.
    + Gegeben: Geometrie, Wärmeleitintegral.
    + Gesucht: Wärmestrom $\dot{Q}$.

## 5. Messtechnik & Sicherheit [[17 Sensorik]]
+ Beschreiben und zeichnen Sie die **4-Leiter-Messung**. Wann und warum wird diese angewendet?
+ Nennen Sie Messtechniken für die **Füllstandsmessung** in:
    + LHe-Tanks (Supraleitungsdraht).
    + $LH_2$-Tanks (Kapazitiv).
    + $LN_2$-Tanks (Differenzdruck).
+ Nennen Sie die **Zünd-/Deflagrations-** sowie **Explosionsgrenzen** von Wasserstoff.

## 6. Komponenten (Kühler & Pumpen) [[20 Cryocooler und KryoVakuumpumpen]]
+ **Cryocooler**: Nennen Sie 3 kommerzielle Arten (z.B. GM, Pulse Tube, Stirling), deren Merkmale und typische Anwendungen.
+ **Konstruktionsaufgabe (Design)**: Entwerfen Sie einen **$H_2$-Dewarbehälter für ein Nutzfahrzeug**. Skizzieren Sie inkl.:
    + Abstützung (Lagerung Innen- zu Außentank).
    + Messtechnik.
    + Entnahmeeinrichtungen.
    + Füllstandsmessung.
    + *Hinweis: Designprinzipien anwenden (minimale Wärmeleitung)!*

## 7. Supraleitung
+ Nennen Sie **3 Supraleitermaterialien** (LTS/HTS) und mit welchem Fluid sie gekühlt werden.
+ Erklären Sie den **Quench** und nennen Sie Schutzmaßnahmen.
+ Definieren Sie **Pinning-Zentren** (Flussverankerung). Wozu dienen sie?
+ Nennen und zeichnen Sie die **3 kritischen Parameter** eines Supraleiters ($T_c, J_c, B_{c2}$).
+ **Teilchenbeschleuniger**: Wozu benötigt man folgende Magnettypen?
    + Supraleitende Dipol-Magnete.
    + Supraleitende Quadrupol-Magnete.
    + Supraleitende Dekapol/Sextupol-Magnete.
+ **SQUID**: Kurze Erklärung und Anwendung.

---

> [!DANGER] Master-Konstruktionsaufgabe (Übung 15)
> **Designaufgabe Magnet-Kryostat**
> Hier wird eine ausführliche Konstruktionszeichnung verlangt (ähnlich der letzten Übung).
> * LHe-Behälter
> * $LN_2$-Abschirmung (Strahlungsschild)
> * Sicherheitsventile / Berstscheiben
> * Thermische Abstützung
> * Stromzuführungen (!)
>
> *Tipp: Üben Sie das Zeichnen der Komponenten aus dem Gedächtnis!*