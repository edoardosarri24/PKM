Il modello più semplice che descrive l'interazione [agente](agenti.md)-ambiente è totalmente deterministico (nella realtà poi non sarà così).
È descritto da $x(t+1)=f(x(t),u(t))$, dove:
- $t$
	Il tempo in questione.
- $x_a(t)$
	È lo stato o la configurazione dell'agente, cioè qualunque informazione che lo descrive.
- $x_e(t)$
	È lo stato o la configurazione dell'ambiente, cioè qualunque informazione che lo descrive.
- $x(t)$
	È lo stato complessivo del mondo, cioè $x(t)=(x_a(t),x_e(t))$. Lo spazio degli stati è $\mathbb{X}$.
- $u(t)$
	È l'azione presa dall'agente al tempo $t$. Lo spazio delle azioni è $\mathbb{U}$. L'azione può modificare sia lo stato dell'agente che lo stato dell'ambiente.
# Spazio azione e stato
Lo spazio delle azioni $\mathbb{U}$ e degli stati $\mathbb{X}$ può essere discreto, continuo o ibrido: nel primo caso le variabili che descrivono l'azione o lo stato sono finite, nel secondo caso sono continue, nell'ultimo invece alcune possono essere continue e altre discrete.
##### Discretizzazione
Solitamente è utile semplificare: un modello continuo può essere discretizzato in modo che le tecniche discrete possano essere applicate pagando solo un'approssimazione.
La discretizzazione può essere fatta con qualunque livello di accuratezza: l'ambiente in cui si muove un robot può essere approssimato a una griglia con celle di qualunque dimensione; più le celle sono piccole più l'ambiente discreto è approssimabile a quello continuo.
# Reward e return
Il reward è una funzione che calcola uno score istantaneo che definisce la bontà di un'azione quando siamo in uno stato. Si definisce come $r(t,x(t),u(t))$. L'insieme delle reward lungo tutto il tempo è definito come total reward (o return o utility function) ed è $R=\displaystyle\sum_{t=0}^Tr(t,x(t),u(t))$. Il return è ciò che vogliamo massimizzare, cioè è strettamente legata alla [funzione obiettivo](agenti.md#Obiettivo) dell'agente.
# Observation model
Solitamente un agente non riesce in ogni istante di tempo a conoscere l'intero stato $x_e$ dell'ambiente, ma può solo raccogliere informazioni parziali; a esmepio un sensore visivo non vede attraverso i muri.
Queste informazioni sono descritte dal modello di osservazione $h$ e sono memorizzate in $y(t)=h(x(t))$, con $y(t)\in\mathbb{Y}$ che è lo spazio delle osservazioni (discreto, continuo o ibrido). Se $y(t)=x(t)$ allora si dice che le informazioni sono complete, altrimenti le informazioni sono parziali.
##### Information vector
Siccome l'agente non percepisce tutto l'ambiente non basta memorizzare solo $y(t)$ (l'ultima osservazione), ma anche le osservazioni e le azioni precedenti; questo perché nel passato potrebbe aver osservato qualcosa che ora non è visibile.
Questo viene fatto nell'information vector $I(t)=[y(0),u(0)\cdots,u(t-1),y(t)]$. La decisione dell'agente sulla prossima azione a questo punto è presa in base alle informazioni raccolte, cioè abbiamo $u(t)=\pi(t,I(t))$, dove $\pi$ è la [policy](multiagent%20system/robot%20motion%20planning/policy.md).
##### Determinismo
Spesso nelle situazioni reali l'observation model non è deterministico, cioè a parità di stato posso avere osservazioni $y(t)$ diverse; ad esempio per il rumore della misurazione di un sensore.
In questo caso l'osservazione è data da una likelihood function (=funzione di vero somiglianza) $l(y(t)|x(t))$.
Si parla in questo caso di Partially Observable [markov decision process](reinforcement%20learning/markov%20decision%20process.md).