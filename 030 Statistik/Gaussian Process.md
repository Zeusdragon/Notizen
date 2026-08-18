# Gaussian Process (GP) — Grundlagen und Anwendung im Diplom

**Tags:** #statistik #gaussian-process #surrogate-model #sensitivity-analysis #diplom
**Status:** 🟡 In Arbeit
**Bezug:** [[DoE Grundlagen]] · [[Sobol-Sequenzen]] · [[200 Diplom/Arbeitsstand]] · [[200 Diplom/Geometrische Parameter]]

---

## 1. Was ist ein Gaussian Process?

Ein Gaussian Process (GP) ist eine Verallgemeinerung der Normalverteilung von Zufalls**vektoren** auf Zufalls**funktionen**. Statt Mittelwert + Kovarianzmatrix hat man eine Mittelwertsfunktion $m(x)$ und eine Kovarianzfunktion (Kernel) $k(x, x')$:

$$f(x) \sim \mathcal{GP}(m(x), k(x, x'))$$

Die Kerneigenschaft: **jede endliche Menge** von Funktionswerten $f(x_1), \dots, f(x_n)$ ist gemeinsam gaußverteilt. Dadurch lässt sich alles mit normaler multivariater Gauß-Algebra rechnen, obwohl man eigentlich über eine unendlich-dimensionale Funktion spricht.

In der Praxis wird ein GP als **Regressionsmethode** genutzt (in der Geostatistik "Kriging" genannt): Gegeben Trainingsdaten $(X, y)$, berechnet man die Posterior-Verteilung von $f$ an neuen, unbeobachteten Punkten $x_*$. Das Ergebnis ist nicht nur eine Punktvorhersage, sondern für jeden Punkt eine **Verteilung** — also automatisch eine Unsicherheitsangabe.

## 2. Der Kernel bestimmt alles

Der Kernel $k(x,x')$ legt fest, wie "glatt" bzw. wie stark korreliert die modellierte Funktion ist. Am gebräuchlichsten ist der RBF-/Squared-Exponential-Kernel:

$$k(x,x') = \sigma_f^2 \exp\left(-\frac{\lVert x-x' \rVert^2}{2\ell^2}\right)$$

* $\ell$ (Lengthscale): wie schnell die Korrelation zwischen zwei Punkten mit ihrem Abstand abfällt → wie glatt die Funktion aussieht. Kleines $\ell$ = wellige Funktion, großes $\ell$ = fast lineare Funktion.
* $\sigma_f^2$ (Signal-Varianz): wie stark die Funktion insgesamt um ihren Mittelwert schwankt.
* $\sigma_n^2$ (Noise): Beobachtungsrauschen (bei deterministischen Simulationen wie Dymola/FMU theoretisch ~0, praktisch oft ein kleiner Wert für numerische Stabilität).

Diese Hyperparameter werden **nicht von Hand gewählt**, sondern automatisch aus den Trainingsdaten gelernt, indem man die Marginal-Likelihood maximiert (macht z.B. `scikit-learn` automatisch beim `.fit()`).

## 3. Ablauf der GP-Regression (Kernformeln)

Prior: $f \sim \mathcal{GP}(0, k)$. Beobachtungen: $y = f(X) + \varepsilon$, $\varepsilon \sim \mathcal{N}(0, \sigma_n^2)$.

Posterior an neuen Punkten $X_*$ ist wieder gaußverteilt mit:

$$\mu_* = K_*^T (K + \sigma_n^2 I)^{-1} y$$
$$\Sigma_* = K_{**} - K_*^T (K + \sigma_n^2 I)^{-1} K_*$$

wobei $K = k(X,X)$, $K_* = k(X,X_*)$, $K_{**} = k(X_*,X_*)$. Wichtig für die Anwendung: $\mu_*$ ist die Vorhersage, die Diagonale von $\Sigma_*$ ist die Vorhersage-**Varianz** — je weiter ein Punkt $x_*$ von den Trainingsdaten entfernt liegt, desto größer wird sie automatisch.

## 4. Warum GP als Surrogatmodell für teure Simulationen?

* Dymola/FMU-Sweeps sind rechen- und zeitintensiv → man kann nicht beliebig viele Parameterkombinationen simulieren.
* Ein GP macht aus wenigen (z.B. 20–100), gut über den Raum verteilten Sample-Punkten (→ [[DoE Grundlagen]], [[Sobol-Sequenzen]]) ein **stetiges, praktisch kostenloses** Vorhersagemodell für die gesamte Antwortfläche.
* Die mitgelieferte Unsicherheit zeigt genau, **wo** man wenig weiß — Regionen mit hoher Vorhersagevarianz sind Kandidaten für weitere, gezielte Simulationen (aktives Lernen / Bayesian Optimization), statt blind den ganzen Raum dicht zu simulieren.
* Ein GP-Surrogat kann anschließend praktisch kostenlos für Sensitivitätsanalysen (Sobol-Indizes) ausgewertet werden, ohne weitere teure Dymola-Läufe.

## 5. Anwendung für die Diplomarbeit

### Kontext
Die Zero-Shot-Transfer-Auswertung (Kapitel 5 & 6, siehe [[200 Diplom/Arbeitsstand]]) soll zeigen, wie robust der RL-Agent gegenüber Verdampfer-Geometrien ist, für die er nicht trainiert wurde. Mikro-Parameter (finPitch, finThickness, Tube-/ParallelTubeDistance, siehe [[200 Diplom/Geometrische Parameter]]) beeinflussen Blockage-Ratio und luftseitigen Druckverlust und damit die Reifdynamik → wirken auf COP/SCOP und Abtauverhalten. Aktuell existiert nur ein OFAT-Sweep über den Rippenabstand ([[200 Diplom/Design of Experiment]]); gesucht werden 3 repräsentative Archetypen (**Baseline, Frost-Falle, Dauerläufer**).

### Konkreter Workflow

1. **Parameterraum festlegen:** relevante Mikro-Parameter + sinnvolle Min/Max-Grenzen definieren, Makro-Parameter (Bauraum/Leistungsklasse) bleiben fix.
2. **DoE-Sampling:** eine Sobol-Sequenz (oder LHS) über den normierten $[0,1]^d$ Raum erzeugen, dann auf die echten Parametergrenzen skalieren (z.B. 32 oder 64 Punkte — Zweierpotenz wegen der Sobol-Eigenschaften). Deutlich informativer als der bisherige One-Parameter-Sweep, weil Interaktionen zwischen den Mikro-Parametern erfasst werden. Details: [[DoE Grundlagen]], [[Sobol-Sequenzen]].
3. **Simulieren:** für jeden Sample-Punkt eine FMU parametrieren, mit Referenzregler oder trainiertem RL-Agent simulieren, Zielgrößen extrahieren (SCOP, Abtauzyklen $n$, absolute Abtauzeit, Abtauanteil %) — dieselben Kernmetriken, die in [[200 Diplom/Arbeitsstand]] Abschnitt 2.3 bereits für die Evaluierung vorgesehen sind.
4. **GP fitten:** je Zielgröße ein eigenes GP-Regressionsmodell (Input = normierter Parametervektor, Output = z.B. SCOP). Inputs vorher standardisieren (unterschiedliche Skalen: mm vs. Stückzahl). Kernel: RBF + WhiteKernel als guter Start, mit mehreren Restarts der Optimierung gegen lokale Minima.
5. **GP validieren:** Leave-One-Out-Kreuzvalidierung oder Train/Test-Split, $R^2$/RMSE prüfen, und checken ob die vorhergesagte Unsicherheit realistisch ist (tatsächlicher Fehler sollte in etwa zur vorhergesagten Varianz passen) — sonst sind die Kernel-Hyperparameter schlecht gefittet.
6. **Sensitivitätsanalyse auf dem GP:** Sobol-Indizes ($S_i$, $S_{T_i}$) mit `SALib` direkt auf dem (billig auswertbaren) GP-Surrogat berechnen statt auf echten Simulationen → zeigt formal, welcher Mikro-Parameter (und welche Parameter-Interaktionen) den größten Einfluss auf SCOP/Abtauverhalten haben. Ersetzt die rein visuelle "um Baseline gut, außerhalb schlecht"-Einschätzung durch belastbare Zahlen.
7. **Archetypen aus der GP-Fläche ableiten:**
   - **Baseline:** nominale Geometriewerte (bereits bekannt)
   - **Frost-Falle:** Punkt, an dem die GP-Vorhersage den schlechtesten SCOP / die meisten Abtauzyklen zeigt (lokales Minimum der Zielgröße auf der Fläche)
   - **Dauerläufer:** Punkt mit gutem SCOP und geringer Sensitivität — eine flache Region der GP-Fläche, in der der Wert über einen größeren Bereich stabil bleibt
   - Die so gefundenen Extrempunkte sollten anschließend **einmal real simuliert** werden, um die GP-Vorhersage zu verifizieren — das GP ist nur ein Surrogat, kein Ersatz für die finale Aussage.
8. **Optional — aktives Nachverfeinern:** Zeigt das GP an entscheidenden Stellen hohe Unsicherheit, gezielt dort neue Punkte simulieren und das GP neu fitten (Bayesian-Optimization-Idee). Spart gegenüber einem dichten, blinden Sweep spürbar Rechenzeit.

### Python-Werkzeuge

```python
from sklearn.gaussian_process import GaussianProcessRegressor
from sklearn.gaussian_process.kernels import RBF, WhiteKernel, ConstantKernel
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler().fit(X_train)
X_scaled = scaler.transform(X_train)

kernel = ConstantKernel() * RBF(length_scale=[1.0] * X_train.shape[1]) + WhiteKernel()
gp = GaussianProcessRegressor(kernel=kernel, n_restarts_optimizer=10, normalize_y=True)
gp.fit(X_scaled, y_train)  # y_train z.B. SCOP je Sample-Punkt

y_pred, y_std = gp.predict(X_neu_scaled, return_std=True)  # Vorhersage + Unsicherheit
```

* **Sampling:** `scipy.stats.qmc.Sobol` oder `SALib.sample.sobol.sample`
* **GP:** `sklearn.gaussian_process.GaussianProcessRegressor` (reicht i.d.R. für ≤10 Dimensionen, wenige hundert Punkte); für spätere Bayesian Optimization bei der Nachverfeinerung eignen sich `GPyTorch`/`BoTorch` besser
* **Sensitivität:** `SALib.analyze.sobol`

### Warum das besser ist als der aktuelle OFAT-Sweep

Der bestehende Rippenabstand-Sweep ([[200 Diplom/Design of Experiment]]) zeigt nur "um Baseline gut, außerhalb schlecht" für **einen** Parameter — er kann weder Wechselwirkungen mit anderen Mikro-Parametern erfassen, noch beantworten, ob die Baseline bei anderen Parameterkombinationen überhaupt noch repräsentativ ist. Mit Sobol-Sampling + GP bekommt man bei ähnlichem oder nur leicht höherem Simulationsaufwand eine vollständige, stetige Antwortfläche über den gesamten relevanten Parameterraum, kann daraus formal die wichtigsten Parameter identifizieren (Sobol-Indizes) und die drei Archetypen für Kapitel 5/6 **datenbasiert statt intuitiv** auswählen.

## 6. Weiterführend

* [[Sobol-Sequenzen]] — für das Sampling-Schema und die Sensitivitätsindizes
* [[DoE Grundlagen]] — Einordnung gegenüber OFAT, Full-Factorial, LHS
* [[200 Diplom/Arbeitsstand]] — aktueller Stand & offene Punkte zur Zero-Shot-Evaluierung
* [[200 Diplom/Geometrische Parameter]] — die konkreten Mikro-/Makro-Parameter des Verdampfers
