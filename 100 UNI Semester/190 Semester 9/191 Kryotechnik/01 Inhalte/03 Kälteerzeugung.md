---
title: "03 Kälteerzeugung"
type: vorlesung
erstellt: 2026-05-05
---

# 03 Kälteerzeugung

**Vorlesung**: [[191 Kryotechnik]]
**Datum**: 
**Topics**: [[Kreisprozesse]], [[Joule-Thomson]], [[Stirling]], [[Brayton]]
![[03 Kälteerzeugung.pdf]]

---

# Grundprinzipien
Um Wärme $Q_0$ von einem kalten Niveau $T_0$ auf Umgebungstemperatur $T_u$ zu pumpen, ist Arbeit $P$ nötig.
$$\dot{Q}_{ab} = \dot{Q}_0 + P$$

# Verfahrensübersicht

## 1. Kaltdampfprozess (Rankine)
* Standard für Kühlschränke.
* Nutzung der Verdampfungsenthalpie.
* Kältemittel: $NH_3, CO_2$, R-Gase.
* **Limit**: Funktioniert nur bis zum Tripelpunkt des Fluids. Für Tieftemperatur (Kryo) ungeeignet, da Fluide dort gefrieren würden (außer He).

## 2. Joule-Thomson-Prozess (Linde-Verfahren)
* **Prinzip**: Isenthalpe Drosselung eines realen Gases.
* **Voraussetzung**: Gas muss unterhalb der **Inversionstemperatur** sein.
* **Diagramm**: Nutzung des $T,s$-Diagramms. Die Isenthalpen müssen eine positive Steigung haben ($dT/dp > 0$).
* **Vorteil**: Keine bewegten kalten Teile.
* **Nachteil**: Geringe Effizienz.

## 3. Brayton / Claude Prozess
* **Prinzip**: Arbeitsleistende Entspannung (Isentrop).
* Nutzung von **Expansionsturbinen** oder Kolbenexpandern.
* **Claude-Prozess**: Kombination aus Expansionsturbine (für Vorkühlung) und Joule-Thomson-Stufe (für Verflüssigung).
* Standard für große Helium/Wasserstoff-Anlagen.

## 4. Stirling Kältemaschine
* **Prinzip**: Regenerativer Gaskreisprozess (Verdränger + Kolben).
* Phasen:
    1. Isotherme Kompression.
    2. Isochore Abkühlung (durch Regenerator).
    3. Isotherme Expansion (Kälteerzeugung).
    4. Isochore Erwärmung.
* **Anwendung**: Cryocooler, kleine $LN_2$-Erzeuger.