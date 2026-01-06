5**Vorlesung**: [[191 Kryotechnik]]
**Datum**: 2026-01-06
**Topics**: [[NMR]], [[MRI]], [[Stromzuführungen]], [[Wiedemann-Franz-Gesetz]]
![[Supraleitung F_NMR; SZF.pdf]]

---

# 14 Supraleitung F: NMR & Stromzuführungen

## 1. NMR & MRI (Kernspinresonanz)
Nutzung des Kernspins (meist Wasserstoff-Protonen) in einem starken Magnetfeld.
* **Prinzip**: Ausrichtung der Spins im $B_0$-Feld (Zeeman-Aufspaltung) $\to$ Anregung mit HF-Puls $\to$ Messung des Relaxationssignals (FID).
* **Resonanzbedingung (Larmor-Frequenz)**:
  $$\Delta E = h \cdot \nu = \gamma \cdot \hbar \cdot B_0$$
  * Je stärker $B_0$, desto höher die Frequenz und Auflösung (Signal-Rausch-Verhältnis steigt linear bis quadratisch mit $B_0$).
* **Anwendung**:
    * **NMR (Spektroskopie)**: Strukturaufklärung von Molekülen (bis > 1 GHz / 23,5 T).
    * **MRI (Bildgebung)**: Ortskodierung durch zusätzliche Gradientenspulen. 

## 2. Stromzuführungen (Current Leads)
Die elektrische Verbindung von Raumtemperatur (300 K) zum kalten Supraleiter (4 K) ist die thermische Schwachstelle jedes Magnetsystems.

### Das Dilemma (Wiedemann-Franz-Gesetz)
Gute elektrische Leiter (für den Strom) sind leider auch gute Wärmeleiter (Wärmeleck).
$$\frac{\lambda}{\sigma} = L \cdot T \quad (\text{für Metalle})$$
* **Optimierung**: Es gibt ein optimales Verhältnis von Länge zu Querschnitt ($L/A$), bei dem die Summe aus *Wärmeleitung* (von warm) und *Joule'scher Wärme* ($I^2 R$) minimal ist.
* **Minimum (reine Wärmeleitung)**: ca. **$42 \, mW/A$** (pro Leiter!). Das ist für Kryostate oft zu viel.

### Technische Lösungen
1.  **Gasgekühlte Stromzuführungen**:
    * Nutzung des kalten Helium-Abgases, das am Leiter entlang strömt und die Wärme aufnimmt.
    * Reduktion auf ca. **$1 \dots 1,2 \, mW/A$**.
    * Aufbau: Bündel aus Drähten oder Netzen (große Oberfläche für Wärmetausch).
2.  **Hybrid-Stromzuführungen (HTS)**:
    * Einsatz von Hochtemperatursupraleitern (YBCO/BSCCO) im kalten Teil (z.B. 4 K bis 60 K).
    * HTS leitet Strom widerstandsfrei, leitet aber Wärme schlecht (Keramik!).
    * **Vorteil**: Das Wiedemann-Franz-Gesetz wird umgangen. Wärmeeintrag extrem gering.