**Vorlesung**: [[Sensorik]]
**Datum**: 28.11.2025
**Topics**: [[Durchflussmessung]] 

$Wirbelfrequenz: f$
$Strouhal Zahl: Sr$$
$K = \frac{Sr}{Ad}$

![[03_VL_PMTS_DFM3.pdf]]
Prinzip ist Strömungsmechanisch
# Messprinzip

Prinzip ist einen Staukörper in die Leitung einzubauen und dadurch eine Wirbelablösung zu verursachen die sogenannte Karmansche Wirbelstraße entsteht.

Man kann die Wirbelfrequenz erfassen und darauf auf en Volumenstrom bzw den Massestrom kommen

$$\dot{V} = \frac{1}{K} f$$

Abhängigkeit $\dot{V}$ ~ $f$ 

K ist dabei eine Gerätespezifische Konstante

dadurch das K aus der Strouhalzahl berechnet wird. Strouhal abhängig von Staukörper geometrie und Reynoldszahl.

Abhängigkeit von Reynoldszahl beeinflussbar durch Staukörpergeometrie Scharfkantige Staukörper weniger Reynoldseinfluss

Es muss eine Mindestreynoldszahl vorhanden sein damit eine Wirbelablösung entsteht es darf aber auch nicht zu hoch sein da sonst sich gar keine Wirbel bilden können

Durch Geomettrie ist die Änderungsrate des K Faktors im Bereich von 1..2% Änderung in Abhängigkeit der Reynoldszahl. Daher kann über kompletten messbereich als Konstant angenommen werden

# Signalgeber
verschiedene Möglichkeiten Wirbelfrequenz zu messen dabei erfolgt die Messung meist im hinteren teil des Staukörpers und im vorderen bereich sind die Anströmflächen

## Messgrößen
### Temperatur
Thermistor kann Temperaturänderung wahrnehmen da der Wärmeübergang von der Geschwindigkeit abhängig ist
### Druck
Dynamsiche Druckmessung wenn Druck sich oben und unten ändert kann man daraus eine Frequenz ableiten
### Weg
Kugel kapazitiv taktil oder induktiv abtasten und Wegänderung in Frequenz übersetzen
### Verformung
Dehnmessstreifen an einer Zunge hinter Staukörper durch dynamische Last lässt sich Frequenz ableiten
### Dehnung
[[Piezoelektrische Wandler]]
### Schalllaufzeit
[[Ultraschallgeber]]



# Messbereich

Der Messbereich lässt sich aus Herstellergegebenen Konstanten C1 und C2 für $v_{min}$ und $v_{max}$ mit der Dichte des Fluids errechnen

$$v_{min} = \frac{C1}{\sqrt{\rho}}  ,    v_{max} = \frac{C2}{\sqrt{\rho}}$$

Bspw. Wasser $v_{min}$ = 0,38 m/s und $v_{max}$ = 6,11 m/s 

Falls andere Drücke verwendet werden wird Reynolds Skalierung verwendet
also mit Viskosität des Fluids rechnen und $Re_{min}$ und $Re_{max}$  

$$v_{min} = \frac{\nu \cdot Re_{min}}{D}, v_{max} = \frac{\nu \cdot Re_{max}}{D}$$


# Vorteile
+ Messsystem ist robust 
+ linearer Zusammenhang
+ weitesgehend unabhängig von Temperatur, Druck, Dichte, Viskosität vom Fluid
+ Kalibrierfaktor einheitlich für Gase und Flüssigkeiten (Gerätekonstante K)
+ geringe Messunsicherheit (typ. 0,5% v.M)
+ kann beliebig in jeder Lage eingebaut werden braucht keinen Vorlauf
+ preiswert

# Nachteile
+ Druckverlust am Staukörper
+ Signalgeber können durch Verschmutzung aussetzen
+ Messbereich eingeschränkt nach oben und insbsondere nach unten weil turbelente Strömung benötigt wird
+ Messung nicht schnell da 100 Wirbel gezählt werden müssen bis messsignal ermittelt werden kann wenn freqeunz 10 Hz dann braucht eine Messung 10 sekunden  