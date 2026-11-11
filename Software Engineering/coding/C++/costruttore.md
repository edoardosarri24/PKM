Un costruttore è un metodo con lo stesso nome della [classe](classi.md) che non ha valore di ritorno. Serve per creare un oggetto senza chiamare direttamente $\texttt{malloc}$, ma usando l'astrazione del [C++](C++.md).
##### Sintassi
Quando si chiama il costruttore abbiamo varie possibilità:
- Senza parametri
	$\texttt{Classe nome\_oggetto\{\};}$ o $\texttt{Classe nome\_oggetto;}$ per il costruttore senza parametri, cioè il costruttore di default. Se facessimo $\texttt{\texttt{Classe nome\_oggetto();}}$ allora stimao dichiarando una funzione che non prende parametri e ritorna un oggetto di tipo $\texttt{nome\_oggetto}$.
- Con parametri
	Si usa  $\texttt{Classe nome\_oggetto(param)}$.
La sintassi è:
- $\texttt{Classe nome\_oggetto(param)}$ per il costruttore con parametri.
- $\texttt{Classe nome\_oggetto()}$ o $\texttt{Classe nome\_oggetto}$ per il costruttore senza parametri, cioè il costruttore di default.
##### Default
Se nessun costruttore è definito in una classe è il compilatore che ne crea uno senza argomenti e con un corpo vuoto.
# Spazio di memoria
Il costruttore di per se serve solo per allocare spazio in memoria. A seconda di come lo usiamo possiamo allora memoria nello stack o nello heap.
Nel caso in cui si utilizzi la object composition, allora l'area di memoria dell'oggetto contenuto è la stesso dell'oggetto contenitore.
##### Stack
Se in una funzione facciamo $\texttt{MyClass object;}$ allora stiamo allocando spazio nello stack.
In questo caso quando la variabile $\texttt{object}$ esce dal suo scope il distruttore viene chiamato automaticamente.
##### Heap
Quando facciamo $\texttt{MyClass* object = new MyClass();}$ allora viene allocata memoria nello heap.
In questo caso il distruttore viene chiamato solo quando di chiama esplicitamente $\texttt{delete object}$.
Se vogliamo alloracare memoria dinamicamente non si utilizza più $\texttt{new}$ e $\texttt{delete}$, ma si usa la [RAII](RAII.md) con i [puntatori smart](puntatori%20smart.md).
# Inizializzazione
Lo scopo del costruttore è [inizializzare](istanziare%20oggetti.md#Pipeline) i campi degli oggetti. Questo in C++ si può fare in tre modi:
- Assegnazione
	Dentro il corpo del costruttore si usa l'operatore $\texttt{=}$ come in Java.
- Lista di attributi
	Il corpo del costruttore si occupa di ulteriori operazioni che devono essere eseguite dopo la costruzione dell'oggetto.
	I vantaggi sono: l'ordine di inizializzazione dei campi è quello dichiarati nel file $\texttt{.hpp}$; i campi vengono allocati e inizializzati nello stesso momento.
	Quando si implementa il costruttore facciamo $\texttt{Classe(tipo\_param nome\_param,...) : nome\_campo(nome\_param)}$.
- Inizializzazione uniforme
	Si inizializzano i campi con un valore di defualt direttamente nella loro dichiarazione. Questo è lo stile preferito rispetto a mettere il valore di default nella lista di attributi (cosa fattibile) perché
	La sintassi è $\texttt{tipo\_campo nome\_campo \{valore\_campo\}}$. Si usano quindi il valore di default all'interno di parentesi quadre.
# Costruttore esplicito
Per evitare di introdurre bug difficili da trovare a causa di comode conversioni esplicite che il compilatore esegue, si deve rendere un costruttore con un solo parametro esplicito.
La sintassi è, quando si dichiara il costruttore, $\texttt{explicit Classe(tipo\_param nome\_param)}$.
##### Problema risolto
In C++ quando il compilatore vede un costruttore con un singolo argomento lo vede come una funzione di conversione: prende in input un tipo e deve restituire un oggetto. Può capitare che il compilatore esegua una conversione automatica ma sbagliata e che non dovrebbe essere fatta: se abbiamo un metodo che prende un oggetto $\texttt{ContoCorrente}$ con un solo campo $\texttt{int saldo}$ potrebbe essere che il compilatore crea al volo un oggetto $\texttt{ContoCorrente}$ convertendo il nostro interno.
# Distruttore
Il distruttore è quel metodo chiamato quando si deve liberare la memoria allocata. Se l'oggetto è allocato nello stack allora viene chiamato automaticamente quando l'oggetto esce dal suo contesto; se invece è allocato nello stack (e senza l'uso di [RAII](RAII.md)) allora si deve chiamare $\texttt{delete object}$.
##### Scopo
Per gli oggetti passati per puntatore, quindi dove il campo è qualcosa del tipo $\texttt{Type* field}$, allora il distruttore di default distrigge solo il puntatore ma non libera la memoria in cui risiede l'oggetto.
##### Virtuali
Sicuramente non è necessario definire un costruttore [virtuale](ereditarietà.md#Virtuali) visto che questo sarà chiamato su una classe concreta.
I distruttori invece devono essere dichiarati come virtuali per tutte le classi che sono state pensate per essere ereditate, quindi per le classi base. In queste situazioni infatti l'ordine di chiamata del distruttore va dalla classe più specializzata alla classe più astratta; senza il $\texttt{virtualize}$ sarebbe chiamato solo il distruttore del tipo della dichiarazione dell'oggetto (i.e., la classe base).
# Multipla
In C++ l'ereditarietà è multipla a differenza ad esempio di Java.
##### Problema del diamante
L'ereditarietà multipla porta al problema del diamante: nella [figura](Diamond_Inheritance_Problem.jpg) la classe $D$ eredita tutto ciò che è presente in $B$ e $C$; se $B$ e $C$ danno due specializzazioni diverse di un metodo e $D$ non ne da nessuna (cosa legittima per il linguaggio) allora $D$ non saprebbe quale eseguire.
##### Ereditarietà virtuale
L'ereditarietà virtuale elimina il problema del diamante facendo si la classe più specializzata scelga quale ramo del grafo seguire.
Questo deve essere fatto esplicitamente facendo l'override del metodo e richiamando la specializzazione della classe base desiderata.