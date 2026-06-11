Un taskset $\Gamma=\{\tau_1,\cdots,\tau_{n}\}$ è un insieme di [task](Real%20time/task.md).
# Generazione
Quando vogliamo verificare che una data priorità sia verificata su un taskset quello che solitamente si fa è generare un grande numero di taskset e osservare il rapporto tra quelli che hanno verificato tale priorità e quelli che sono feasible per quel problema; nel caso del [real time](real%20time.md) si considerano tutti quelli che sono schedulabili, cioè quelli che hanno un fattore di utilizzo $U \leq 1$.
##### Execution time
Visto che il tempo di esecuzione è molto variabile perché dipende da fattori di basso livello e dallo stato del sistema, spesso senza conoscenze a priori si modella con una distribuzione uniforme.
Dati che $U_{i}$ è il fattore di utilizzo dell'$i$-simo task $\tau_{i}$ e $T_{i}$ il tempo di rilascio deterministico del task, abbiamo due contesti:
- Deterministico
	Il tempo di esecuzione deterministico è definito da $C_{i}=U_i\cdot T_{i}$.
- Stocastico
	Se vogliamo che il tempo di esecuzione sia una variabile aleatoria deterministica allora possiamo definirla come $C_{i}=\mathcal{U}(0,T_{i})$.
##### Periodo
Il tempo di rilascio di un task è gestito dal sistema operativo e solitamente solo vicino a essere deterministico; per questo motivo trattarlo come una distribuzione uniforme non rispecchia la realtà. In ogni caso, se non abbiamo conoscenza a priori del tempo di rilascio dobbiamo comunque modellarli con una qualche distribuzione.
##### Fattore di utilizzo
Vedi [paper](Measuring_the_Performance_of_Schedulability_Tests.pdf).
Spesso è utile generare un fattore di utilizzo della CPU che abbia un valore cumulativo $\bar{U}$, i.e, $\sum_{i}U_{i}=\bar{U}$; vorremmo inoltre che ogni $U_{i}$ sia generato uniformemente in $[0,\bar{U}]$.
Generare questi fattori di utilizzo in modo scorretto porta a produrre parametri dei task non validi e non generalizzabili.
- UScalingAlgorithm
	Si campiona $n$ fattori di utilizzo $U_{i}$ da $\mathcal{U}(0,\bar{U})$ e poi si scala ognuno di essi per un fattore $\frac{\bar{U}}{\sum_{i}^nU_{i}}$.
	- Complessità $\mathcal{O}(n)$.
	- Si generano fattori di utilizzo non uniformi.
- UFittingAlgorithm
	Si campiona $U_{1}$ da $\mathcal{U}=[0,\bar{U}]$; si campiona dopo $U_{2}$ da $\mathcal{U}(0,\bar{U}-U_{1})$; si continua così fino a $U_{n}$ campionato da $\mathcal{U}( 0,\bar{U}-\sum_{i=1}^{n-1}{U_{i}})$.
	- Complessità $\mathcal{O}(n)$.
	- Si generano fattori di utilizzo non uniformi.
- UUniformAlgorithm
	Genera fattori di utilizzo totalmente uniformi ma ha una complessità $\mathcal{O}((n-1)!)$.
- UFastAlgorithm
	Si basa sul fatto che la PDF della somma di random variable uniformi è ottenibile tramite [convoluzione](matematica/polinomi/convoluzione.md) e il risultato è una spline.
	- Complessità $\mathcal{O}(n)$.
	- Si generano fattori di utilizzo perfettamente uniformi.