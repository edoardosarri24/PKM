In programmazione [parallela](parallel%20pragramming.md), ma in generale quando si programma, quando si valuta come strutturare e [decomporre i dati](decomposizione.md#Data%20decomposition) è importante anche valutare come memorizzarli. L'obiettivo di questo è cercare di sfruttare al massimo la cache della [memoria](memoria.md).
Ci sono due possibili e una loro combinazione: AoS e SoA. Con struct si intende il costrutto del C++ per un wrapper di oggetti complessi.
# Array of Structure
In AoS i dati vengono memorizzati in una matrice dove ogni riga contiene un oggetto e le colonne rappresentano i campi di questo oggetto. Possiamo vedere questo tipo di memorizzazione come allineato con l'Object Oriented.
##### Conseguenze
È una struttura particolarmente utile quando il nostro codice esegue operazioni (e.g., ciclo) su tutti o molti campi) di un oggetto alla volta. In questo modo l'oggetto, se non presente nei registri, sarà trovato in un'intera riga di cache di basso livello.
# Structure of Array
In SoA i dati vengono memorizzati in una matrice dove ogni riga contiene un campo di tutti gli oggetti e le colonne rappresentano gli oggetti stessi. Possiamo definire questo tipo di memorizzazione più allineato a un paradigma data oriented.
##### Conseguenze
È una struttura particolarmente utile quando il nostro codice esegue operazioni (e.g., ciclo) su un solo campo di molti oggetti. In questo modo l', se non presente nei registri, sarà trovato in un'intera riga di cache di basso livello.