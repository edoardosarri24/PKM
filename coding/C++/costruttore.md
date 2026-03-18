Un costruttore è un metodo con lo stesso nome della [classe](classi.md) che non ha valore di ritorno. Serve per creare un oggetto senza chiamare direttamente $\texttt{malloc}$, ma usando l'astrazione del [C++](C++.md).
##### Sintassi
La sintassi è:
- $\texttt{Classe nome\_oggetto(param)}$ se per il costruttore con parametri.
- $\texttt{Classe nome\_oggetto()}$ o $\texttt{Classe nome\_oggetto}$ per il costruttore senza parametri, cioè il costruttore di default.
##### Default
Se nessun costruttore è definito in una classe è il compilatore che ne crea uno senza argomenti e con un corpo vuoto.
# Inizializzazione
Lo scopo del costruttore è [inizializzare](istanziare%20oggetti.md#Pipeline) i campi degli oggetti. Questo in C++ si può fare in tre modi:
- Assegnazione
	Il classico modo usato anche in Java con l'operatore $\texttt{=}$.
- Lista di attributi
	I campi in questa lista devono seguire l'ordine della loro dichiarazione.
	La sintassi è $\texttt{Classe(tipo\_param nome\_param,...) : nome\_campo(nome\_param)}$.
- Inizializzazione uniforme
	Si inizializzano i campi con un valore di defualt direttamente nella loro dichiarazione. Questo è lo stile preferito rispetto a mettere il valore di default nella lista di attributi (cosa fattibile) perché
	La sintassi è $\texttt{tipo\_campo nome\_campo \{valore\_campo\}}$. Si usano quindi il valore di default all'interno di parentesi quadre.
# Costruttore esplicito
Per evitare di introdurre bug difficili da trovare a causa di comode conversioni esplicite che il compilatore esegue, si deve rendere un costruttore con un solo parametro esplicito.
##### Sintassi
La sintassi è $\texttt{explicit Classe(tipo\_param nome\_param)}$.
##### Problema risolto
In C++ quando il compilatore vede un costruttore con un singolo argomento lo vede come una funzione di conversione: prende in input un tipo e deve restituire un oggetto. Può capitare che il compilatore esegua una conversione automatica ma sbagliata e che non dovrebbe essere fatta: se abbiamo un metodo che prende un oggetto $\texttt{ContoCorrente}$ con un solo campo $\texttt{int saldo}$ potrebbe essere che il compilatore crea al volo un oggetto $\texttt{ContoCorrente}$ convertendo il nostro interno.
# Distruttore
È il metodo chiamato da $\texttt{delete}$ quando si deve liberare memoria allocata nell'[heap](istanziare%20oggetti.md#Dinamica). Non si deve più chiamare 
##### Sintassi
La sintassi è $\sim\texttt{Classe()}$.
##### Virtuali
Sicuramente non è necessario definire un costruttore [virtuali](ereditarietà.md#Virtuali)visto che questo sarà chiamato su un particolare tipo di oggetto.
I distruttori invece devono essere dichiarati come virtuali per tutte le classi che sono state pensate per essere ereditate. Questo permette di evitare memory leak nel caso in cui nella classe base ci sono altri campi: verrebbero distrutti solo i campi presenti nella classe base e non quelli nella classe derivata.
# Multipla
In C++ l'ereditarietà è multipla a differenza ad esempio di Java.
##### Problema del diamante
L'ereditarietà multipla porta al problema del diamante: nella [figura](Diamond_Inheritance_Problem.jpg) la classe $D$ eredita tutto ciò che è presente in $B$ e $C$; se $B$ e $C$ danno due specializzazioni diverse di un metodo e $D$ non ne da nessuna (cosa legittima per il linguaggio) allora $D$ non saprebbe quale eseguire.
##### Ereditarietà virtuale
L'ereditarietà virtuale elimina il problema del diamante facendo si la classe più specializzata scelga quale ramo del grafo seguire.
Questo deve essere fatto esplicitamente facendo l'override del metodo e richiamando la specializzazione della classe base desiderata.