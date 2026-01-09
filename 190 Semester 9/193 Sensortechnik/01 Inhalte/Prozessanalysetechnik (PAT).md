**Vorlesung**: [[Sensorik]]
**Datum**: 2026-01-07
**Topics**: [[Prozessanalysetechnik]], [[Spektroskopie]], [[UV/VIS]], [[FTIR]], [[Raman]], [[NMR]], [[Fluoreszenz]]

![[05_Vl_PMTS_PAT.pdf]]

---

# 05 Prozessanalysetechniken (PAT)

> [!INFO] Definition PAT
> Die Prozessanalysetechnik beschäftigt sich mit der Erfassung von **Stoffzusammensetzungen**, **Konzentrationen** und der **stofflichen Struktur** (Phasen, Vermischung) in technischen Prozessen.
> * **Ziel:** Echtzeit-Überwachung und Regelung von chemischen/physikalischen Prozessen.
> * **Methode:** Meistens **spektroskopische Verfahren**.

---

## 1. Grundlagen der Spektroskopie
Spektroskopie basiert auf der Wechselwirkung elektromagnetischer Strahlung mit Materie. Je nach Wellenlänge werden unterschiedliche Zustände im Molekül angeregt:

| Bereich | Wellenlänge | Wechselwirkung |
| :--- | :--- | :--- |
| **Röntgen** | < 10 nm | Innere Elektronen (Ionisation) |
| **UV / VIS** | 200 - 780 nm | **Valenzelektronen** (Äußere Schale) |
| **NIR** | 0,78 - 2,5 µm | Oberschwingungen von Molekülen |
| **MIR (IR)** | 2,5 - 50 µm | **Grundschwingungen** von Molekülen |
| **Mikrowellen** | > 1 mm | Molekülrotation |
| **Radiowellen** | > 1 m | Kernspin (NMR) |

---

## 2. UV/VIS/NIR Spektroskopie (Absorptionsspektroskopie)

### Prinzip
* Anregung von Valenzelektronen auf höhere Energieniveaus.
* Messung der **Transmission** $T = I/I_0$ bzw. der **Extinktion** (Absorption) $E$.

### Lambert-Beer'sches Gesetz
Der Zusammenhang zwischen Absorption und Konzentration ist linear (bei verdünnten Lösungen):
$$E = - \log_{10}(T) = \epsilon(\lambda) \cdot c \cdot d$$
* $E$: Extinktion
* $\epsilon$: Extinktionskoeffizient (stoffspezifisch)
* $c$: Konzentration
* $d$: Schichtdicke (Pfadlenge des Lichts)

### Aufbau eines Gitterspektrometers (Dispersiv)
1.  **Lichtquelle**: Deuteriumlampe (UV) oder Halogenlampe (VIS/NIR).
2.  **Monochromator**: Beugungsgitter zerlegt das Licht in seine Spektralfarben.
3.  **Probe**: Küvette oder Durchflusszelle.
4.  **Detektor**: Photodiode (Si, InGaAs) oder CCD-Zeile (für gleichzeitige Messung aller Wellenlängen).

---

## 3. Infrarot-Spektroskopie (IR / FTIR)

### Prinzip
* Anregung von **Molekülschwingungen** (Vibrationen) und Rotationen.
* **Auswahlregel:** Das Molekül muss sein **elektrisches Dipolmoment** während der Schwingung ändern (z.B. $HCl$, $H_2O$, $CO_2$). Symmetrische zweiatomige Moleküle ($N_2$, $O_2$) sind IR-inaktiv!
* Typische Schwingungen: Streckschwingungen (Stretching), Biegeschwingungen (Bending).

### FTIR-Spektrometer (Fourier-Transform-IR)
Statt eines Gitters (langsam, lichtschwach) wird ein **Michelson-Interferometer** genutzt.
1.  **Interferometer**: Strahlteiler teilt Licht auf festen und beweglichen Spiegel auf.
2.  **Interferogramm**: Detektor misst Intensität in Abhängigkeit vom Spiegelweg (Zeitdomäne).
3.  **Fourier-Transformation (FFT)**: Ein Computer rechnet das Interferogramm in ein Spektrum um (Frequenzdomäne).
* **Vorteile**: Viel schneller und lichtstärker als dispersive Geräte (Fellgett-Vorteil, Jacquinot-Vorteil).

### ATR-Sonde (Attenuated Total Reflection)
Für Prozessmessungen in undurchsichtigen oder stark absorbierenden Flüssigkeiten.
* Licht wird in einem Kristall (hoher Brechungsindex, z.B. Diamant, ZnSe) totalreflektiert.
* An der Grenzfläche zur Probe bildet sich eine **evaneszente Welle** aus, die wenige µm in die Probe eindringt.
* Wird Licht bei bestimmten Wellenlängen von der Probe absorbiert, wird die Totalreflexion gedämpft ("Attenuated").

---

## 4. Fluoreszenz-Spektroskopie

### Prinzip
1.  **Absorption**: Ein Photon hebt ein Elektron in einen angeregten Zustand ($S_0 \to S_1$).
2.  **Relaxation**: Teil der Energie wird wärmeabgegeben (vibronische Relaxation).
3.  **Emission**: Das Elektron fällt zurück und sendet ein Photon aus.
* **Stokes-Shift**: Das emittierte Licht ist energieärmer (langwelliger) als das angeregte Licht.
* Dargestellt im **Jablonski-Diagramm**.

---

## 5. Raman-Spektroskopie

### Prinzip
* Basiert auf **inelastischer Streuung** von monochromatischem Licht (Laser).
* **Auswahlregel**: Die **Polarisierbarkeit** der Elektronenhülle muss sich ändern.
* Komplementär zur IR-Spektroskopie (IR sieht Dipoländerungen, Raman sieht Polarisierbarkeitsänderungen).

### Streuarten
1.  **Rayleigh-Streuung**: Elastisch (gleiche Wellenlänge wie Laser). Intensiv, aber keine Info.
2.  **Stokes-Raman**: Energie wird an Molekül abgegeben (Licht wird röter/langwelliger). Standardfall.
3.  **Anti-Stokes-Raman**: Energie wird vom Molekül aufgenommen (Licht wird blauer). Nur bei bereits angeregten Molekülen (heiß).

---

## 6. NMR-Spektroskopie (Kernspinresonanz)

### Prinzip
* Nutzt den **Kernspin** (Drehimpuls) von Atomkernen (z.B. $^1H$, $^{13}C$).
* In einem starken externen Magnetfeld ($B_0$) richten sich die Spins aus (Zeeman-Effekt) und präzedieren mit der **Larmor-Frequenz**:
    $$\omega_0 = \gamma \cdot B_0$$
* Ein HF-Impuls regt die Spins an; beim Zurückfallen induzieren sie eine Spannung (FID - Free Induction Decay).

### Chemische Verschiebung
* Die Elektronen um den Kern schirmen das Magnetfeld ab.
* Unterschiedliche chemische Umgebungen (z.B. $CH_3$ vs. $OH$) führen zu leicht unterschiedlichen Resonanzfrequenzen.
* Dies erlaubt die genaue Bestimmung der **Molekülstruktur**.

---

# 🎓 Antworten auf die Prüfungsfragen

Hier sind die Antworten auf die Fragen von Folie 2, basierend auf den Folieninhalten:

### 1. Was versteht man unter Prozessanalysetechnik?
Unter Prozessanalysetechnik (PAT) versteht man Verfahren zur **Echtzeit-Erfassung** chemischer und physikalischer Eigenschaften in einem Prozess. Es geht um die Bestimmung von **Stoffzusammensetzungen**, **Konzentrationen** und **Strukturen** (z.B. Mischungszustand) direkt in der Anlage, meist mittels spektroskopischer Methoden, um den Prozess zu überwachen und zu regeln.

### 2. Beschreiben Sie Grundprinzipien, Aufbau und Funktionsweise eines UV/VIS/NIR-Spektrometers.
* **Grundprinzip:** Absorption von Licht durch Anregung von Valenzelektronen (UV/VIS) oder Moleküloberschwingungen (NIR). Es gilt das Lambert-Beer'sche Gesetz ($E = \epsilon \cdot c \cdot d$).
* **Aufbau (Dispersiv):**
    1.  **Lichtquelle:** Halogenlampe (VIS/NIR) oder Deuteriumlampe (UV).
    2.  **Probennahme:** Durchflussküvette.
    3.  **Monochromator:** Ein Beugungsgitter spaltet das polychromatische Licht spektral auf.
    4.  **Detektor:** Eine Photodiode oder ein Diodenarray (CCD) misst die Lichtintensität $I$ im Vergleich zur Referenz $I_0$.

### 3. Beschreiben Sie Grundprinzipien, Aufbau und Funktionsweise eines FTIR-Spektrometers mit ATR-Sonde.
* **Grundprinzip:** Anregung von Molekül-Grundschwingungen im mittleren Infrarot. Moleküle müssen ihr Dipolmoment ändern.
* **Aufbau (Interferometer):** Nutzung eines Michelson-Interferometers (Strahlteiler, fester Spiegel, beweglicher Spiegel). Das gemessene Interferogramm wird per Fourier-Transformation in ein Spektrum umgerechnet.
* **ATR-Sonde (Attenuated Total Reflection):** Dient der Messung. Der IR-Strahl wird in einem Kristall mit hohem Brechungsindex geführt und an der Grenzfläche zum Medium totalreflektiert. Eine evaneszente Welle dringt leicht in das Medium ein. Absorbiert das Medium bei bestimmten Wellenlängen, wird der reflektierte Strahl geschwächt. Dies ermöglicht Messungen in trüben/stark absorbierenden Flüssigkeiten.

### 4. Beschreiben Sie die Fluoreszenzspektroskopie.
Die Fluoreszenzspektroskopie nutzt die Emission von Licht nach vorheriger Anregung.
1.  Ein Molekül absorbiert ein Photon hoher Energie (kurze Wellenlänge) und geht in einen angeregten Zustand ($S_1$) über.
2.  Durch strahlungslose Relaxation (Wärme) verliert es etwas Energie.
3.  Es fällt in den Grundzustand ($S_0$) zurück und sendet dabei ein Photon geringerer Energie (längere Wellenlänge) aus.
* Die Verschiebung hin zu längeren Wellenlängen nennt man **Stokes-Shift**. Sie ist sehr empfindlich und selektiv (gut für Spurenanalytik).

### 5. Beschreiben Sie die Raman-Spektroskopie.
Raman nutzt die **inelastische Streuung** von monochromatischem Licht (Laser) an Molekülen.
* Ändert sich die **Polarisierbarkeit** der Elektronenhülle eines Moleküls durch eine Schwingung, wird ein kleiner Teil des Lichts in seiner Frequenz verschoben.
* **Stokes-Streuung:** Das gestreute Licht hat weniger Energie als der Laser (Energie bleibt im Molekül $\to$ Molekül schwingt stärker).
* Das Raman-Spektrum liefert einen "Fingerabdruck" der Molekülschwingungen, ähnlich dem IR-Spektrum, aber mit anderen Auswahlregeln (komplementär).

### 6. Beschreiben Sie die NMR-Spektroskopie.
Die NMR (Nuclear Magnetic Resonance) nutzt den **Kernspin** von Atomen (z.B. Wasserstoffkerne $^1H$).
* In einem starken statischen Magnetfeld ($B_0$) richten sich die Kernspins aus und rotieren mit der **Larmor-Frequenz**.
* Ein senkrechter Hochfrequenz-Impuls lenkt die Magnetisierung aus. Beim Relaxieren senden die Kerne ein Signal aus.
* Da die Elektronen um den Kern das Magnetfeld lokal abschirmen, verschiebt sich die Resonanzfrequenz je nach chemischer Bindung (**Chemische Verschiebung**).
* Dadurch lässt sich die Struktur von Molekülen extrem genau bestimmen.