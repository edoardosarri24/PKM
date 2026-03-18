Un processo, cioè un programma in esecuzione, è formato da un insieme di task. Un task è un concetto logico: è una sequenza di istruzioni che, in assenza di altre attività, viene eseguita dalla CPU senza interruzioni. Il corrispettivo fisico, cioè l'implementazione del task è il [thread](thread.md).
Un task è caratterizzato da un comportamento funzionale che dipende dall'input dato al task. Tale comportamento può essere: deterministico, se dato un input l'output è sempre lo stesso; non deterministico, se l'output non dipende solo dall'input (e.g., una ricerca randomica).
# Definizioni
Dato il task $i-esimo$ $\tau_i$:
- Activation time
	Tempo $a_i$ nel quale il task diventa pronto per l'esecuzione. Si indica con una freccia blu verso l'alto nella timeline. È detto anche tempo di rilascio.
- Start time
	Tempo $s_i$ nel quale il task inizia l'esecuzione per la prima volta.
- Finishing time
	Tempo $f_i$ nel quale il task termina l'esecuzione.
- Computational time
	Tempo $C_i$ complessivo di esecuzione senza interruzioni. Solitamente, siccome parleremo sempre del caso pessimo, questo prenderà il nome di Worst case Execution Time (WCET).
- Completion time
	Tempo $K_i=f_i-s_i$ che passa dallo start alla fine dell'esecuzione.
- Response time
	Tempo $R_i=f_i-a_i$ che passa dall'attivazione alla fine dell'esecuzione.
- Jitter
	In generale è la misura della variazione di un evento periodico.
	- Start time jitter
		- Assoluto
			È la massima deviazione dell'inizio tra tutti i job, cioè $max_k\{s_{ik}-a_{ik}\}-min_k\{s_{ik}-a_{ik}\}$.
		- Relativo
			È la massima deviazione dell'inizio tra due job consecutivi, cioè $max_k\{|(s_{ik}-a_{ik})- (s_{i,k-1}-a_{i,k-1})|\}$.
	- Finishing time jitter
		È uguale al primo sostituendo $s$ con $f$.
	- Completion time jitter
		È uguale al secondo sostituendo $a$ con $s$.
# Parametri
Sono i valori che servono al task per eseguire. Possono essere:
- Offline
	Sono i parametri del task conosciuti e specificati dal programmatore:
	- Il periodo $T_i$, nel task periodici.
	- Il computational time $C_i$, che si valuta in base al codice scritto.
	- La deadline relativa $D_i$.
- Online
	Sono i parametri conosciuti solo una volta che il tutto è in esecuzione:
	- L'activation time $a_i$.
	- La laxity $X_i$ e la laxity relativa $Y_i$.
	- La lateness $L_i$ e la tardiness $E_i$.
	- I vari jitter.
# Real-time
Un task real-time, oltre a essere definito da un comportamento funzionale che dipende dall'input, ha anche un comportamento non funzionale definito da un vincolo sul tempo di risposta, cioè che deve finire entro una certa deadline.
La deadline del task $\tau_i$ può essere assoluta, cioè $d_i$ considerata dal tempo iniziale, o relativa, cioè $D_i=d_i-a_i$ calcolata dal tempo di attivazione del task.
- Un task real-time $\tau_i$ è $feasible$ se completa la sua esecuzione prima della deadline assoluta, cioè se $f_i\le d_i$ oppure $R_i\le D_i$.
- La $laxity$ (o slack time) $X_i$ del task $\tau_i$ è il tempo in cui il task è stato attivo ma non in esecuzione, cioè è $X_i=D_i-C_i$. La residual laxity $Y_i$ è la laxity misura dal tempo di completamento, cioè $Y_i=d_i-f_i$, ovvero quando prima della dealine il task finisce il suo compito.
- La lateness $L_i$ è il ritardo rispetto alla deadline, cioè $L_i=f_i-d_i$; un valore negativo indica che il task è finito entro al deadline. Per valutare se il task ha superato la deadline si può definire la tardiness, cioè $E_i=max\{0,L_i\}$.
# Jobs
Le diverse esecuzioni dello stesso task su dati diversi (e.g., sensore che misura la temperatura periodicamente) è detto task instance o jobs.
##### Periodico
Si parla di time-driven task e l'attivazione è causata da un timer.
Dato il task $\tau_i$ è una sequenza infinita di jobs $\tau_{i1},\cdots,\tau_{ik},\cdots$ che si attivano periodicamente con periodo $T_i$.
- Se $T_i=D_i$, cioè se la dealined del job in corso coincide con l'attivazione del job successivo, allora il task si dice puramente periodico.
- Il fattore di utilizzo si misura come $U_i=C_i/T_i$ ed è la percentuale di tempo in cui il task ha usato la CPU.
- Dato il periodo $T_i$, il tempo di computazione $C_i$ e $la$ deadline realtiva $D_i$, allora il tempo di attivazione del primo job è $a_{i1}$, il tempo di attivazione del job $k-esimo$ è $a_{ik}=a_{i1}+(k-1)T_i$ e la deadline assoluta del job $k-esimo$ è $d_{ik}=a_{ik}+D_i$.
- Per l'[implementazione](periodic%20task%20implementation.png) serve un OS che metta a disposizione primitive opportune. È compito del programmatore assicurarsi che il corpo del task possa essere svolto in un tempo minore del periodo; se così non fosse una volta arrivato alla fine rinizierebbe subito (il prossimo periodo sarebbe già arrivato), ma si perderebbero job. Dalla chiamata $wait\_for\_the\_next\_period$ all'inizio del nuovo periodo, il task è nello stato [idle](Real%20time/task.md#Idle).
##### Aperiodico
Di parla data-driven task e l'attivazione è causata da un evento.
Dato il task $\tau_i$ è una sequenza infinita di jobs $\tau_{i1},\cdots,\tau_{ik},\cdots$ che non sono periodicamente attivati.
Un task aperiodico si dice sporadico quando due suoi job consecutivi sono separati almeno da un minimum inter-arrival time $T_i$, cioè quando $a_{i,k+1}\ge a_{ik}+T_i$ in ogni caso.