La responce time analysis (RTA) è un metodo di analisi della schedulabilità necessario e sufficiente utilizzato nei sistemi [real time](real%20time.md) per determinare se ogni [task](Real%20time/task.md) di un sistema soddisfa i propri vincoli temporali.
Rispetto ai test di schedulabilità basati sull'utilizza della CPU (e.g.,) la RTA ci fornisce un uppur bound del tempo di risposta nel caso pessimo. Se questo tempo risposta è inferiore alla deadlinea allora il taskset è schedulabile.
# Teorema
Il teorema su cui si basa la responce time analysis è il seguente:
> Sotto uno uniprocessor fixed-priority preemptive scheduling, se la lunghezza dell'intervallo della [zona critica](istante%20critico.md#Zona%20critica) di un task $\tau_{k}$​ non è superiore al suo periodo $T_{k}$​, allora il tempo di risposta del job di $\tau_{k}$​ rilasciato in quel momento rappresenta il worst-case responce time per $\tau_{k}$​. In caso contrario, il worst-case responce time di $\tau_{k}$​ è maggiore di $T_{k}$.​
# Utilizzo
- Taskset con [task periodici](Real%20time/task.md#Periodico) ma non puramente periodici, cioè quando la deadline relativa è minore del periodo.
- Taskset con [task sporadici](Real%20time/task.md#Aperiodico).
- Situazioni di [probabilistic timing analysis](probabilistic%20timing%20analysis.md), dove il tempo di esecuzione è definito da una distribuzione.
##### Confronto
Con task periodici (non puramente) utilizzare i test di schedulabilità basati sull'utilizza della CPU, sostituendo i periodi con le deadline relative, porterebbe a un'analisi [troppo conservativa](LL%20and%20HB%20are%20only%20necessary.png): i test potrebbero non passare ma, essendo necessari e non sufficienti, anche se il taskset è schedulabile.
# Algoritmo
L'[algoritmo](response%20time%20analysis.png) parte dal task a priorità maggiore, ma si potrebbe partire da uno a scelta, visto che deve essere soddisfatto per ogni task.
Per ogni task $\tau_i$:
- Calcolare l'interferenza $I_i$ subita da $\tau_i$ a causa dei task a priorità più alta nel tempo $[0,R_i]$ (dove $R_i$ è il response time del task $\tau_i$).
	$I_i=\sum_{\tau_k|D_k<D_i}z_{ik}C_k=\sum_{k=1}^{i-1}\lceil\tfrac{R_i}{T_k}\rceil C_k$, dove i task sono ordinati per deadline relativa crescente.
	- $D_k<D_i$ ci dice che $\tau_i$ ha priorità maggiore di $\tau_k$.
	- $z_{ik}$ è il numero di rilasci del task $\tau_k$ nell'intervallo $[0,R_i]$. Per questo motivo si prende poi l'interno superiore, perché si vuole calcolare tutti i rilasci, anche quelli il cui periodo non è terminato.
	- Al primo passo dell'algoritmo l'interferenza è pari a un job per ogni task a priorità più alta.
- Calcolare il response time $R_i$.
	$R_i=C_i+I_i=C_i+\sum_{k=1}^{i-1}\lceil\tfrac{R_i}{T_k}\rceil C_k$.
	- Notiamo che non c'è una forma chiusa per $R_i$ ma lo si calcola in modo iterativo.
	- Notiamo inoltre che $R_i$ non è il vero tempo di risposta, ma cresce a ogni iterazione (ci si ferma quando non cresce più). Se trovo un tempo di risposta più piccolo per cui il task sfora la deadline mi basta per dire che il task (e quindi il task set) non schedulable.
- Se $R_i>D_i$ allora $\gamma$ non è schedulabile, altrimenti si continua con le iterazioni sul task in esame e ci si arresta quando il response time non cresce più.
##### Complessità
Ha complessità pseudo polinomiale $\mathcal{O}{(n\times N)}$, dove $n$ è il numero di task e $N$ è il numero delle iterazioni per task.
##### Esempi
- [Unfeasible](unfeasible%20example%20RTA%20example%20with%20DM.pdf)
- [Feasible](feasible%20RTA%20example%20with%20DM.pdf)