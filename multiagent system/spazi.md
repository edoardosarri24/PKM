Il mondo in cui si muove un [agente](agenti.md) è definito da uno spazio reale $\mathbb{R}^2$ o $\mathbb{R^3}$, detto work space $W$.
# $so(2)$
Oltre ai classici spazi euclidei in $\mathbb{R}^n$, ci interessano gli spazi in cui si considerano angoli; in questo contesto solitamente si considera lo special ortogonal $so(2)$, cioè lo spazio non cartesiano degli angoli di rotazione.
##### Caratteristiche
Questo spazio è isomorfo alla circonferenza unitaria, cioè si ripete dopo $2\pi$. Se su una retta gli angoli 0 e $2\pi$ sono lontani, in questo spazio invece sono vicini. Questo diventa importante quando si considerano le distanze tra punti.
##### Rappresentazione
Per rappresentare $so(2)$ dobbiamo tenere in considerazione la caratteristica dell'isomorfismo, cioè della ripetizione.
- $so(2)$
	Lo spazio si rappresenta con una circonferenza: in questo modo ovviamente gli angoli 0 e 2$\pi$ sono gli stessi.
- $so(2)^2$
	Lo spazio si rappresenta come un toro (la ciambella): in questo modo 0 e $2\pi$ sono vicini in entrambe le dimensioni.
	Se vogliamo aprire il toro si può rappresentare in un piano in due dimensioni dove sia l'ascissa che l'ordinata vanno da 0 a $2\pi$. In questo caso si deve pensare che se la fine dell'altro (di destra) è l'inizio del asso (di sinistra) e viceversa.
# Configuration space
Un configuration space $C$ è lo spazio cui prendono valore tutte le possibili configurazioni $q_i$ dell'agente, cioè abbiamo che $q\in C$. Definisce in modo univoco la posizione del robot in $W$ secondo un sistema di riferimento fisso.
Solitamente si considera l'insieme minimale; ci sono però delle situazioni in cui può semplificare la complessità prende uno spazio non minimale.
##### Caratteristiche
- La dimensionalità dello workspace e del configuration space non sempre sono uguali.
- La dimensionalità del configuration space coincide con il numero di gradi di libertà dell'agente e quindi può essere molto elevata portando alla [maledizione of dimensionality](dimensionality%20reduction.md#La%20maledizione%20della%20dimensionalità).
##### Esempi
- Robot puntiforme
	- 2D
		Abbiamo $q=[p_x,p_y]$ con $C=\mathbb{R}^2$ e $W=\mathbb{R}^2$.
	- 3D
		Abbiamo $q=[p_x,p_y,p_z]$ con $C=\mathbb{R}^3$ e $W=\mathbb{R}^3$.
- Corpo solito in 2D
	Lo spazio di lavoro è $W=\mathbb{R}^2$.
	La configurazione è data dalle coordinate del centro di rotazione e dell'angolo che il robot dorma con gli assi, cioè $q=[p_x,p_y,\theta]$. Lo spazio di configurazione è quindi da $C=\mathbb{R}^2\times so(2)$.
- Braccio robotico che si muove nel piano con tre snodi.
	Lo spazio di lavoro è $W=\mathbb{R}^2$.
	La configurazione è data dai tre angoli, cioè $q=[\theta_1,\theta_2,\theta_3]$.
	Lo spazio di configurazione è dato da $C=so(2)^3$.
# Free and obstacle space
Il free space $C_{free}$ e l'obstacle space $C_{obs}$ permettono di capire quali sono le configurazioni ammissibili e quelle vietate dell'agente.
Ad esempio nel [robot motion planning](robot%20motion%20planning.md) ci interessa infatti trovare un percorso da start a goal in $W$ in modo che l'agente non colpisca ostacoli; questo percorso in $W$ può essere visto come una sequenza di configurazioni ammissibili in $C$.
##### Definizione
Definiamo la funzione $R:C\to W$, tale che $R(q)\subset W$, come la funzione che mappa una configurazione dell'agente nel mondo reale. Se $W_{obs,k}\subset W$ è l'insieme dei punti che compongono il $k$-eismo ostacolo nel workspace, allora $C_{obs,k}=q\in C:W_{obs,k}\cap R(q)\ne\emptyset$ è l'insieme delle configurazioni in cui il robot collide con l'oggetto $k$-esimo.
Possiamo adesso definire l'insieme delle configurazioni ammissibili come $C_{free}=C-C_{obs}$, con $C_{obs}=\bigcup_kC_{obs,k}$. Notiamo che $C_{obs}$ è complementare rispetto a $C_{free}$.