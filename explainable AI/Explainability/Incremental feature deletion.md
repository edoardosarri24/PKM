![incremental featurs deletion](incremental%20featurs%20deletion.jpeg)
È una tecnica di tipo [outcam explaination](Metodi.md#Outcam%20explaination) usata nel campo dell'[ixplainability](Explainability.md).
La tecnica si basa sull'eliminazione incrementale delle features e osservare il cambiamento della qualità della previsione del modello.
# Grafico
Le curve principali del grafico sono:
- La curva rossa rappresenta la variazione delle prestazioni (valutate secondo un qualche indice di sicurezza della risposta del modello) rispetto al numero di features mascherate in modo casuale; la curva blu rispetto al numero di features mascherate in ordine di importanza stabilito da un qualche algoritmo di XAI. Quello che mi aspetto è che rimuovendo le feature secondo il ranking la qualità delle predizioni si abbassi più velocemente che rimuovendole in modo casuale; più l'area definita da queste due curve, detta correctness, è ampia più la spiegazione che mi ha dato il modello (cioè quali sono le features più rilevanti) è soddisfacente.
- La curva verde è la soglia in cui la decisione del classificatore cambia (solitamente è decisa dall'utente). Se $\bar{n}$ è l'ascissa dove questa curva intercetta la curva blu, allora $\bar{n}$ rappresenta il numero di features che devono essere rimosse perché il modello cambia decisione. Più questo valore, detto compactness, è piccolo più le features importanti sono poche.
- Il valore $n^*$, detto out completeness, è il numero di tutte le features importanti; solitamente è difficile da determinare. Se $n^*>\bar{n}$ allora vuol dire che il modello cambia predizione prima di aver mascherato tutte le features importanti. Se siamo nel caso opposto, quello in cui il modello cambia prima predizione, allora possiamo dire che l'insieme delle features importanti identificate dall'algoritmo di XAI non ci soddisfa, perché vuol dire che nonostante aver mascherato tutte le features importanti il modello non cambia predizione.
# Features mask
Ci sono vari modi per mascherare le features.
- Usare altri valori.
	Il modo giusto per farlo non è usare valori casuali, ma campionare altri valori dalla distribuzione del training set. In questo modo se la predizione cambia allora quelle features che erano state dichiarate come importanti lo sono veramente.
	Se non campioniamo il valore dalla distribuzione del dataset si ha il problema dell'[OOD](Incremental%20feature%20deletion.md#Out%20of%20distribution).
- Usare 0.
	Siamo nella situazione precedente, ma il valore che usiamo è costante e non campionato dalla distruzione del dataset e quindi si ha il problema dell'[out of distribution](Explainability.md#Out%20of%20distribution). Inoltre 0 potrebbe essere un valore rilevante per il dominio.
- Elimando il vettore che le rappresenta.
	Il problema è che il modello (la maggior parte dei modelli) non funziona più (e.g., la dimensione dell'input che passo a un MLP é fissato) e va quindi riaddestrato. Si devono quindi confrontare i risultati di due modelli diversi, addestrati su dati diversi.
# Out of distribution
Questo problema è critico nei vari modelli: finché i sample che si presentano hanno una distribuzione delle feature simile a quella del training set allora il modello funziona; valutare un sample campionato da una distribuzione diversa è un tallone di Achille per quasi ogni modello.
Questo è dovuto all'overfitting, che sarà sempre un po' presente: classificare un nuovo sample è tanto più difficile quanto questo è diverso da quelli su cui il modello è stato addestrato.