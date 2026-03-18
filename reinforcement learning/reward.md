Nel [reiforcement learning](reiforcement%20learning.md) quando l'agente esegue un'azione in uno stato riceve una ricompensa immediata $r(x(t),u(t))$, detta (immediate) reward, cioè una funzione $r:\mathbb{X}\times\mathbb{U}\to\mathbb{R}$. Il reward può essere negativo o positivo, ma deve essere limitato.
# Modellazione
La cosa più complessa quando definiamo la funzione di reward è stabilire un trade-off tra quanto essa esprime l'obiettivo dell'agente e quanto aiuta l'apprendimento.
Un reward può essere:
- Sparsa
	Restituisce 0 per tutti gli stati tranne uno.
	Una reward sparsa modella meglio l'obiettivo: l'agente sa che l'unico modo per massimizzare il return è andare nello stato che da una qualche reward positiva.
	Il problema è che con questo caso estremo l'agente non impara.
- Densa
	Non restituisce mai zero.
	Una reward densa permette all'agente di apprendere meglio perché da goal intermedi.
# Expected return
Non vogliamo solo considerare il reward (istantaneo), ma vorremmo che l'agente prendesse decisioni in modo da mettersi in una situazione che massimizzi anche i reward futuri.
Sulla base di questa idea si definisce l'expected return su una [policy](reinforcement%20learning/policy.md) $\pi$ come $R_\pi=\mathbb{E}\left\{\displaystyle\sum_{t=0}^\infty\gamma^tr(x(t),\pi(x(t))\right\}$, dove: il valore atteso è perché lo stato successivo è [incerto](reinforcement%20learning/markov%20decision%20process.md#Markov%20transition%20density) anche dato stato e azione attuale e quindi in esperimenti diversi ho return diversi  e quindi devo prendere una media (teoricamente un valore atteso, negli esperimenti una media); l'infinito viene dal fatto che non sappiamo quando l'episodio (i.e., l'esperimento) termina.
##### Discount factor
Il discount factor $\gamma$ serve per implementare l'idea che mi interessano reward futuri ma non troppo: un reward tra infinito tempo non è rilevate, ma è rilevante quello nel prossimo futuro.
Per impedire che il problema non abbia senso, cioè che si debba massimizzare un return infinito, è fondamentale (in task continui) che $\gamma\in(0,1]$: supponendo un reward bounded (i.e., $|r(x,u)|\le M,\exists M\in\mathbb{R}$) allora definendo $\gamma<1$ si ha $|R|=\displaystyle\mathbb{E}\{\sum_{t=0}^\infty\gamma^t|r(x(t),u(t))|\}\le\displaystyle\sum_{t=0}^\infty\gamma^tM=\tfrac{1}{1-\gamma}M$ e quindi la serie converge e quindi il return non può essere infinito.
##### Continuos vs Episodic task
Ci sono due tipi di task che possiamo eseguire: nei task continui vogliamo mantenere uno stato valido fino all'infinito e quindi il return è $R_\pi=\mathbb{E}\left\{\displaystyle\sum_{t=0}^\infty\gamma^tr(x(t),\pi(x(t))\right\}$; nei task episodici volgiamo raggiungere uno stato goal $X_f\subset \mathbb{X}$ entro un tempo $T$ (dove questo tempo di completamento è una [random variable](random%20variable.md) perché varia di esperimento in esperimento) e quindi il return è $R_\pi=\mathbb{E}\left\{\displaystyle\sum_{t=0}^T\gamma^tr(x(t),\pi(x(t))\right\}$.
Per eliminare questa differenza si aggiunge uno stato terminale assorbente (i.e., dove si finisce prima o poi) dal quale non ci si muove più e che ha una reward costante di 0; questo stato rappresenta la fine dell'episodio. In questo modo si riesce a modellare ogni problema come un task continuo.