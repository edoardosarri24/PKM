 L'ereditarietà permette il facile riuso del codice: la classe derivata eredita le funzionalità (campi e metodi) della classe base senza dover duplicarne il codice.
Quando una classe $A$ eredita da una base $B$, stiamo dicendo che $A$ $è\_un$ $B$.
# Sintassi
La sintassi, se $\texttt{B}$ è la classe base è $\texttt{class A [final]: modificatore B}$, dove:
- $\texttt{modificatore}$ è il modificatore di accesso ai membri della classe base.
- $\texttt{final}$ indica che la classe non può essere derivata.
# Modificatori
In Java è possibile diminuire la visibilità dei membri della classe base eseguendone l'overriding usando un modificatore meno visibile.
Notiamo che in ogni caso la classe derivata può vdere solo i membri che non sono $\texttt{private}$.
In C++ questo può essere fatto nella dichiarazione con tre modificatori:
- $\texttt{public}$
	Tutti i membri pubblici o protected della classe base sono pubblici per la classe ereditata.
- $\texttt{protected}$
	Tutti i membri pubblici o protected della classe base sono protected per la classe ereditata.
- $\texttt{private}$
	Tutti i membri pubblici o protected della classe base sono privati per la classe ereditata.
# Binding
In Java la scelta della firma è fatta staticamente, la scelta di quale (i.e., quale specializzazione) metodo eseguire è fatta in modo dinamico eseguendo sempre la versione più specializzata.
In [C++](C++.md) di dafault la ricerca del metodo è statica: se viene chiamato un metodo si un'istanza di una classe base che a run-time è del tipo di una sua classe derivata che specializza quel metodo allora verrà eseguito comunque quello della classe base.
Il motivo di questa scelta sono le performance: in C e C++ non si paga quello che non si usa. La ricerca del metodo è costosa e quindi se non ci serve non viene implementata.
##### Virtuali
Si può ridefinire questo comportamento, permettendo al client di usare il metodo più specializzato senza eseguire cast espliciti, si possono usare i metodi virtuali.
Si deve aggiunger $\texttt{virtual}$ nella dichiarazione del metodo nella classe base.