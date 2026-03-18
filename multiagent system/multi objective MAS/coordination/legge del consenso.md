La policy del consenso utilizza solo informazioni locali (paradigma [reattivo](paradigmi%20di%20agenti.md#Reattivo)) per ridurre la distanza tra l'[agente](agenti.md) $i$-esimo e i suoi vicini $N_i$.
# Studio
Noi studiamo la policy del consenso a tempo [continuo](consenso%20a%20tempo%20continuo.md) e [discreto](consenso%20a%20tempo%20discreto.md) per un problema di [sincronizzazione](multiagent%20system/multi%20objective%20MAS/coordination/task.md#Sincronizzazione) in un sistema [multi agente](agenti.md#Multi%20agente). Dato che siamo in una sincornizzazione, il goal è $X_G=\{x=[x_1,\cdots,x_n]:x_i=x_j=\bar{x},\forall i,j\}$, vogliamo quindi che $\displaystyle\lim_{t\to+\infty}x_i(t)=\bar{x},\forall i$.
# Supposizione
Mettiamoci nello scenario della [single integrator dynamic](single%20integrator%20dynamic.md).
In generale per il tempo continua avremmo $\dot{x}_i(t)=u_i(t)$, dove lo stato $x_i(t)\in\mathbb{R}$ è uno scalare; per il tempo discreto avremmo $x_i(t+1)=u(t)$.