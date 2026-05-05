# 3.1 Signale

**Tags:** #Signalverarbeitung #Grundlagen #Signale
**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 3.1)

---

## Definition
Ein **Signal** ist der Verlauf einer informationstragenden Größe über der Zeit. [cite_start]In der Signalverarbeitung wird dies abstrahiert zu einer eindimensionalen, kontinuierlichen, einheitenlosen Funktion $f(t)$[cite: 23].

**Beispiele:**
* Lokale Temperatur $\vartheta(t)$
* Absolutdruck $p(t)$
* Schalldruckamplitude $\Delta p(t)$

## Signalarten
Unterscheidung nach Wertebereich und Definitionsbereich (Zeit):

1. [cite_start]**Analoges Signal:** Kontinuierlicher Wertebereich (jeder Wert innerhalb der Grenzen zulässig)[cite: 26].
2. [cite_start]**Digitales Signal:** Diskreter Wertebereich (endliche Anzahl an Werten, z.B. 0 oder 1 bei Binärsignalen)[cite: 28].
3. [cite_start]**Zeitkontinuierliches Signal:** Zu jedem beliebigen Zeitpunkt definiert[cite: 32].
4. [cite_start]**Zeitdiskretes Signal:** Nur zu bestimmten, abzählbaren Zeitpunkten definiert (abgetastet)[cite: 34].

### Kombinationen
* [cite_start]**Zeitkontinuierlich analog:** Klassische Analogsignalverarbeitung (Modellierung)[cite: 38].
* [cite_start]**Zeitdiskret analog:** Theoretische Behandlung abgetasteter Signale[cite: 40].
* [cite_start]**Zeitdiskret digital:** Digitale Signalverarbeitung (Softwarealgorithmen)[cite: 42].
* [cite_start]**Zeitkontinuierlich digital:** Digitale Elektronik (Logikschaltungen)[cite: 43].

> [!INFO] Endlich vs. Unendlich
> Ein Signal heißt **unendlich**, wenn sein Definitionsbereich mindestens positiv unendlich ist. [cite_start]Praktische Signale sind immer **endlich** (zeitbegrenzt)[cite: 36].

---
# 3.2 Wichtige Grundsignalformen

**Tags:** #Signalverarbeitung #Grundlagen #Signalformen
**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 3.2)

---

## Übersicht
[cite_start]Für die mathematische Behandlung sind folgende Grundformen essenziell[cite: 49]:

### 1. Rechteck-Puls
Ein Signal mit einer gegebenen zeitlichen Ausdehnung $a$.
$$\text{rect}(a;t) = \begin{cases} 1 & \text{für } |t| < a/2 \\ 0 & \text{sonst} \end{cases}$$

### 2. Gauß-Puls
Basiert auf der Normalverteilung.
$$f(t) = \frac{1}{\sqrt{2\pi\sigma^2}} e^{-\frac{t^2}{2\sigma^2}}$$

### 3. Harmonische
Grundform ist das Cosinus-Signal (symmetrisch zu $t=0$).
$$f(t) = \cos(\omega t) = \cos\left(\frac{2\pi}{T}t\right)$$

### 4. Dirac-Stoß (Dirac-Distribution)
Grenzfall eines Signals, das nur zu einem Zeitpunkt wirkt (normierte Wirkungsstärke).
[cite_start]Mathematisch als Grenzwert eines Rechteckpulses, dessen Breite gegen 0 geht bei Fläche 1[cite: 56]:
$$\delta(t) = \lim_{a \to 0} \left( \frac{1}{a} \text{rect}(t;a) \right)$$

### 5. Sprungsignal
[cite_start]Entsteht durch Integration der Dirac-Distribution[cite: 57]:
$$s(t) = \int_{-\infty}^{t} \delta(t') dt' = \begin{cases} 1 & \text{für } t > 0 \\ 0 & \text{sonst} \end{cases}$$

--- 
# 3.3 Signal und Information

**Tags:** #Signalverarbeitung #Informationstheorie
**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 3.3)

---

## Informationsgehalt
[cite_start]Information wird definiert als **beseitigte Ungewissheit**[cite: 60].

Wenn ein digitales Signal $N$ Werte annehmen kann und alle Werte gleich wahrscheinlich sind, beträgt der Informationsgehalt eines einzelnen Wertes:
$$I(x_i) = \log_2 N \quad [\text{Bit}]$$

**Beispiel:**
Ein Signal kann 8 diskrete Werte annehmen.
$I = \log_2 8 = 3$ Bit. [cite_start]Man muss 3 Ja/Nein-Fragen stellen, um den Wert zu ermitteln[cite: 62].

> [!NOTE] Shannon-Information
> [cite_start]Wenn Werte unterschiedliche Auftrittswahrscheinlichkeiten haben, sinkt der Informationsgehalt für häufige Werte (man weiß schon "eher", was kommt)[cite: 69].

---
# 3.4 Signalverarbeitendes System

**Tags:** #Signalverarbeitung #LTI #Systemtheorie
**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 3.4)

---

## Definition
[cite_start]Ein System transformiert ein Eingangssignal $f(t)$ in ein Ausgangssignal (Systemantwort) $g(t)$[cite: 73].
$$g(t) = \mathcal{H}\{f(t)\}$$

## Lineares Zeitinvariantes System (LTI)
[cite_start]Ein System ist ein **LTI-System**, wenn zwei Bedingungen erfüllt sind[cite: 80]:

1. **Linearität:**
   $$\mathcal{H}\{a f(t) + b g(t)\} = a \mathcal{H}\{f(t)\} + b \mathcal{H}\{g(t)\}$$
2. **Zeitinvarianz:**
   Die Systemantwort hängt nicht vom Zeitpunkt ab (Verschiebung im Eingang führt zu gleicher Verschiebung im Ausgang).
   $$\mathcal{H}\{f(t - t')\} = g(t - t')$$
---
# 3.5 Technische Signaldigitalisierung

**Tags:** #Signalverarbeitung #ADC #Digitalisierung
**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 3.5)

---

## Aufbau eines ADC
[cite_start]Ein Analog-Digital-Converter (ADC) besteht meist aus folgenden Stufen[cite: 85]:
1. **Anti-Aliasing-Filter:** Bandbegrenzung des Signals.
2. **Abtast- und Halteglied (Sample-and-Hold):** Hält die Spannung konstant, während gewandelt wird (Kondensator lädt auf, Schalter öffnet).
3. **Umsetzer:** Führt die eigentliche Diskretisierung durch.

## Umsetzer-Prinzipien

### 1. Integrierender Umsetzer (Sägezahn)
Vergleich der Eingangsspannung mit einer linear ansteigenden Sägezahnspannung. [cite_start]Die Zeit bis zur Gleichheit wird durch Zählen von Oszillator-Pulsen gemessen[cite: 93].

### 2. Sukzessive Approximation (SAR)
Vergleichsspannung wird binär angenähert ("Wägeverfahren").
* Setze höchstes Bit (MSB). Ist $U_{Vergleich} > U_{Eingang}$? Wenn ja, Bit löschen, sonst behalten.
* Wiederhole für alle Bits bis LSB.
* [cite_start]Schnell: Ein $n$-Bit Umsetzer benötigt nur $n$ Schritte[cite: 106].

### 3. Sigma-Delta-Wandler
Verwendung von Überabtastung und Bitstrommodulation.
* Ein Integrator summiert die Differenz aus Eingang und Feedback.
* [cite_start]Hohe Auflösung möglich, aber durch Überabtastung meist langsamer[cite: 119].
* [cite_start]**Noise Shaping:** Rauschen wird in höhere Frequenzen verschoben[cite: 117].