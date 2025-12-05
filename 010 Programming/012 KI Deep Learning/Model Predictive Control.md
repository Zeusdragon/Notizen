
**Tags:** #control-theory #optimization #robotics #ai
**Status:** 🟢 Fertig

---

## 1. Was ist MPC?
Model Predictive Control (MPC) ist ein fortgeschrittenes Verfahren zur Prozessregelung, das ein **dynamisches Modell** des Systems verwendet, um das zukünftige Verhalten vorherzusagen und zu optimieren.

Im Gegensatz zu einfachen PID-Reglern, die nur auf Fehler in der Vergangenheit/Gegenwart reagieren, "plant" der MPC in die Zukunft.


## 2. Das Funktionsprinzip: Receding Horizon
MPC arbeitet iterativ in diskreten Zeitschritten. In jedem Schritt $t$ passiert folgendes:

1.  **Messung:** Der aktuelle Zustand $x_t$ des Systems wird gemessen.
2.  **Prädiktion & Optimierung:** Der Controller löst ein Optimierungsproblem über einen endlichen Zeithorizont $N$ (Prediction Horizon). Er sucht die optimale Folge von Stellgrößen (Inputs) $u$, um die Kostenfunktion zu minimieren.
    $$J = \sum_{k=0}^{N-1} (\|x_{k} - x_{ref}\|^2 Q + \|u_{k}\|^2 R)$$
3.  **Anwendung:** Nur der **erste** berechnete Input $u_0$ wird auf das System angewendet.
4.  **Wiederholung:** Im nächsten Zeitschritt $t+1$ verschiebt sich der Horizont, und alles wird neu berechnet ("Receding Horizon").

## 3. Hauptkomponenten
* **Systemmodell:** Beschreibt, wie sich der Zustand $x$ abhängig vom Input $u$ ändert (z.B. $x_{k+1} = Ax_k + Bu_k$).
* **Kostenfunktion (Cost Function):** Definiert, was "gut" ist (z.B. Abweichung vom Ziel minimieren, Energieverbrauch minimieren).
* **Constraints (Beschränkungen):** Physikalische Grenzen (z.B. maximale Motorleistung, Hindernisse).
    * $u_{min} \leq u \leq u_{max}$
    * $x_{min} \leq x \leq x_{max}$

## 4. Unterschied zu Reinforcement Learning
| Feature | MPC | Reinforcement Learning (RL) |
| :--- | :--- | :--- |
| **Modell** | Benötigt explizites physikalisches Modell | Lernt das Modell oder die Policy durch Erfahrung |
| **Optimierung** | Online (bei jedem Schritt) | Offline (Training), dann schnelle Ausführung |
| **Sicherheit** | Constraints werden mathematisch garantiert | Constraints sind schwerer zu garantieren |

> **Zusammenhang:** Modernes "Learning-based MPC" nutzt Neurale Netze, um die Systemdynamik zu lernen, wenn die Physik zu komplex für Formeln ist.

---
**Links:** [[Reinforcement Learning]], [[Pytorch Workflow]]