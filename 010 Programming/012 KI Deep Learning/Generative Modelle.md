---
title: "Generative Modelle"
type: konzept
status: fertig
tags:
  - deep-learning
  - unsupervised
  - generative
  - ai
erstellt: 2026-09-07
---

**Quelle:** Simon J.D. Prince — *Understanding Deep Learning*, Kapitel 14–18 (Unsupervised learning, GANs, Normalizing flows, VAEs, Diffusion models)

---

## 1. Die gemeinsame Aufgabe
Beim [[Supervised Learning]] lernt man $Pr(y|x)$. Generative Modelle lernen die Verteilung der Daten selbst: $Pr(x)$ — mit dem Ziel, **neue plausible Beispiele zu erzeugen**.

Das Grundprinzip aller vier Familien ist dasselbe:

$$\mathbf z \sim \mathcal N(0, \mathbf I) \quad \longrightarrow \quad \mathbf x = g(\mathbf z, \boldsymbol\phi)$$

Ein einfacher **latenter Vektor** $\mathbf z$ wird durch ein Netz in ein komplexes Datenbeispiel verwandelt. Die Annahme dahinter: Reale Daten liegen auf einer **niedrigdimensionalen Mannigfaltigkeit** im hochdimensionalen Raum ([[Generalisierung und Double Descent]]).

Die Familien unterscheiden sich nur darin, **wie** sie $g$ trainieren, weil die exakte Likelihood $Pr(x)$ in aller Regel nicht berechenbar ist.

| Familie | Likelihood | Qualität | Sampling | Latenter Raum |
| :--- | :--- | :--- | :--- | :--- |
| **GAN** | keine | sehr gut | 1 Schritt (schnell) | ja |
| **Normalizing Flow** | exakt | mittel | 1 Schritt | ja, invertierbar |
| **VAE** | untere Schranke | verwaschen | 1 Schritt | ja, strukturiert |
| **Diffusion** | untere Schranke | exzellent | viele Schritte (langsam) | ja |

## 2. GAN (Kapitel 15)
Zwei Netze im Wettstreit:
* **Generator** $g(\mathbf z)$ erzeugt Fälschungen.
* **Discriminator** entscheidet: echt oder gefälscht?

$$\min_{\boldsymbol\phi}\max_{\boldsymbol\theta}\ \mathbb E_{x}[\log D(x)] + \mathbb E_{z}[\log(1 - D(g(z)))]$$

Kein Likelihood-Term, nur ein Minimax-Spiel. Der Discriminator ist ein **gelernter Loss**.

**Bekannte Probleme:**
* **Mode Collapse:** Der Generator produziert nur noch wenige Varianten, die zuverlässig durchgehen.
* **Instabiles Training:** Das Gleichgewicht ist kein Minimum, sondern ein Sattelpunkt. Kippt der Discriminator zu schnell, gibt es keinen brauchbaren Gradienten mehr.
* **Gegenmittel:** Wasserstein-Loss + Gradient Penalty, Progressive Growing, StyleGAN.

## 3. Normalizing Flows (Kapitel 16)
Baue $g$ als **invertierbare** Funktion. Dann liefert die Transformationsformel die exakte Dichte:

$$Pr(x) = Pr(z)\left|\det \frac{\partial g}{\partial z}\right|^{-1}, \quad z = g^{-1}(x)$$

Damit ist Maximum Likelihood direkt möglich. Der Preis: Jede Schicht muss invertierbar sein **und** eine billig berechenbare Jacobi-Determinante haben. Das erreicht man mit **Coupling Layers** (RealNVP: eine Hälfte des Vektors bleibt unverändert und parametrisiert die Transformation der anderen) oder autoregressiven Flows. Die Architekturfreiheit ist dadurch stark eingeschränkt.

## 4. Variational Autoencoder (Kapitel 17)
Ein latentes Variablenmodell mit $Pr(x) = \int Pr(x|z)Pr(z)\,dz$. Das Integral ist nicht lösbar, also maximiert man eine untere Schranke, die **ELBO** (Evidence Lower Bound):

$$\text{ELBO} = \underbrace{\mathbb E_{q(z|x)}[\log Pr(x|z)]}_{\text{Rekonstruktion}} - \underbrace{D_{KL}\big(q(z|x)\,\|\,Pr(z)\big)}_{\text{Regularisierung des Latentraums}}$$

* **Encoder** $q(z|x)$ sagt $\mu$ und $\sigma$ der Posterior-Verteilung voraus.
* **Decoder** $Pr(x|z)$ rekonstruiert.
* **Reparametrisierungstrick:** $z = \mu + \sigma \odot \epsilon$ mit $\epsilon \sim \mathcal N(0,\mathbf I)$ — nur so lässt sich durch den Zufallsknoten hindurch differenzieren.

Der KL-Term zwingt den Latentraum in eine glatte Normalverteilungsform, sodass Zwischenwerte sinnvolle Ausgaben liefern. Typische Schwächen: **verwaschene Ergebnisse** (Folge des mittelnden Rekonstruktions-Loss) und **posterior collapse** (der Decoder ignoriert $z$). Erweiterung: $\beta$-VAE für stärkere Entflechtung ("disentanglement").

## 5. Diffusion Models (Kapitel 18)
Das heute dominierende Verfahren.

**Vorwärtsprozess (fest, kein Lernen):** Schrittweise Gauß-Rauschen zu einem Bild addieren, bis nach $T$ Schritten reines Rauschen übrig ist. Dank der Reparametrisierung springt man direkt zu beliebigem $t$:

$$\mathbf z_t = \sqrt{\alpha_t}\,\mathbf x + \sqrt{1-\alpha_t}\,\boldsymbol\epsilon$$

**Rückwärtsprozess (gelernt):** Ein Netz — meist ein **U-Net** ([[Convolutional Neural Network]]) — bekommt $\mathbf z_t$ und $t$ und sagt das **hinzugefügte Rauschen** voraus. Der Loss ist damit erstaunlich simpel:

$$L = \big\|\, \boldsymbol\epsilon - \hat{\boldsymbol\epsilon}(\mathbf z_t, t, \boldsymbol\phi) \,\big\|^{2}$$

Generieren heißt: mit Rauschen starten und $T$-mal ein bisschen Rauschen abziehen.

**Warum so gut?** Das Problem wird in viele winzige, leichte Teilprobleme zerlegt statt in einen einzigen großen Sprung. Das Training ist stabil (reine Regression, kein Minimax).

**Erweiterungen:**
* **Conditional Generation / Classifier-free Guidance:** Text- oder Klassenkonditionierung; im Sampling wird die Differenz zwischen konditionierter und unkonditionierter Vorhersage verstärkt.
* **Latent Diffusion (Stable Diffusion):** Diffusion nicht im Pixelraum, sondern im komprimierten Latentraum eines Autoencoders — drastisch billiger.
* **DDIM und andere Solver:** reduzieren die Sampling-Schritte von ~1000 auf ~20.

## 6. Einordnung
Für Regelungs- und Simulationsaufgaben sind generative Modelle indirekt interessant:
* als **gelernte Surrogat-Dynamik** (Weltmodell) für model-based [[Reinforcement Learning]], wenn ein FMU-Schritt zu teuer ist,
* zur **Erzeugung synthetischer Betriebs- oder Wetterszenarien**,
* **Diffusion Policies** erzeugen ganze Aktionssequenzen statt einzelner Aktionen und können damit mehrdeutige Verhaltensweisen abbilden, an denen ein MSE-Modell scheitert.

---
**Links:** [[Supervised Learning]], [[Convolutional Neural Network]], [[Transformer]], [[Loss Functions]], [[Reinforcement Learning]]
