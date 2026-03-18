Il problema della coordinazione in un sistema [multi agente](agenti.md#Multi%20agente) è così strutturato:
- $N$ è il numero di agenti.
- $x_i(t)$ è lo stato dell'$i$-esimo agente al tempo $t$.
- $x(t)=(x_1(t),\cdots,x_n(t))$ è lo stato complessivo del sistema al tempo $t$, definito dallo stato di tutti gli agenti.
- $u_i(t)$ è l'azione dell'$i$-esimo agente al tempo $t$.
- L'$i$-esimo agente osserva il proprio stato e quello degli agenti vicini, cioè l'[observation model](modello.md#Observation%20model) è $y_i(t)=\begin{bmatrix}x_i(t)\\x_j(t), \forall j\in N_i\end{bmatrix}$.
- Siamo in un modello [collaborativo](agenti.md#Multi%20agente): i vari agenti hanno un obiettivo comune, ma le loro [policy](multiagent%20system/robot%20motion%20planning/policy.md) sono indipendenti, cioè $u_i(t)=\pi_i(y_i(t))$.
# Formalmente
Supponendo lo stato collettivo iniziale sia $x(0)\in X_A$, si vuole trovare una policy $\pi=[\pi_1,\cdots,\pi_n]$ tale che:
- Lo stato collettivo $x(t)$ è sempre ammissibile, cioè $x_i(t)\in X_A,\forall t,i$.
- Per ogni condizione iniziale $x(0)$ lo stato collettivo converge asintoticamente alla configurazione desiderata, cioè $\forall x(0),\exists x_g\in X_G$ tale che $x(t)\displaystyle\to_{t\to\infty} x_g$.
# Topic
- [task](multiagent%20system/multi%20objective%20MAS/coordination/task.md)
- [legge del consenso](legge%20del%20consenso.md)
- [APF per il coordinamento](APF%20per%20il%20coordinamento.md)
