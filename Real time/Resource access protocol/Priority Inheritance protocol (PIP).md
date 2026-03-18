È un [protocollo di accesso alle risorse](Resource%20access%20protocol.md).
# ASPETTI CHIAVE
- Accesso
	Il task $\tau_i$ si blocca all'ingresso della CS (quando chiama $wait$) se la risorsa $R_k$ è già posseduta da un task $\tau_j$ a priorità più bassa.
	- Si dice che $\tau_i$ è bloccato da $\tau_j$.
	- I task bloccati sono schedulati in base alla priorità dinamica. Se due task bloccati hanno pari priorità vengono schedulati sulla base FCFS (First Come First Served).
- Progresso
	All'interno di una CS associata alla risorsa $R_k$ il task $\tau_j$ che esegue ha priorità dinamica $p_j=max\{P_j,max_i\{P_i|\tau_i\ è\ bloccato\ su\ R_k\}\}$.
	- Si dice che il task eredità la massima priorità di task bloccati su quella risorsa.
	- Gode della proprietà transitiva: se $\tau_3$ blocca $\tau_2$, e $\tau_2$ blocca $\tau_1$ (con $P_1>P_2>P_3$) allora $p_3=P_1$.
- Rilascio
	Quando $\tau_j$ esce dalla CS associata alla risorsa $R_k$ allora:
	- Il semaforo $S_k$ viene rilasciato (operazione $signal$).
	- Viene svegliato il task a priorità maggiore che era bloccato dalla risorsa $R_k$ e quindi da $S_k$.
	- Se non blocca altri task allora $p_j=P_j$, altrimenti $p_j$ è la massima $P_a$ tra i $\tau_a$ bloccati da $\tau_j$.
# TIPI DI BLOCCO
- Direct block
	Quando un task a priorità più alta viene bloccato da uno a priorità più bassa perché richiede una risorsa posseduta dal task a priorità più bassa.
- Push-through block
	Quando un task a priorità media viene bloccato da uno a priorità nominale più bassa ma a priorità dinamica attuale maggiore.
# PROPRIETÁ
In generale vogliamo vedere quantitativamente quando un task si blocca e quanto spesso si blocca, in modo da poter osservare se il blocked time è buonded o unbounded. Si vorrebbe l'esistenza di un intero che indichi il numero di volte per cui un task è bloccato; in questo modo il blocked time sarebbe bounded.
##### Lemma 1
Un semaforo $S_k$ può provocare un push-through blocking al task $\tau_i$ solo se $S_k$ è acceduto da un task con priorità $>P_i$ e uno con priorità $<P_i$.
- Vuol dire che il task a priorità più bassa eredita la priorità del task a priorità più alta e quello con priorità intermedia viene bloccato da quello a priorità bassa.
- Dimostrazione
	Per assurdo supponiamo che $S_k$ sia acceduto da $\tau_l$ che ha priorità $<P_i$ ma non da un task con priorità $>P_i$. Allora $\tau_l$ non può ereditare la priorità $>P_i$ e allora $\tau_i$ farà preemption su $\tau_l$.
##### Lemma 2
La priority inheritance può avvenire solo nel caso di CS nested.
- Dimostrazione
	L'ereditarietà di priorità avviene quando un task $\tau_h$ a priorità alta è bloccato da un task $\tau_m$ a priorità media che a sua volta è bloccato da un task $\tau_l$ a priorità bassa.
	Allora $\tau_m$ ha il semaforo $S_a$ (che blocca $\tau_h$) e $\tau_l$ ha il semaforo $S_b$ (che blocca $\tau_m$). Allora $\tau_m$ attende il semaforo $S_b$ all'interno della CS guardata da $S_a$ e allora si hanno due CS nested.
##### Lemma 3
Se ci sono $l_i$ task a priorità più bassa di $\tau_i$ che lo possono bloccare, allora $\tau_i$ può essere bloccato al più per la durata delle $l_i$ CS.
- Vuol dire che un task a priorità bassa può bloccare al più (ma anche nessuna) volta un task a priorità più alta con cui condivide una risorsa.
- Dimostrazione
	Il task $\tau_i$ può essere bloccato da un task $\tau_j$ a priorità più bassa solo se $\tau_i$ fa preemption su $\tau_j$ mentre $\tau_j$ è all'interno della CS $z_{j,k}$ (cioè $\tau_i$ richiederà la risorsa posseduta da $\tau_j$).
	Allora $\tau_j$ può subire preemption da $\tau_i$ solo quando esce dalla CS $z_{j,k}$ e a questo punto $\tau_i$ non può essere bloccato da $\tau_j$ un'altra volta (perché la priorità di $\tau_j$ scenderà). Allora $\tau_i$ può essere bloccato al più una volta da ogni task $\tau_j$ a priorità più bassa, cioè $l_i$ volte.
##### Lemma 4
Se ci sono $s_i$ semafori distinti (e quindi $s_i$ risorse) che possono bloccare $\tau_i$, allora $\tau_i$ può essere bloccato per al più la durata delle $s_i$ CS.
- È lo stesso del [lemma 3](Priority%20Inheritance%20protocol%20(PIP).md#Lemma%203) ma con i semafori e quindi le risorse al posto dei task. Ci interessa considerare entrambi gli aspetti.
- Dimostrazione
	Siccome i semafori sono binari un solo task $\tau_j$ a priorità più bassa di $\tau_i$ può essere all'interno di una CS. Allora $\tau_j$ può subire preemption da $\tau_i$ solo quando esce dalla CS e a questo punto $\tau_i$ non può essere bloccato da $\tau_j$ un'altra volta. Allora $\tau_i$ può essere bloccato al più una volta da ogni task $\tau_j$, cioè $s_i$ volte.
##### Teorema
Sotto PIP un task $\tau_i$ può essere bloccato per al più la durata di $\alpha_i=min\{l_i,s_i\}$ CS.
- Si considera il minore perché nel [lemma 3](Priority%20Inheritance%20protocol%20(PIP).md#Lemma%203) e nel [lemma 4](Priority%20Inheritance%20protocol%20(PIP).md#Lemma%204) c'è il termine "al più".
- Ci dice che il blocked time è bounded.
# CONSEGUENZE
### Vantaggi
- Protocollo efficiente perché un task viene bloccato solo quando ce n'è effettivamente bisogno.
- Implementazione trasparente al programmatore, nel senso che il tutto viene gestito dallo scheduler.
- Il blocked time è bounded, al più grande come $\alpha_i$ per il task $\tau_i$.
### Svantaggi
- Calcolare il blocked time è abbastanza complesso a causa del direct blocking, push-through blocking dell'inversion priority.
- È soggetto al [chained blocking](chain%20blocking%20esample%20PIP.png). In questo caso ogni task $\tau_i$ è bloccato $\alpha_i$ volte nel caso pessimo.
- È soggetto al [deadlock](deadlock%20example%20PIP.png), cioè quando ogni task è bloccato in attesa di una risorsa posseduta da un altro task. Questo è dovuto al fatto che PIP fa il minimo sindacale, ovvero che alza la priorità di $\tau_i$ solo quando questo blocca $\tau_j$, con $P_j>P_i$.
# ESEMPI
- [Due tipi di blocchi](PIP%20type%20of%20block.png).
- [Corretta annidazione della CS](PIP%20CS%20nested.png).
- [Ereditarietà della priority](PIP%20priority%20iheritance.png).