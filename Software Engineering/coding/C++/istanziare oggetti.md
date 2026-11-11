In [C++](C++.md) dobbiamo istanziare gli oggetti che abbiamo definito.
Per creare e distruggere oggetti non si usano più direttamente $\texttt{malloc}$ e $\texttt{free}$, ma si utilizzano due nuove keyword, che non devono essere mescolati con $\texttt{malloc}$ e $\texttt{free}$.
- $\texttt{new}$
	Alloca la memoria necessaria per l'oggetto che stiamo allocando, calcolando in automatico tale dimensione; non dobbiamo quindi passare il numero di byte come con $\texttt{malloc}$.
	Se fallisce non restituisce un puntatore nullo, ma in questo caso si solleva un eccezione. Non è quindi necessario controllare il puntatore restituito.
- $\texttt{delete}$
##### Memory leak
Notiamo che comunque la memoria deve essere liberata automaticamente e che non è presente alcun garbage collector.
# Statica
È l'inizializzazione sullo [stack](processo.md#Stack) del processo.
Come in [C](C.md), la memoria nello stack è [allocata](allocazione.md) quando una varaibile viene definita, e viene liberata automaticamente quando il suo scope termina.
##### Sintassi
La sintassi è:
$\texttt{MiaClasse oggetto;}$
$\texttt{oggetto.faiQualcosa();}$
# Dinamica
È l'inizializzazione sull'[heap](processo.md#Heap) del processo.
Come in [C](C.md), la memoria nell'heap è [allocata](allocazione.md) e deallocata manualmente ed è usata quando volgiamo che la variabile sopravviva allo scope oppure quando è molto grande e non vogliamo rischiare di finire lo stack.
##### Pipeline
Quello che succede dietro le quindi è:
- Allocazione
	Il processo richiede al sistema operativo la memoria necessaria per contenere l'oggetto, calcolata tramite $\texttt{sizeof}$.
- Inizializzazione
	Il processo chiama il [costruttore](costruttore.md) della classe per inizializzare l'oggetto nella memoria appena allocata.
- Restituzione
	Restituisce l'indirizzo di memoria del primo byte dell'oggetto allocato.
##### Sintassi
La sintassi è:
$\texttt{MiaClasse* ptr = new MiaClasse();}$
$\texttt{delete ptr;}$