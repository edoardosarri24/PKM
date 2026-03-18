Una automa a tempo supporta la rappresentazione di un sistema concorrente, esattamento come lo fanno le [Petri Net (PN)](Petri%20Net%20(PN).md).

Un time automata (TA) è un automa finito arricchito con:
- Un insieme di clock.
- Vincoli temporali sulle transizioni.
- Un reset dei clock sulle transizioni.

Un [TA minimo](minimum%20time%20automata.png) è composto da: due locazioni $H$ e $K$, due clock $x$ e $y$ e una transizione da $H$ a $K$.
# SINTASSI
Un TA è una tupla $\langle L,L_0,\Sigma, X, E\rangle$ ([esempio](esempio%20di%20time%20automata.png)), dove:
- $L$ è un insieme di locazioni.
- $L_0\in L$ è un insieme di locazioni iniziali.
- $\Sigma$ è un alfabeto finito di azioni.
- $X$ è un insieme di clock.
- $E\subset L\times\Sigma\times G(X)\times 2^X\times L$ è un insieme di archi dove $G(X)$ è un insieme di congiunzioni di guardie della forma $x\sim c$, dove $x\in X$, $\sim\in\{<,\le,=,>\ge\}$ e $c\in \mathcal{R}$.
# SEMANTICA
##### Valutazione di un clock
La valutazione del clock $v\in\mathcal{R^X_{\ge0}}$ assegna un valore a ogni clock.
- Se la valutazione di $v$ soddisfa la guardia $g\in G(X)$, allora si scrive $v\vDash g$.
- $v+\tau$ indica la valutazione $(v+\tau)(x)=v(x)+\tau,\forall\tau\in\mathcal{R_{\ge0}}$. 
##### Stato
Lo stato di un TA è la coppia $(l,v)$, dove:
- $l$ è una locazione.
- $v$ è una valutazione.
##### Transizioni
Tra gli stati esistono due tipi di transizioni:
- Delay
	$(l,v)\xrightarrow{\tau}(l,v+\tau)$ per ogni stato $(l,v)$ e ritardo (delay) $\tau\in\mathcal{R_{\ge0}}$.
- Discrete
	$(l,v)\xrightarrow{a}(l',v'))$ se $\exists(l,a,g,R,l')\in E|v\vDash g, v'(x)=0\mbox{ se }x\in R\mbox{ oppure }v'(x)=v(x)\mbox{ se } x\notin R$.
##### Run
Un run di un TA è un alternarsi di transizioni delay e discrete.
- [Esempio](run%20of%20TA%20example.png)
##### Parola temporizata (timed word)
È una sequenza finita di coppie $(a_i,t_i)$, dove:
- $a_i\in\Sigma$.
- $t_i$ appartiene a una time sequence, cioè una sequenza finita di valori reali non nulli.

Una times word è accettata da un TA se esiste $\rho=(l_0,v_0)\xrightarrow{\tau_1,a_1}\cdots\xrightarrow{\tau_k,a_k}(l_k,v_k)$ con $l_0\in L_0$.
# PARTIZIONE DELLA REGIONE
Quelli che nelle [TPN](Time%20Petri%20Net%20(TPN).md) erano le classi di stato, qua prendono il nome di regioni, che sono un insieme di valutazioni equivalenti.
Se due stati che stanno nella stessa classe di stati (nelle TPN) non sono per forza uguali (alcuni attivano delle transizioni, altri ne attivano altre), quando due valutazioni stanno nella stessa regione vuol dire che sono uguali, cioè il loro comportamento futuro è le stesso. Avere regioni in cui le valutazioni sono totalmente equivalenti porta a un'esplosione del numero delle regioni.
##### Equivalenza di valutazioni
Due valutazioni sono equivalenti quando:
- Soddisfano lo stesso insieme dei vincoli sui clock.
- Il trascorre del tempo non distingue le due valutazioni.
- Il reset dei clock non distingue le due valutazioni.
##### Grafo delle regioni
Il grafo delle regioni descrive cosa può succedere nel tempo senza occuparsi della locazione in cui ci troviamo.
Se abbiamo due clock e non abbiamo vincoli diagonali, cioè vincoli che coinvolgono due clock contemporaneamente, allora le forme delle regioni che possiamo ottenere sono (nel caso di due clock) triangoli (superiori e inferiori), linee verticali, orizzontali e diagonali e un punti.
##### Automa delle regioni
L'automa delle regioni descrive cosa succede in relazione alle locazioni. I nodi del grafo sono una coppia descritta da una locazione e una regione.
Il problema dell'automa delle regioni è il numero delle regioni è esponenziale nel numero dei clock.