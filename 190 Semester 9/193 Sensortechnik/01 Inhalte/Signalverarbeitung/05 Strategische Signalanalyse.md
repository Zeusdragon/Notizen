# 5.1 Verteilungen und Kenngrößen

**Tags:** #Signalverarbeitung #Statistik
**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 5.1)

---

## Häufigkeitsverteilung
Gibt an, wie oft ein Merkmal in einer Stichprobe vorkommt.
* **Histogramm:** Übliche Darstellung.
* **Relative Häufigkeit:** Absolute Häufigkeit / $N$.
* [cite_start]**Kumulative Häufigkeit:** Summe der Häufigkeiten bis zu einem Wert (Integralfunktion)[cite: 226].

## Gesetz der großen Zahlen
[cite_start]Für $N \to \infty$ geht die relative Häufigkeitsverteilung in die **Wahrscheinlichkeitsdichtefunktion** $w(x)$ über[cite: 231].

## Wichtige Kenngrößen
* **Mittelwert:** $\bar{x} = \frac{1}{N} \sum x_i$
* **Varianz:** $s^2 = \frac{1}{N-1} \sum (x_i - \bar{x})^2$
* **Standardabweichung:** $s = \sqrt{s^2}$
* **Median:** Zentraler Wert der sortierten Folge.
* **Modalwert:** Häufigster Wert.
* [cite_start]**Quantil:** Wert $x_p$, für den $p \cdot 100 \%$ der Werte kleiner sind[cite: 242].
---
# 5.2 Wichtige Wahrscheinlichkeitsverteilungen

**Tags:** #Signalverarbeitung #Statistik #Verteilungen
**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 5.2)

---

| Verteilung           | Beschreibung                                                              | Formel (Dichte)                                                      |
| :------------------- | :------------------------------------------------------------------------ | :------------------------------------------------------------------- |
| **Gleichverteilung** | Jeder Wert im Intervall gleich wahrscheinlich.                            | $w(x) = \text{const}$                                                |
| **Binomial**         | Wahrscheinlichkeit für $M$ Treffer bei $N$ Versuchen (binär).             | $\binom{N}{M} p^M (1-p)^{N-M}$                                       |
| **Exponential**      | Zeit bis zum Eintritt eines Ereignisses (z.B. Zerfall).                   | $\lambda e^{-\lambda x}$                                             |
| **Poisson**          | Anzahl zufälliger Ereignisse in einem Zeitfenster (seltene Ereignisse).   | $\frac{\alpha^n}{n!} e^{-\alpha}$                                    |
| **Normal (Gauß)**    | Summe vieler unabhängiger Einflussgrößen (z.B. Rauschen).                 | $\frac{1}{\sqrt{2\pi\sigma^2}} e^{-\frac{(x-\bar{x})^2}{2\sigma^2}}$ |
| **Log-Normal**       | Wenn Werte multiplikativ zusammenwirken oder logarithmisch skaliert sind. | -                                                                    |

---
# 5.3 Auto- und Kreuzkorrelation

**Tags:** #Signalverarbeitung #Korrelation
**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 5.3)

---

## Kreuzkorrelation (CCF)
Maß für die Ähnlichkeit zweier Signale $f(t)$ und $g(t)$ in Abhängigkeit einer Zeitverschiebung $\tau$.
$$r_{fg}(\tau) = \int f(t) g(t+\tau) dt$$
[cite_start]**Anwendung:** Finden von Signalmustern (z.B. Echos, Laufzeiten von Ultraschallpulsen in verrauschten Signalen)[cite: 275].

## Autokorrelation (ACF)
Ähnlichkeit eines Signals mit sich selbst bei Verschiebung.
$$r_{ff}(\tau) = \int f(t) f(t+\tau) dt$$
[cite_start]Gibt Auskunft über Periodizitäten im Signal[cite: 276].

---
# 5.4 Lineare Regression

**Tags:** #Signalverarbeitung #Regression
**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 5.4)

---

## Ziel
Ermittlung eines funktionalen Zusammenhangs (Modell) zwischen Datenpunkten. [cite_start]Bei der linearen Regression wird eine Gerade $y = ax + b$ gesucht[cite: 294].

## Minimierung der Residuen
Parameter $a$ und $b$ werden so bestimmt, dass die Summe der quadratischen Abweichungen minimal ist:
$$\sum (y_i - (ax_i + b))^2 \rightarrow \text{Min}$$

## Bestimmtheitsmaß $R^2$
Gütemaß der Regression. Verhältnis der Streuung der Modellwerte zur Streuung der Messwerte. [cite_start]$R^2 \approx 1$ bedeutet sehr gute Anpassung[cite: 297].
