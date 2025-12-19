**Vorlesung**: [[Sensorik]]
**Datum**: 19.12.2025
**Topics**: [[Faseroptische Sensoren]]

![[04_VL_PMTS_FaseroptischeSensoren.pdf]]

---

# 04 Faseroptische Sensoren

> [!INFO] Zusammenfassung
> Faseroptische Sensoren nutzen Lichtwellenleiter (LWL), um physikalische Größen wie Temperatur, Dehnung oder Druck zu messen. Sie basieren auf der Änderung von Lichteigenschaften (Intensität, Phase, Wellenlänge, Polarisation) durch die Messgröße.

## Grundlagen Lichtwellenleiter

Die Lichtübertragung in Glasfasern basiert auf dem Prinzip der **Totalreflexion**.
* **Aufbau**: Ein lichtführender **Kern** (hoher Brechungsindex $n_1$) ist von einem **Mantel** (Cladding, niedrigerer Brechungsindex $n_2$) umgeben.
* **Schutz**: Eine zusätzliche Beschichtung (Coating) schützt die Faser vor mechanischer Belastung und Feuchtigkeit.
* **Funktion**: Lichtstrahlen, die flach genug auf die Grenzfläche treffen, werden totalreflektiert und im Kern geführt.

### Arten von Lichtwellenleitern
> [!INFO] Klausurrelevant
> Arten von Lichtwellen kommen in der Klausur dran

* **[[Glasfaser]]**: Der Standard für Telekommunikation und Sensorik (Langstrecke).
* **[[Kunststofffaser]]** (POF): Preiswerte Alternative für Kurzstrecken.
* **[[InfraRot-LWL]]**: Spezialfasern für Laserlichtübertragung und Spektroskopie.
* **[[Saphir-LWL]]**: Speziallösung für extreme Hochtemperatur-Sensorik (bis 2000 °C).

> [!NOTE] Dämpfung
> LWL haben signalbedingte Verluste (Dämpfung).
> * **Metrik**: $0,5~dB/km$ (typisch für Glasfaser).
> * **Beispiel**: Bei 20 km Länge wird das Signal um den Faktor 10 abgeschwächt ($10~dB$).

---

## Sensorprinzipien

Man unterscheidet grundsätzlich zwei Arten, wie die Faser genutzt wird:

1.  **Extrinsische Sensoren**:
    * Die Faser dient nur als **Transportmedium** für das Licht.
    * Die eigentliche Messung findet außerhalb der Faser statt (z.B. optische Sonden, Lichtschranken).
    * *Messfunktion an der Sondenspitze.*

2.  **Intrinsische Sensoren**:
    * Die Faser selbst ist das **Sensorelement**.
    * Die physikalische Größe wirkt direkt auf die Faser ein und verändert die Lichtausbreitung im Inneren.
    * *Messfunktion innerhalb des Lichtwellenleiters.*

--- 

## Faser-Bragg-Gitter (FBG)

Das Faser-Bragg-Gitter ist einer der wichtigsten faseroptischen Sensoren.

### Funktionsweise
* In den Faserkern wird ein **periodisches Gitter** mit variierender Brechzahl eingeschrieben.
* Dieses Gitter wirkt wie ein selektiver Spiegel: Es reflektiert nur eine ganz bestimmte Wellenlänge (die **Bragg-Wellenlänge** $\lambda_B$), während alle anderen Wellenlängen transmittiert werden.
* Ändert sich die Temperatur oder wird die Faser gedehnt, ändert sich die Gitterperiode und damit die reflektierte Farbe (Wellenlänge).

### Abhängigkeit von Dehnung und Temperatur
Die Verschiebung der Zentralwellenlänge $\Delta \lambda$ hängt von der mechanischen Dehnung $\Delta \epsilon$ und der Temperaturänderung $\Delta T$ ab.

$$\frac{\Delta\lambda}{\lambda_{0}} = (1-p_{e})\Delta\epsilon + (\alpha_{\Lambda}+\alpha_{n})\Delta T$$

**Parameter**:
* $\lambda_0$: Ausgangswellenlänge
* $p_e$: Photoelastischer Koeffizient (Reaktion auf Dehnung)
* $\alpha_\Lambda$: Wärmeausdehnungskoeffizient
* $\alpha_n$: Thermooptischer Koeffizient (Brechzahländerung durch Temperatur)

> [!WARNING] Querempfindlichkeit
> Da FBG-Sensoren sowohl auf **Dehnung** als auch auf **Temperatur** reagieren, muss man in der Praxis oft eine Größe kompensieren, um die andere exakt zu messen (z.B. durch eine lose Referenzfaser, die nur die Temperatur sieht).

### Anwendung: Verteilte Messung
Da man viele Gitter mit unterschiedlichen Gitterkonstanten (unterschiedlichen Reflexionsfarben) in eine einzige Faser schreiben kann, lassen sich viele Messpunkte entlang einer einzigen Leitung abfragen (Multiplexing).