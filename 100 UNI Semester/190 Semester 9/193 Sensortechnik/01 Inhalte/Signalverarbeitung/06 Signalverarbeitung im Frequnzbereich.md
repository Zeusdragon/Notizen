---
title: "06 Signalverarbeitung im Frequnzbereich"
type: vorlesung
tags:
  - Signalverarbeitung
  - Fensterfunktionen
erstellt: 2026-05-05
---

# 6.1 Fourier-Reihen

**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 6.1)

---

## Prinzip
Jede **periodische** Funktion lässt sich als Summe von harmonischen Schwingungen (Sinus/Cosinus) darstellen.
$$f(t) = \sum_{n=-\infty}^{+\infty} F_n e^{j\omega_n t}$$
[cite_start]mit den Grundfrequenzen $\omega_n = 2\pi n / T$[cite: 302].

## Koeffizienten
Die Fourier-Koeffizienten $F_n$ berechnen sich durch Integration über eine Periode:
$$F_n = \frac{1}{T} \int_{-T/2}^{+T/2} f(t) e^{-j\omega_n t} dt$$
---

# 6.2 Fourier-Transformation

**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 6.2)

---

## Definition
Transformation eines (auch nicht-periodischen) Signals vom Zeitbereich in den Frequenzbereich.
$$F(\omega) = \int_{-\infty}^{+\infty} f(t) e^{-j\omega t} dt$$
[cite_start]Das Ergebnis $F(\omega)$ ist das **Spektrum** (komplexwertig)[cite: 305].

## Rücktransformation
$$f(t) = \frac{1}{2\pi} \int_{-\infty}^{+\infty} F(\omega) e^{j\omega t} d\omega$$

## Eigenschaften (für reelle Signale)
* [cite_start]$F(\omega) = F^*(-\omega)$: Spektrum bei negativen Frequenzen ist konjugiert komplex zu positiven[cite: 317].
* **Amplitudenspektrum** $|F(\omega)|$ ist symmetrisch (gerade).
* **Phasenspektrum** ist antisymmetrisch (ungerade).
* Breite Signale im Zeitbereich $\leftrightarrow$ schmale Spektren im Frequenzbereich (und umgekehrt).
---
# 6.3 Signalleistung

**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 6.3)

---

## Leistungssignal
Definiert als Verlustleistung an einem $1\,\Omega$-Widerstand: $p(t) = u(t)^2$.
Das **Leistungsspektrum** ist das Quadrat des Amplitudenspektrums:
$$P(\omega) = |F(\omega)|^2$$

## Dezibel (dB)
Logarithmisches Maß für Leistungsverhältnisse.
$$V_{dB} = 10 \log_{10} \frac{P_1}{P_2} = 20 \log_{10} \frac{U_1}{U_2}$$
(Faktor 20 bei Spannungen, da Leistung quadratisch zur Spannung ist) [cite_start][cite: 347].

# 6.4 Differenziale und Integrale im Frequenzbereich

**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 6.4)

---

Die Fourier-Transformation vereinfacht Calculus-Operationen zu algebraischen Operationen:

1. **Ableitung:**
   $$\frac{d}{dt} f(t) \leftrightarrow j\omega F(\omega)$$
   [cite_start]Differenzieren im Zeitbereich entspricht Multiplikation mit $j\omega$ im Frequenzbereich[cite: 350].

2. **Integration:**
   $$\int f(t) dt \leftrightarrow \frac{1}{j\omega} F(\omega)$$
   Integrieren entspricht Division durch $j\omega$.


# 6.5 Zeitverschobene Signale im Frequenzbereich

**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 6.5)

---

## Verschiebungssatz
Eine Verschiebung im Zeitbereich um $\Delta t$ führt zu einer **Phasendrehung** im Frequenzbereich. Die Amplitude bleibt gleich.
$$f(t - \Delta t) \leftrightarrow e^{-j\omega \Delta t} F(\omega)$$
[cite_start][cite: 353]


# 6.6 Faltung

**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 6.6)

---

## Definition
Die Faltung (Convolution) ist die zentrale Operation der Systemtheorie (z.B. Anwendung eines Filters auf ein Signal).
$$g(t) = f(t) * h(t) = \int_{-\infty}^{+\infty} h(t - t') f(t') dt'$$
[cite_start]Anschaulich wird der "Faltungskern" $h(t)$ gespiegelt und über das Signal $f(t)$ geschoben[cite: 355].

**Beispiel:** Glättung (Mittelwertbildung) ist eine Faltung mit einem Rechteckpuls.



# 6.7 Faltung im Frequenzraum

**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 6.7)

---

## Faltungstheorem
Eine Faltung im Zeitbereich entspricht einer einfachen **Multiplikation** im Frequenzbereich.
$$f(t) * g(t) \leftrightarrow F(\omega) \cdot G(\omega)$$
[cite_start]Das macht Faltungsoperationen (Filterung) oft effizienter, wenn man sie über die FFT berechnet[cite: 367].



# 6.8 Korrelation und Faltung

**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 6.8)

---

## Zusammenhang
Korrelation ist ähnlich wie Faltung, aber ohne Zeitumkehr des zweiten Signals. Im Frequenzbereich gilt:
$$R_{fg}(\omega) = F^*(\omega) G(\omega)$$

## Wiener-Chintschin-Theorem
[cite_start]Das Leistungsspektrum $|F(\omega)|^2$ ist die Fourier-Transformierte der **Autokorrelationsfunktion**[cite: 375].

## Parsevalsche Theorem
Die Energie des Signals ist im Zeit- und Frequenzbereich gleich.
$$\int |F(\omega)|^2 d\omega = \int f^2(t) dt$$



# 6.9 Abtastung analoger Signale: Abtasttheorem und Aliasing

**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 6.9)

---

## Abtastung
Mathematisch: Multiplikation des Signals mit einem Dirac-Kamm.
Im Frequenzbereich führt dies zu einer **Periodisierung des Spektrums**. [cite_start]Das Spektrum wiederholt sich im Abstand der Abtastfrequenz $\omega_A$[cite: 386].

## Aliasing
Wenn die Abtastfrequenz zu niedrig ist, überlappen sich die periodischen Spektren. Hohe Frequenzen werden als falsche tiefe Frequenzen interpretiert ("Geisterfrequenzen").

## Nyquist-Shannon-Abtasttheorem
[cite_start]Um Aliasing zu vermeiden, muss die Abtastfrequenz mehr als doppelt so hoch sein wie die höchste im Signal vorkommende Frequenz[cite: 392].
$$\omega_A > 2 \omega_G$$
Die **Nyquist-Frequenz** ist $\omega_{Ny} = \omega_A / 2$.

**Lösung:** Anti-Aliasing-Tiefpassfilter vor der Abtastung verwenden.



# 6.10 Abtastung von unendlichen Signalen

**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 6.10)

---

## Problem des Zeitfensters
Da Computer nur endliche Signale verarbeiten können, wird aus einem unendlichen Signal ein Stück (Fenster $\Delta T$) herausgeschnitten.
[cite_start]Dies entspricht einer Multiplikation mit einem Rechteckfenster im Zeitbereich $\rightarrow$ **Faltung mit einer Sinc-Funktion** im Frequenzbereich[cite: 423].

## Leck-Effekt (Leakage)
Wenn die Signalfrequenz kein exaktes Vielfaches der Fensterbreite ist ($n/\Delta T$), entstehen Unstetigkeiten an den Rändern (bei periodischer Fortsetzung).
Im Spektrum "schmiert" die Energie auf benachbarte Frequenzen (Sinc-Nebenkeulen). [cite_start]Frequenzen tauchen auf, die gar nicht da waren[cite: 444].



# 6.11 Fenster für die Begrenzung unendlicher Signale

**Quelle:** [[Signalverarbeitung Skript.pdf]] (Kapitel 6.11)

---

## Lösung für Leakage
Verwendung von Fensterfunktionen, die an den Rändern sanft gegen Null gehen, um Unstetigkeiten (harte Sprünge) zu vermeiden.

**Typische Fenster:**
* **Rechteck:** (Kein Fenster), höchste Frequenzauflösung, aber starkes Leakage.
* [cite_start]**Von-Hann (Hanning), Hamming, Blackman:** Reduzieren die Nebenkeulen (weniger Leakage), verbreitern aber die Hauptkeule (etwas schlechtere Frequenzauflösung)[cite: 462].