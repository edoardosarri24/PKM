Sono le strutture dati classiche della [STL](STL.md)
Ogni contenitore gestisce la memoria necessaria per mantenere i suoi elementi.
# Omogeneità
Solitamente collezionano dati omogenei.
Se abbiamo bisogno di collezionare dati eterogenei possiamo definire una collezione di puntatori alla classe base di tali elementi.
# Inserimento
Quando si inserisce un elemento in un contenitore in realtà si inserisce una sua copia. Dobbiamo quindi accertarci che il tipo contenuto dal contenitore metta a disposizione un costruttore di copia abbastanza profondo.
# Classificazione
##### Sequenze
Gli elementi hanno una posizione relativa. I più usati sono $\texttt{vector<T>}$, $\texttt{deque<T>}$ e $\texttt{list<T>}$.
- Vector
	È un vettore, cioè una porzione di memoria contigua.
	Gestisce la memoria in modo automatico: quando è pieno sposta tutti gli elementi in un nuovo vettore (che si troverà in un'altra area di memoria) più grande; questo spostamento, che è costoso, solitamente avviene poche volte perché la dimensione cresce in modo esponenziale. Per risolvere questo si può chiedere di allocare una determianta quantità di memoria.
##### Associativi
Associano un valore a un elemento. I più usati sono $\texttt{set<K>}$, $\texttt{map<K,V>}$, $\texttt{multiset<K>}$ e $\texttt{multimap<K,V>}$. Esistono anche le loro versioni non ordinate, che sono implementate con hash table invece che con alberi.
##### Adattatori
	Sono le stesse strutture dati ma adattate a casi specifici. Sono esempi $\texttt{stack}$ e $\texttt{queque}$.