# 10 Supraleitung B - Theorie

**Vorlesung**: [[191 Kryotechnik]]
**Datum**: 18.11.2025
**Topics**: [[BCS-Theorie]], [[Meissner-Ochsenfeld]], [[Josephson-Effekt]], [[SQUID]]
[[Supraleitung B_Theorie.pdf]]
---

# Thermodynamik
[cite_start]Der Phasenübergang (normalleitend $\to$ supraleitend) ist ein **Übergang 2. Ordnung** (keine Latentwärme/Enthalpieänderung, aber Sprung in der spezifischen Wärme $c_p$)[cite: 14574].
* Elektronenanteil der spez. [cite_start]Wärme im SL-Zustand: $c_{EleS} \sim \exp(-C/T)$ (exponentieller Abfall deutet auf Energielücke hin)[cite: 9550].

# Meissner-Ochsenfeld-Effekt
* Supraleiter sind **ideale Diamagneten** ($\chi = -1$).
* [cite_start]Ein äußeres Magnetfeld wird durch Oberflächenströme vollständig aus dem Inneren verdrängt ($B=0$)[cite: 9377].
* Dies gilt unabhängig von der Vorgeschichte (Abkühlung im Feld oder Feld einschalten nach Abkühlung) $\to$ Reversibler thermodynamischer Zustand.

# Theorien

## London-Gleichungen (1935)
[cite_start]Phänomenologische Beschreibung[cite: 9401]:
1. Beschleunigungsgleichung (widerstandsfrei): $\partial_t \vec{j}_s \propto \vec{E}$.
2. Abschirmgleichung (Meissner-Effekt): $\nabla \times \vec{j}_s \propto -\vec{B}$.
* [cite_start]Führt zur **Eindringtiefe $\lambda_L$**: Das Magnetfeld fällt am Rand exponentiell ab[cite: 9578].

## BCS-Theorie (1957)
[cite_start]Mikroskopische Erklärung durch Bardeen, Cooper, Schrieffer (Nobelpreis)[cite: 9584].
* **Cooper-Paare**: Zwei Elektronen mit entgegengesetztem Spin ($\uparrow \downarrow$) und Impuls verbinden sich durch Gitterverformung (Phononen-Austausch).
* **Mechanismus**: Ein Elektron zieht positive Ionen an ("Kugel auf Matratze"), das zweite Elektron folgt in die Mulde.
* **Kondensat**: Alle Cooper-Paare besetzen denselben quantenmechanischen Grundzustand (Bosonen-Charakter).
* **Energielücke $2\Delta$**: Um ein Paar aufzubrechen, ist Energie nötig. [cite_start]Solange thermische Energie $k_B T < \Delta$, streuen die Paare nicht $\to$ **Widerstandslosigkeit**[cite: 9604].

# Josephson-Effekt & SQUIDs
* [cite_start]**Josephson-Effekt**: Cooper-Paare können durch eine dünne Isolatorschicht tunneln[cite: 9724].
* **SQUID (Superconducting Quantum Interference Device)**:
    * Ring mit zwei Josephson-Kontakten.
    * Extrem empfindlicher Magnetfeldsensor (misst einzelne Flussquanten $\Phi_0$).
    * [cite_start]Anwendungen: Medizin (MEG, MCG - Messung von Herz-/Hirnströmen), Materialprüfung[cite: 9831].
---

# Äußere Magentfelder
Ganz oder gar nicht wenn bei 1 wenn eine kritische Magentfeldstärke erreicht wird dann innenfeld = außenfeld
Bei typ2 wenn erster punkt erreicht dann gehen die schläuche immer neäher zusammen bis bc2 wo innenfeld wieder ausenfeld wird


![[Pasted image 20251028164751.png]]

# Josephson Effekt
Cooperpaare können Isolationsschicht überwinden, falls d < ${\xi}$
![[Pasted image 20251028165442.png]]


# SQUID

super quatnum interference device
![[Pasted image 20251028165737.png]]
![[Pasted image 20251028170847.png]]
![[Pasted image 20251028170855.png]]


damit lässt sich die B Änderung in einer eingeschlossenen Fläche messen

sehr hohe Auflösung
da sehr sensibel muss Abschirmung gemacht werden
Abschirmung durch
+ Metal schichten
+ Aluminium 
+ Feld Reduktion
+ shield faktoren
	+ 2x 10⁶ @ 0,01 Hz
	+ 2x 10⁸ @ 5 Hz
## Anwendung 
Nutzung für Gehirn und Herz observation
Kurz Distanz um Risse an Objekten zu gucken kann auch sonstiges Objekt sein zum Beobachten