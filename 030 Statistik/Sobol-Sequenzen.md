---
title: "Sobol-Sequenzen"
type: konzept
status: fertig
tags:
  - statistik
  - sampling
  - sensitivity-analysis
  - doe
erstellt: 2026-08-11
---

# Sobol-Sequenzen

**Bezug:** [[DoE Grundlagen]] · [[Gaussian Process]] · [[200 Diplom/Arbeitsstand]]

---

## 1. Was sind Sobol-Sequenzen?

Eine Sobol-Sequenz ist eine **quasi-zufällige, deterministische** Punktfolge, die einen Parameterraum (z.B. den Würfel $[0,1]^d$) so gleichmäßig wie möglich auffüllt — im Gegensatz zu "echtem" Zufall (Pseudo-Random), der immer klumpt und Lücken lässt.

* **Kein Zufall, sondern Konstruktion:** Die Punkte werden nicht gewürfelt, sondern über eine feste mathematische Vorschrift erzeugt (daher reproduzierbar, kein Seed im klassischen Sinn nötig — nur ein Startindex/Skip).
* **Low-Discrepancy:** "Discrepancy" misst, wie ungleichmäßig eine Punktmenge einen Raum abdeckt. Sobol-Sequenzen minimieren diese Größe gezielt.
* **Space-Filling:** Für dieselbe Anzahl Punkte $N$ ist die Abdeckung des Raums deutlich gleichmäßiger als bei zufälligem Sampling — wichtig, wenn jeder Punkt eine teure Simulation kostet (siehe [[200 Diplom/Geometrische Parameter]]).

> **Kernidee:** Man will mit möglichst wenigen Simulationspunkten möglichst viel über den gesamten Parameterraum wissen. Sobol-Sequenzen sind dafür effizienter als reines Zufalls-Sampling oder ein grobes Raster.

## 2. Wie funktionieren sie (Grundprinzip)?

* Basis ist die **van-der-Corput-Folge**: eine 1D-Folge, die das Intervall $[0,1]$ durch Bit-Umkehrung einer Zählvariable sukzessive halbiert und die entstehenden Lücken auffüllt (0, 1/2, 1/4, 3/4, 1/8, 5/8, …).
* Sobol erweitert das auf $d$ Dimensionen, wobei jede Dimension eine eigene, über sogenannte **Direction Numbers** und Gray-Code-Arithmetik erzeugte Bit-Folge bekommt (vollständig durchgerechnet in Abschnitt 3) — so bleiben die Dimensionen näherungsweise unkorreliert zueinander, statt einfach dieselbe 1D-Folge zu kopieren.
* **Progressive Eigenschaft:** Jede Verdopplung der Punktzahl ($N \to 2N$) füllt exakt die verbliebenen Lücken der vorherigen Menge auf. Man kann also jederzeit mehr Punkte nachziehen, ohne die bisherigen zu verwerfen — praktisch, wenn man die Anzahl Simulationen im Nachhinein erhöhen will.
* In der Praxis nutzt man meist $N = 2^m$ Punkte (Zweierpotenzen), da die Gleichmäßigkeits-Eigenschaften dafür am besten bewiesen sind. Moderne Implementierungen (z.B. `scipy`) nutzen zusätzlich **Owen-Scrambling**, um die Sequenz leicht zu randomisieren und damit auch Fehlerabschätzungen (Konfidenzintervalle) zu ermöglichen.

## 3. Beispiel von Hand: Kuchen backen

Damit die Begriffe *Direction Numbers*, *Bit-Umkehr* und *XOR* nicht abstrakt bleiben, hier ein vollständig durchgerechnetes Beispiel mit **zwei Parametern**:

| Parameter | Symbol | Bereich |
| --- | --- | --- |
| Backtemperatur | $T$ | 150 … 250 °C |
| Backdauer | $t$ | 20 … 60 min |

Gesucht sind $N = 2^3 = 8$ Versuchskombinationen, die den Raum „Temperatur × Dauer" möglichst gleichmäßig abdecken.

### 3.1 Schritt 1: Direction Numbers bestimmen

Jede Dimension bekommt ihre **eigene** Liste von Direction Numbers $v_1, v_2, v_3, \dots$ Das sind Binärbrüche der Form

$$v_k = \frac{m_k}{2^k}, \qquad m_k \ \text{ungerade}, \qquad 0 < m_k < 2^k$$

Das $m_k$ ist also einfach ein Zähler; das $2^k$ im Nenner sorgt dafür, dass $v_k$ genau $k$ Nachkommastellen im Binärsystem hat. Woher die $m_k$ kommen, hängt an einem **primitiven Polynom** über $\mathbb{F}_2$, das je Dimension fest vergeben ist.

**Dimension 1 (Temperatur)** — hier wird das triviale Polynom benutzt, alle $m_k = 1$:

| $k$ | $m_k$ | $v_k$ binär | $v_k$ dezimal |
| --- | --- | --- | --- |
| 1 | 1 | 0,1 | 1/2 |
| 2 | 1 | 0,01 | 1/4 |
| 3 | 1 | 0,001 | 1/8 |

Das ist genau die van-der-Corput-Folge — Bit-Umkehr, sonst nichts.

**Dimension 2 (Dauer)** — hier ist das primitive Polynom $x + 1$ (Grad $s = 1$) hinterlegt. Daraus folgt die Rekurrenz

$$m_k = 2\,m_{k-1} \oplus m_{k-1}, \qquad m_1 = 1$$

($\oplus$ = XOR, also bitweise Addition ohne Übertrag.) Von Hand:

```
m_1 = 1

m_2 = 2·1 XOR 1  =    10          m_3 = 2·3 XOR 3  =   110
                 XOR  01                           XOR 011
                 ------                            -------
                     11  = 3                          101  = 5

m_4 = 2·5 XOR 5  =  1010
                 XOR 0101
                 --------
                     1111 = 15
```

| $k$ | $m_k$ | $v_k$ binär | $v_k$ dezimal |
| --- | --- | --- | --- |
| 1 | 1 | 0,1 | 1/2 |
| 2 | 3 | 0,11 | 3/4 |
| 3 | 5 | 0,101 | 5/8 |
| (4) | 15 | 0,1111 | 15/16 |

> [!note] Das ist der ganze Trick
> Die beiden Dimensionen bekommen **unterschiedliche** Direction Numbers (`0,1 / 0,01 / 0,001` gegen `0,1 / 0,11 / 0,101`). Genau deshalb laufen die beiden Achsen nicht synchron, sondern näherungsweise unkorreliert. Würde man in beiden Dimensionen dieselbe van-der-Corput-Folge nehmen, lägen alle Punkte auf der Diagonalen.

### 3.2 Schritt 2: Punkt $i$ aus den Bits von $i$

Für den Punkt mit Index $i$ schreibt man $i$ binär, $i = b_3b_2b_1$, und verknüpft die Direction Numbers derjenigen Stellen, an denen eine 1 steht:

$$x_i = b_1 v_1 \oplus b_2 v_2 \oplus b_3 v_3$$

XOR wird dabei **stellenweise auf den Nachkommastellen** ausgeführt — kein Übertrag, keine normale Addition.

**Beispiel $i = 6$:**  $6 = 110_2$, also $b_3 = 1$, $b_2 = 1$, $b_1 = 0$ → benötigt werden $v_2$ und $v_3$.

```
Temperatur (Dim 1)            Dauer (Dim 2)
   v_2 = 0,010                   v_2 = 0,110
   v_3 = 0,001                   v_3 = 0,101
   ------------ XOR              ------------ XOR
   x_1 = 0,011                   x_2 = 0,011
```

Binär → dezimal, Stelle für Stelle:

$$0{,}011_2 = 0\cdot\tfrac{1}{2} + 1\cdot\tfrac{1}{4} + 1\cdot\tfrac{1}{8} = \tfrac{3}{8} = 0{,}375$$

### 3.3 Schritt 3: Auf echte Einheiten skalieren

Der Sobol-Punkt liegt immer in $[0,1]$. Die Umrechnung ist eine simple lineare Abbildung auf die Parametergrenzen:

$$T = T_{min} + (T_{max}-T_{min})\cdot x_1 = 150 + 100\,x_1$$
$$t = t_{min} + (t_{max}-t_{min})\cdot x_2 = 20 + 40\,x_2$$

Für $i = 6$:

$$T = 150 + 100 \cdot 0{,}375 = \mathbf{187{,}5\ °\text{C}} \qquad t = 20 + 40 \cdot 0{,}375 = \mathbf{35\ \text{min}}$$

### 3.4 Alle acht Versuche

| $i$ | $i$ binär | benutzte $v_k$ | $x_1$ binär | $x_1$ | **$T$ [°C]** | $x_2$ binär | $x_2$ | **$t$ [min]** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 000 | — | 0,000 | 0 | 150,0 | 0,000 | 0 | 20 |
| 1 | 001 | $v_1$ | 0,100 | 1/2 | 200,0 | 0,100 | 1/2 | 40 |
| 2 | 010 | $v_2$ | 0,010 | 1/4 | 175,0 | 0,110 | 3/4 | 50 |
| 3 | 011 | $v_1 \oplus v_2$ | 0,110 | 3/4 | 225,0 | 0,010 | 1/4 | 30 |
| 4 | 100 | $v_3$ | 0,001 | 1/8 | 162,5 | 0,101 | 5/8 | 45 |
| 5 | 101 | $v_1 \oplus v_3$ | 0,101 | 5/8 | 212,5 | 0,001 | 1/8 | 25 |
| 6 | 110 | $v_2 \oplus v_3$ | 0,011 | 3/8 | 187,5 | 0,011 | 3/8 | 35 |
| 7 | 111 | $v_1 \oplus v_2 \oplus v_3$ | 0,111 | 7/8 | 237,5 | 0,111 | 7/8 | 55 |

### 3.5 Was daran gleichmäßig ist

```
 t
60|  ·  ·  ·  ·  ·  ·  ·  ●7       Spalte = 12,5-K-Band
   │  ·  ·  ●2 ·  ·  ·  ·  ·        Zeile  = 5-min-Band
   │  ·  ●4 ·  ·  ·  ·  ·  ·
   │  ·  ·  ·  ·  ●1 ·  ·  ·
   │  ·  ·  ·  ●6 ·  ·  ·  ·
   │  ·  ·  ·  ·  ·  ·  ●3 ·
   │  ·  ·  ·  ·  ·  ●5 ·  ·
20|   ●0 ·  ·  ·  ·  ·  ·  ·
   └──────────────────────────
    150                    250  °C
```

Drei Eigenschaften lassen sich an der Tabelle direkt ablesen:

1. **Jede Achse ist perfekt geschichtet.** Die acht Temperaturen 150 / 162,5 / 175 / 187,5 / 200 / 212,5 / 225 / 237,5 °C treffen jedes der acht 12,5-K-Bänder genau einmal — dasselbe gilt für die acht 5-min-Bänder der Dauer. Kein Band ist doppelt besetzt, keines leer.
2. **Auch die Fläche ist geschichtet.** Teilt man den Raum in 2 × 4 oder 4 × 2 gleich große Zellen, liegt in jeder Zelle genau ein Punkt. Das ist die formale (0,3,2)-Netz-Eigenschaft — und der Unterschied zu reinem Zufall, der solche Zellen mal doppelt und mal gar nicht trifft.
3. **Progressiv.** Schon nach den ersten vier Punkten ($i = 0\ldots3$) ist jede Achse in Viertel zerlegt: $x_1 \in \{0, \tfrac12, \tfrac14, \tfrac34\}$. Die Punkte 4 bis 7 halbieren diese Viertel erneut. Man kann also jederzeit abbrechen oder verdoppeln, ohne das Bisherige zu verwerfen — genau die Eigenschaft, die in [[200 Diplom/Design of Experiment]] den Sprung von 64 auf 128 Punkte erlaubt.

> [!warning] Index 0 ist entartet
> $i = 0$ liefert immer die Ecke $(0,0)$, hier also 150 °C bei 20 min — den kältesten und kürzesten Versuch. Dieser Punkt ist kein „Sample", sondern ein Artefakt der Konstruktion. In der Praxis überspringt man ihn (`skip`/Startindex) oder benutzt Scrambling, das die Ecke auflöst.

### 3.6 Nachtrag: der Gray-Code-Trick

Die obige Rechnung XOR-t für jeden Punkt alle gesetzten Bits neu. Implementierungen nutzen stattdessen die Gray-Code-Reihenfolge und kommen mit **einem einzigen XOR je Punkt** aus:

$$x_i = x_{i-1} \oplus v_c, \qquad c = \text{Position der rechtesten } 0 \text{ in } (i-1)_2$$

Beispiel Dimension 2: $i = 2$ → $i-1 = 1 = \ldots0001$, die rechteste Null steht an Position 2 → $x_2 = x_1 \oplus v_2 = 0{,}100 \oplus 0{,}110 = 0{,}010 = 1/4$.

Dabei entsteht **dieselbe Punktmenge**, nur in anderer Reihenfolge:

| $i$ | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| $x_1$ | 0 | 1/2 | 3/4 | 1/4 | 3/8 | 7/8 | 5/8 | 1/8 |
| $x_2$ | 0 | 1/2 | 1/4 | 3/4 | 3/8 | 7/8 | 1/8 | 5/8 |

Das ist auch die Reihenfolge, die `scipy.stats.qmc.Sobol` ausgibt. Wer die Handrechnung aus 3.4 mit `scipy` vergleicht, findet also dieselben acht Kombinationen, aber in vertauschter Reihenfolge — kein Fehler, sondern nur eine andere Durchlaufordnung.

---

## 4. Warum nicht einfach Zufall oder ein Raster?

| Methode | Anzahl Punkte für $d$ Parameter | Problem |
|---|---|---|
| Full-Factorial-Raster | $k^d$ (bei $k$ Stufen je Parameter) | Explodiert exponentiell ("curse of dimensionality") |
| Pseudo-Zufall (Monte Carlo) | frei wählbar | Klumpt, braucht viele Punkte für gute Abdeckung, Konvergenz nur $O(1/\sqrt{N})$ |
| Sobol-Sequenz | frei wählbar | Gleichmäßige Abdeckung, Konvergenz für Integrale bis $O(1/N)$ möglich |
| One-Factor-at-a-Time (OFAT) | $\sum_i k_i$ | Erfasst keine Interaktionen zwischen Parametern |

Für teure, deterministische Simulationen (Dymola/FMU-Sweeps, kein Messrauschen) ist das entscheidend: Man kann sich mit gleicher Rechenzeit ein viel vollständigeres Bild des Parameterraums verschaffen.

## 5. Zwei konkrete Anwendungsfälle

### a) Als Sampling-/DoE-Methode
Sobol-Sequenzen werden verwendet, um Stichprobenpunkte für ein Design of Experiments zu erzeugen — z.B. um Kombinationen mehrerer Geometrieparameter (finPitch, finThickness, TubeLength, …) gleichzeitig und gleichmäßig über den zulässigen Bereich zu verteilen, statt nur einen Parameter nach dem anderen zu variieren (OFAT). Details dazu in [[DoE Grundlagen]].

### b) Für Sobol-Sensitivitätsanalyse (globale, varianzbasierte Sensitivität)
Hier werden Sobol-Sequenzen genutzt, um die Monte-Carlo-Integrale zu schätzen, mit denen man berechnet, **wie viel der Ausgangsvarianz** (z.B. Varianz im SCOP) **durch welchen Eingangsparameter** verursacht wird.

Varianzzerlegung (Sobol-Zerlegung) der Modellausgabe $Y = f(X_1, \dots, X_d)$:

$$\text{Var}(Y) = \sum_i V_i + \sum_{i<j} V_{ij} + \dots$$

Daraus werden zwei zentrale Kennzahlen berechnet:

* **First-Order-Index** $S_i = V_i / \text{Var}(Y)$ — wie viel Varianz erklärt Parameter $i$ **allein**.
* **Total-Order-Index** $S_{T_i} = 1 - V_{\sim i}/\text{Var}(Y)$ — wie viel Varianz erklärt Parameter $i$ **inklusive aller Interaktionen** mit anderen Parametern.

Ist $S_{T_i} \gg S_i$, hat der Parameter starke Wechselwirkungen mit anderen — genau das, was ein reiner OFAT-Sweep nie zeigen kann.

## 6. Praktisches Vorgehen in Python

```python
from scipy.stats.qmc import Sobol

sampler = Sobol(d=3, scramble=True, seed=42)   # d = Anzahl Parameter
punkte = sampler.random_base2(m=5)             # 2^5 = 32 Punkte in [0,1]^d

# anschließend auf echte Parametergrenzen skalieren, z.B.:
from scipy.stats.qmc import scale
grenzen_min = [1.0, 0.15e-3, 0.4]   # finPitch, finThickness, TubeLength ...
grenzen_max = [7.0, 0.30e-3, 0.8]
skaliert = scale(punkte, grenzen_min, grenzen_max)
```

Für eine vollständige Sensitivitätsanalyse-Pipeline (inkl. korrektem Sampling-Schema nach Saltelli) eignet sich die Bibliothek **SALib** (`SALib.sample.sobol`, `SALib.analyze.sobol`) — sie übernimmt automatisch, dass für Sobol-Indizes ein bestimmtes, größeres Sample-Schema nötig ist als für reines Space-Filling-Sampling.

## 7. Bezug zur Diplomarbeit

Aktuell wird der Rippenabstand einzeln variiert ([[200 Diplom/Design of Experiment]]), alle anderen Mikro-Parameter bleiben fix. Für die geplante Zero-Shot-Transfer-Auswertung (mehrere Mikro-Parameter gleichzeitig, siehe [[200 Diplom/Arbeitsstand]] Abschnitt 2.3) wäre ein Sobol-Sample über alle relevanten Mikro-Parameter gemeinsam sinnvoll — das liefert automatisch auch die Datenbasis für ein [[Gaussian Process]]-Surrogatmodell und eine echte Sensitivitätsanalyse statt nur einer visuellen "Baseline gut / außerhalb schlecht"-Einschätzung.
