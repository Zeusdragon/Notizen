Wir mappen input x mit einer funktion zu output y dabei wird besteht die funktion aus drei Teilen.

Drei lineare Terme für x mit parameter $\theta_{10} ,\theta_{11},\theta_{20},\theta_{21},\theta_{30},\theta_{31}$
anschließend die jeweiligen terme n eine Aktivierungsfunktion packen und danach ein gewicht auf die jeweiligen Terme legen.
Training schaut ob 

Generell gilt:
$$y = \phi_0 + \sum\limits_{d=1}^D{\theta_{d0}h_d}$$
## Aktivierungsterme
Ab wann wird ein Term $\theta_{n0} + \theta_{n1} \cdot x$ aktiviert
Beispiel ReLU Funktion gibt den wert des Terms aus wenn positiv sonst 0
ReLU bekanteste schon 1969 genutzt
in early devolopment von NN war sigmoid oder tanh activation verbreitet allerdings wird seit 2010 wieder lieber ReLU genutzt
ReLU nachteil das alles negative null wird also wenn negative inputs vorhanden sind dann kann nichts verbessert werden.
