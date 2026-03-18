È un [protocollo di accesso alle risorse](Resource%20access%20protocol.md).
##### Ceiling
Il ceiling $C(S_k)$ di un semaforo $S_k$ è la più alta priorità nominale tra quelle dei task che possono usare $S_k$, cioè $C(S_k)=max_{i\in\{1,\cdots,n\}}\{P_i|\tau_i\ può\ usare\ S_k\}$.
# ASPETTI CHIAVE
- Accesso
	Il task $\tau_i$ si blocca all'ingresso della CS (quando chiama $wait$) se la sua priorità non è maggiore del massimo ceiling del semaforo bloccato da altri task, cioè se $P_i\le max_k\{C(S_k)|S_k\mbox{ è posseduto da un task} \ne \tau_i\}$.
	- Vuol dire che un task non può entrare in una CS regolata da un semaforo libero se ci sono semafori bloccati che potrebbero bloccarlo in futuro. Ovvero che una volta che un task entra nella sua prima CS non può essere bloccato da un task a priorità inferiore. [Esempio](ceiling%20block%20example%20PCP.png).
	- Osserviamo che in questo caso un task può bloccarsi a causa di una risorsa anche se non ha la certezza che la userà.
- Progresso (come [PIP](Priority%20Inheritance%20protocol%20(PIP).md))
	All'interno di una CS associata alla risorsa $R_k$ il task $\tau_j$ che esegue, ha priorità dinamica $p_j=max\{P_j,max_i\{P_i|\tau_i\ è\ bloccato\ su\ R_k\}\}$.
	- Si dice che il task eredità la massima priorità di task bloccati su quella risorsa.
	- Gode della proprietà transitiva: se $\tau_3$ blocca $\tau_2$, e $\tau_2$ blocca $\tau_1$ (con $P_1>P_2>P_3$) allora $p_3=P_1$.
- Rilascio (come [PIP](Priority%20Inheritance%20protocol%20(PIP).md))
	Quando $\tau_j$ esce dalla CS associata alla risorsa $R_k$ allora:
	- Il semaforo $S_k$ viene rilasciato (operazione $signal$).
	- Viene svegliato il task a priorità maggiore che era bloccato dalla risorsa $R_k$ e quindi da $S_k$.
	- Se non blocca altri task allora $p_j=P_j$, altrimenti $p_j$ è la massima $P_a$ tra i $\tau_a$ bloccati da $\tau_j$.
# TIPI DI BLOCCO
- Ceiling blocking
	Quando un task si blocca perché non ha superato la politica di accesso. [Esempio](ceiling%20block%20example%20PCP.png).
# PROPRIETÁ
##### Lemma 1
Se il task $\tau_k$ può subire preemption quando è nella CS dal task $\tau_i$ che entra in una CS diversa, allora $\tau_k$ non può ereditare una priorità $\ge$ della priorità del task $\tau_i$ finchè $\tau_i$ non ha completato.
- Dimostrazione
	Se suppongo per assurdo che $\tau_k$ erediti una priorità $\ge$ della priorità di $\tau_i$ prima che $\tau_i$ completi, allora deve esistere un task $\tau_h$ a priorità maggiore ($P_h\ge P_i$) che è bloccato da $\tau_k$, cioè tale che $\tau_k$ eredita da $\tau_h$.
	Siccome $\tau_i$ è entrato nella CS allora $P_i>C^*$ dove $C^*$ è il massimo ceiling dei semafori bloccati da altri task a priorità minore. In questo momento ho $P_h\ge P_i>C^*$ e allora $\tau_h$ non può essere bloccato da $\tau_k$, e questo è assurdo.
##### Lemma 2
PCP previene il transitive blocking (situazione in cui i task si bloccano a cascata).
- Dimostrazione
	Supponiamo per assurdo che il transitative blocking succeda. Allora esistano i task $\tau_1$, $\tau_2$ e $\tau_3$ tali che $P_1>P_2>P_3$ e che $\tau_1$ è bloccato da $\tau_2$ e $\tau_2$ da $\tau_3$. In questa situazione $\tau_3$ eredita la priorità di $\tau_1$, e questo contraddice il [lemma 1](Priority%20ceiling%20protocol%20(PCP).md#Lemma%201).
##### Teorema 1
PCP evita il deadlock (situazione in cui tutti i task sono bloccati mentre aspettano una risorsa posseduta da altri).
- Dimostrazione
	Se avviene deadlock allora esistono i task $\tau_1$, $\tau_2$, $\cdots$ e $\tau_n$ tali che $P_1>P_2>\cdots>P_n$ e $\tau_1$ è bloccato da $\tau_2$ e $\tau_2$ da $\cdots$ $\tau_n$. In questa situazione $\tau_n$ eredita la priorità di $\tau_1$, e questo contraddice il [lemma 1](Priority%20ceiling%20protocol%20(PCP).md#Lemma%201).
##### Teorema 2
Sotto PCP un task può essere bloccato per al più la durata di una CS.
- Ci dice che il blocked time è bounded.
- È dovuto al fatto che il criterio di arresto è più restrittivo di [PIP](Priority%20Inheritance%20protocol%20(PIP).md).
- Dimostrazione
	Supponiamo che $\tau_i$ sia bloccato da $\tau_1$ e $\tau_2$ con $P_i>P_1>P_2$, che $\tau_2$ entri per primo nella CS e che $C_2^*$ sia il massimo ceiling tra i semafori usati da $\tau_2$.
	Se $\tau_1$ entra nella CS allora $P_1>C_2^*$ e se $\tau_i$ può essere bloccato da $\tau_2$ allora $P_i\le C_2^*$. Allora abbiamo $P_i\le C_2^*<P_1$ che contraddice l'ipotesi in cui $P_i>P_1$.
# CONSEGUENZE
### Vantaggi
- Il blocked time è limitato dalla durata di una CS.
- Evita il deadlock e il transitive blocking.
### Svantaggi
- È più complesso sia per lo scheduler, sia per il programmatore. Lo scheduler per capire il ceiling dovrebbe accedere al codice sorgente (cosa che non succede); è il programmatore che deve indicare allo scheduler il ceiling di ogni semaforo. Questo ovviamente comporta che il tutto sia error prone e che sia più complesso da manutenere (a ogni cambiamento di codice si deve rivedere il celing dei semafori).
- Si comporta in modo pessimistico, cioè blocca task anche se non sarebbe effettivamente necessario. Questa è una differenza con [PIP](Priority%20Inheritance%20protocol%20(PIP).md), che invece blocca solo quando è effettivamente necessario.