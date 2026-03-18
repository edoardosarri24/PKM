La dinamica in un sistema [multi agente](agenti.md#Multi%20agente) descrive come l'agente cambia il suo stato nel tempo. La dinamica a singolo integratore definisce tale cambiamento $u(t)$ sulla base della velocità $\dot{x}(t)$ dell'agente, cioè $\dot{x}(t)=u(t)$; è detta singolo integratore perché la posizione $x(t)$ può essere ottenuta integrando una volta tale velocità.
# Osservazioni
- In questo modello, a differenza del [double integrator dynamics](double%20integrator%20dynamics.md), lo stato dell'agente è definito solo sulla base della posizione $x(t)$.
- Intuitivamente $\dot{x}(t)$ indica il cambio di stato infinitesimale di un agente approssimato a un punto mobile.
# Definizione
Possiamo definire $\begin{cases}\dot{q}=u(t)&\text{ nel tempo continuo}\\q(t+1)=q(t)+T_su(t)&\text{ nel tempo discreto }\end{cases}$, dove:
- $q(t)$ è la posizione del robot all'instante $t$.
- $u(t)$ la velocità del robot, che possiamo anche definire come il comportamento del robot, cioè la sua azione.
- $T_s$ è un tempo campionato, cioè è il tempo in cui mi sposto della velocità.