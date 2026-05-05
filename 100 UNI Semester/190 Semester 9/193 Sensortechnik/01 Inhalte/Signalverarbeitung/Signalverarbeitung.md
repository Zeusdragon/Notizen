# Signalverarbeitung (Master-Note)

**Vorlesung**: [[Sensorik]] / Signalverarbeitung
**Quelle**: [[Signalverarbeitung Skript.pdf]]
**Tags**: #Signalverarbeitung #MOC #Master

---

## Übersicht
Diese Vorlesung behandelt die mathematischen Methoden zur Analyse und Verarbeitung von Signalen. Der Fokus liegt auf der Digitalisierung, Filterung im Zeitbereich, statistischen Analyse und der Transformation in den Frequenzbereich.

---

## 📂 Kapitel-Struktur

### [[03 Grundlagen|Kapitel 3 - Grundlagen der Signalverarbeitung]]
* **3.1 Signale**: Definition, Analog vs. Digital, Zeitkontinuierlich vs. Zeitdiskret.
* **3.2 Grundsignalformen**: Rechteck, Gauß, Harmonische, Dirac-Stoß, Sprungfunktion.
* **3.3 Information**: Informationsgehalt, Shannon-Information.
* **3.4 LTI-Systeme**: Linearität und Zeitinvarianz.
* **3.5 Digitalisierung**: ADC-Aufbau, Aliasing-Filter, Sample & Hold, Wandler-Typen (SAR, Sigma-Delta).

### [[04 Digitale Signalverarbeitung im Zeitbereich|Kapitel 4 - Digitale Verarbeitung im Zeitbereich]]
* **Interpolation**: Polynome, Splines (Kubisch).
* **Approximation**: Ausgleichsrechnung (Methode der kleinsten Quadrate).
* **Numerik**: Differenzieren und Integrieren (Trapezregel, Simpson-Regel).
* **Filterung (Glättung)**:
    * Arithmetische Mittelung (Gleitender Durchschnitt).
    * Median-Filter (Robust gegen Ausreißer).

### [[05 Strategische Signalanalyse|Kapitel 5 - Statistische Signalanalyse]]
* **Kenngrößen**: Mittelwert, Varianz, Standardabweichung, Momente.
* **Verteilungen**: Normalverteilung, Poisson, Gleichverteilung.
* **Korrelation**:
    * **Autokorrelation (ACF)**: Periodizitäten finden.
    * **Kreuzkorrelation (CCF)**: Ähnlichkeiten/Verschiebungen zwischen zwei Signalen.
* **Regression**: Lineare Zusammenhänge ($R^2$).

### [[06 Signalverarbeitung im Frequnzbereich|Kapitel 6 - Signalverarbeitung im Frequenzbereic]]
* **Fourier-Reihen**: Zerlegung periodischer Signale.
* **Fourier-Transformation (FT)**: Spektrum nicht-periodischer Signale.
* **Leistungsspektrum**: Signalleistung in dB.
* **Faltung**: Faltungstheorem ($Zeitbereich * \leftrightarrow Frequenzbereich \cdot$).
* **Abtastung & Aliasing**: Nyquist-Theorem, Leck-Effekt (Leakage), Fensterfunktionen (Hanning, Hamming).

---

## 📝 Wichtige Konzepte & Formeln

> [!IMPORTANT] Nyquist-Shannon-Abtasttheorem
> $f_{Abtast} > 2 \cdot f_{max}$
> Um Aliasing zu vermeiden, muss die Abtastfrequenz mehr als doppelt so hoch sein wie die höchste Signalfrequenz.

> [!TIP] Faltung
> Die Systemantwort $g(t)$ eines LTI-Systems ist die Faltung des Eingangs $f(t)$ mit der Impulsantwort $h(t)$.
> $$g(t) = f(t) * h(t)$$
> Im Frequenzbereich entspricht dies einer einfachen Multiplikation: $G(\omega) = F(\omega) \cdot H(\omega)$.

---