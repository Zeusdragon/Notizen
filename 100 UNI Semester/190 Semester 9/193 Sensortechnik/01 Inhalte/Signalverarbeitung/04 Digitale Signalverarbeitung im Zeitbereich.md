# 4.1 Interpolation: Polynome

**Tags:** #Signalverarbeitung #Interpolation #Polynome
**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 4.1)

---

## Ziel
[cite_start]Verbinden von $N$ Datenpunkten $\{t_i, y_i\}$ durch eine stetige Funktion, z.B. zur Integration oder um Lücken zu füllen[cite: 116].

## Polynom-Methode
Ein Polynom $(N-1)$-ter Ordnung wird durch die Punkte gelegt:
$$y(t) = a_0 + a_1 t + a_2 t^2 + \dots$$
[cite_start]Die Koeffizienten $a_i$ werden durch ein lineares Gleichungssystem gelöst (Vandermonde-Matrix)[cite: 124].

> [!WARNING] Problem
> Polynome hohen Grades neigen zu starken **Oszillationen** zwischen den Stützstellen (siehe Runge-Phänomen). [cite_start]Die Verläufe sind oft physikalisch nicht sinnvoll[cite: 132].

---
# 4.2 Interpolation: Splines

**Tags:** #Signalverarbeitung #Interpolation #Splines
**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 4.2)

---

## Kubische Splines
[cite_start]Anstatt eines einzigen Polynoms hohen Grades werden stückweise Polynome 3. Grades zwischen je zwei Punkten verwendet[cite: 134].

**Bedingungen:**
1. Das Polynom trifft die Datenpunkte.
2. An den Stoßstellen (Übergang zwischen zwei Intervallen) sind die **1. Ableitung** und die **2. [cite_start]Ableitung** stetig (identisch)[cite: 135].

## Berechnung
Der Ansatz für das Intervall $i$:
$$s_i(t) = a_i + b_i t + c_i t^2 + d_i t^3$$
[cite_start]Dies führt auf ein Gleichungssystem für die zweiten Ableitungen, das sich effizient lösen lässt (diagonaldominante Matrix)[cite: 156].

**Randbedingungen (für Anfang und Ende):**
* **Natürlicher Spline:** 2. Ableitung an den Rändern ist 0.
* **Zyklischer Spline:** 2. Ableitung am Anfang = 2. Ableitung am Ende.
* [cite_start]**Not-a-knot:** Extrapolation durch Polynome an den Rändern[cite: 152].
---
# 4.3 Ausgleichsfunktionen

**Tags:** #Signalverarbeitung #Regression #Fitting
**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 4.3)

---

## Ziel
Datenreduktion und Trendermittlung. [cite_start]Im Gegensatz zur Interpolation muss die Kurve nicht *durch* die Punkte gehen, sondern den Fehler minimieren ("Fitting")[cite: 162].

## Methode der kleinsten Fehlerquadrate
Gesucht sind Koeffizienten $a_m$ für ein Polynom $p(t)$, sodass die Summe der quadratischen Abweichungen minimal wird:
$$\sum_{i=0}^{N-1} [y_i - p(t_i)]^2 \rightarrow \text{Min}$$
[cite_start]Die Lösung erfolgt durch Nullsetzen der partiellen Ableitungen nach den Koeffizienten, was zu einem linearen Gleichungssystem führt[cite: 175].

---
# 4.4 Numerisches Differenzieren

**Tags:** #Signalverarbeitung #Differentiation
**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 4.4)

---

## Differenzenquotienten
Ersetzung des Differentialquotienten durch diskrete Differenzen.

1. [cite_start]**Linksseitig:** $\frac{f(t_i) - f(t_{i-1})}{h}$ [cite: 179]
2. **Rechtsseitig:** $\frac{f(t_{i+1}) - f(t_i)}{h}$
3. **Beidseitig (Zentral):** $\frac{f(t_{i+1}) - f(t_{i-1})}{2h}$

> [!NOTE] Rauschen
> Einfache Differenzenquotienten verstärken Rauschen extrem. [cite_start]Besser ist es, zuerst einen Spline oder ein Ausgleichspolynom zu berechnen und dieses analytisch abzuleiten[cite: 180].

---
# 4.5 Numerisches Integrieren

**Tags:** #Signalverarbeitung #Integration
**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 4.5)

---

## Verfahren
Berechnung der Fläche unter dem Signal.

1. **Rechteckregel:**
   $$\int f(t) dt \approx \sum y_i h$$
2. **Trapezregel:** (Genauer als Rechteck)
   $$\int f(t) dt \approx h \left( \frac{1}{2}(y_0 + y_N) + \sum_{i=1}^{N-1} y_i \right)$$
3. **Keplersche Fassregel (Simpson-Regel):**
   Approximation durch Parabeln (Polynome 2. Ordnung) über je 3 Punkte.
   $$\int f(t) dt \approx \frac{1}{3}h (y_0 + 4y_1 + 2y_2 + \dots + y_N)$$
   (Gilt für ungerade Anzahl von Punkten) [cite_start][cite: 187].
---
# 4.6 Mittelungsverfahren: Arithmetische Mittelung

**Tags:** #Signalverarbeitung #Mittelwert #Filter
**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 4.6)

---

## Ungewichtete Mittelung
[cite_start]Integration des Signals über einen Bereich $t \pm \Delta t$ und Division durch die Zeitdauer[cite: 190].
Bei diskreten Signalen entspricht dies dem **Gleitenden Mittelwert** (Moving Average).

## Gewichtete Mittelung
Messwerte, die weiter vom Zentrum entfernt sind, erhalten weniger Gewicht.
$$m_j = \sum_{i=-N}^{N} a_{|i|} y_{j+i} \quad \text{mit} \quad \sum a_i = 1$$
[cite_start]Dies entspricht einer **Faltung** des Signals mit einer Gewichtsfunktion (Filter)[cite: 194].

**Randproblem:**
Am Anfang und Ende der Daten fehlen Werte für das Filterfenster. Lösungen:
* **Zero-Padding:** Auffüllen mit Nullen (oft unphysikalisch).
* [cite_start]**Extrapolation:** Fortsetzen des ersten/letzten Wertes oder lineare Extrapolation[cite: 199].
---
# 4.7 Mittelungsverfahren: Zentralwertbildung

**Tags:** #Signalverarbeitung #Median #Filter
**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 4.7)

---

## Median-Filter
Anstatt den Durchschnitt zu berechnen, werden die Werte im Fenster der Größe nach **sortiert**. [cite_start]Der neue Wert ist der **Zentralwert** (Median) der sortierten Folge[cite: 207].

**Vorteile:**
* **Robust gegen Ausreißer:** Einzelne extreme Fehlmessungen verfälschen das Ergebnis nicht (im Gegensatz zum arithmetischen Mittel).
* [cite_start]Kanten im Signal bleiben besser erhalten (weniger Verschleifung)[cite: 211].
