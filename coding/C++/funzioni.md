In [C++](C++.md) di default gli argomenti sono passati per valore, cioè la variabile della funzione ottiene una copia della variabile del chiamante.
# Passaggio per riferimento
Gli argomenti possono essere passati anche per riferimento e per farlo si usano i [riferimenti](riferimenti.md): l'argomento della funzione, se è un riferimento, è direttamente la variabile stessa, e non una sua copia.
Rispetto ai puntatori questo è molto più semplice perché non si deve deferenziare nulla (con l'operatore $\texttt{*}$).
##### Efficienza
Il fatto che non si debba copiare il valore della variabile del chiamante permette di guadagnare molta efficienza.
##### Sintassi
La sintassi è $\texttt{<tipo\_ritorno> nome (<tipo\_param>\& nome\_param)}$.
# Parametri di default
Le funzioni del C++ hanno la stessa caratteristica del Python: permettono ai parametri di avere dei valori di default.
##### Sintassi
- Devono essere alla fine di tutti i parametri.
- Si possono usare funzioni come valori di default.
# Funzioni libere
Spesso abbiamo funzioni di utilità che non hanno senso di appertenere a una classe. In C++ non abbiamo il singolo paradigma a oggetti di Java e spesso in queste situazioni l'approccio della programmazione procedurale del [C](C.md) è più opportuna.
Quello che possiamo fare in più con il C++ è definire una funzione libera all'interno di un [namespace](C++.md#Namespace), in modo da darle comunque un contesto ma non doverla dichiarare come statica in una qualche classe.