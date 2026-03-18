È un [ILP system](ILP%20system.md) che estende Popper e che permette il ragionamento numerico, cioè di considerare nei predicati valori appartenenti a un dominio infinito (e.g., reali e interi).
Per l'apprendimento Numsynth utilizza il [LFF](Inductive%20Logic%20Programming%20(ILP).md#Leaning%20From%20Failure%20(LFF)) la cui funzione di costo è definita è la dimensione della dell'ipotesi $H$, cioè il numero di [letterali](Programmazione%20logica.md#Sintassi) in essa.
# Vantaggi
- Permette il ragionamento numerico.
- Permette di apprendere concetti che altri strumenti non imparano.
- Scala in modo (non strettamente) lineare rispetto al numero di esempi. Questo dipende dalla complessità del ragionamento numerico.
# Implementazione apprendimento
Il sistema segue due fasi:
- Program search
	Il sistema apprende un insieme di regole parziale seguendo la stessa struttura di Aleph; questi sono detti partial program.
	In questa prima fase i valori numerici sono sostituiti con variabili numeriche e nella regola compare il predicato unario $@numerical(A)$. In questo momento la regola ancora non deve soddisfare un bound definito dal valore numerico.
	I letterali che contengono le variabili numeriche sono detti letterali numerici. Una variabile (non numerica) che compare sia in un letterale numerico che un letterale normale è detta related variables.
- Numerical search
	Durante questa fase il sistema costruisce una formula Satisfiability Modulo Theories (SMT) (come SAT ma con operazioni, cioè non solo con operatori booleani) e sostituisce ogni possibile valore che si trova nel training set relativo alle variabili numeriche.
	Se la formula SMT creata è soddisfacibile, allora ogni sostituzione alla variabile numerica che la soddisfa è una soluzione dell'input LFF.
# Letterali numerici
I letterali numerici che sono built-in sono: $A\ge N$, $A\le N$, $A+B=C$ e $A*N=B$.
È importante notare che $add$ non include valori che devono essere scoperti ($N$ negli altri sopra): si deve sapere che i valori non saranno scoperti e quindi validi per tutti gli esempi (perché ogni $N$ è sostituita con un solo valore preso dal train set), ma fisso e non variabile.
##### Non linearità
Per problemi di complessità non sono permessi letterali non lineari.