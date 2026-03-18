Una struttura [C](C.md) è una collezione di variabili a cui si può fare riferimento utilizzando il nome della struttura stessa. Aiutano a organizzare oggetti i cui membri (gli elementi che compongono la struttura) sono correlati.
# Dichiarazione
La dichiarazione è $\texttt{struct nome \{membro\_1,...\}; [nome\_var]}$:
- $nome$ identifica il tag della struttura, cioè una specie di tipo. Per riferirsi a questa struttura si deve usare $struct\ nome$.
- $nome\_var$ identifica il nome della variabile che è una struct. Se $nome\_var$ non è definito allora non stiamo allocando memoria per la struttura, ma solo dichiarando la struttura.
- Se usiamo anche $typedef$ con un nome dopo le parentesi ma prima del punto e virgola allora quel nome sarà l'alias e lo possiamo usare sempre anche senza $strcut$.
- Il nome dei membri deve essere unico solo all'interno della struttura: è come se fossero variabili locali della struttura.
- Un membro di una struttura può essere a sua volta una struttura.
##### Ricorsione
Quando si ha una struttura ricorsiva $st$, cioè dove uno o più membri sono dello stesso tipo della struttura $st$ (e.g., alberi), allora quei membri non possono essere direttamente definito del tipo $st$, ma devono essere sei puntatori a $st$.
Il motivo di questo è che definirebbe una ricorsione infinita. In particolare quando il compilatore va ad allocare spazio in memoria e calcola $size(st)$ questo include una nuova $st$ e quindi lo spazio è infinito; invece $size(puntatore\_st)$ è solitamente 2 o byte.
# Accesso
Definita una struttura possiamo accedere ai membri usando il nome della variabile che contiene la struttura e il punto tramite $nome\_struct.membo$.
# Puntatori
Spesso quando si passa una struttura a una funzione si passa il suo puntatore; questo è più efficiente perché si evita di copiare tutta la struttura in un'altra variabile.
##### Accesso
Se $pp$ è il puntatore alla struttura allora $*pp$ è la struttura e si può accedere ai membri come $(*pp).membro$ (parentesi necessarie perché $.$ ha una precedenza maggiore di $*$.
In alternativa possiamo usare $pp->membro$.
# Flexible Array Member
È una tecnica del molto potente introdotta in [C99](C.md).
Si tratta dichiarare un campo della struttura come $\texttt{uint8\_t data[]}$ (o un altro nome) che è un vettore senza dimensione e che funge da segnaposto, cioè da puntatore al byte immediatamente successivo all'ultimo campo.
È come se stessi considerando gli altri campi della struct come metadati e i dati effettivi sono nella memoria puntata da $\texttt{data}$.
##### Allocazione
Per allocare questa memoria puntata da $\texttt{data}$ si fa $\texttt{Type blocco = malloc(sizeof(Type) + memoria\_desiderata);}$.