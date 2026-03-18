Il PCY algorithm è un'ottimizzazione dell'[A-Priory algorithm](A-Priory%20algorithm.md) e nasce dall'osservazione che al primo passo dell'algoritmo la memoria utilizzata è molto poca rispetto a quella delle iterazioni successiva; in pratica memorizziamo solo la lista di tutti gli item con i relativi contatori e solamente al secondo passo si usa la restante memoria allocata.
##### Obiettivo
Si vuole ottimizzare il primo e secondo passaggio di A-Priori, cioè si vuole generare in modo più efficiente (rispetto all'uso della memoria) l'insieme dei candidati 2-itemset $C_2$.
Rispetto ad A-priory si arriva ad avere una cardinalità di $C_2$ inferiore.
# Passi
Per generare $C_2$ si fanno due passi distinti in [questo](PCY.png) modo.
##### Passo 1
- Alloco in memoria la lista di item con un contatore inizializzato a 0. Alloco anche una tabella hash con tanti elementi quanti mi stanno in memoria.
- Per ogni transaction che arriva:
	- Per ogni item della transaction incremento il relativo contatore.
	- Per ogni coppia di item calcolo la relativa funzione hash e incemento il relativo contatore.
##### Passo 2
- Elimino dalla lista degli item quelli che non sono frequenti, cioè il cui support count è minore della soglia.
- Trasforma l'hash map in una bit map dove: se il support count della entry è maggiore della soglia allora il bti 1; altrimenti è 0.
- Per ogni transaction genero nuovamente ogni possibile coppia. Una coppia diventa una candidata in $C_2$ se soddisfa due condizioni:
	- Entrambi gli item che forma la coppia sono frequent item. E questa è la condizione che valuta anche [A-Priory](A-Priory%20algorithm.md).
	- Il valore del suo hash mi porta a una posizione della bit map con valore 1.
# Multistage
L'idea nasce quando l'insieme delle candidate coppie frequenti non sta in memoria e quindi si deve diminuire la sua cardinalità. Non si usano due passi ma se ne usa tre in [questo](PCY%20multistage.png) modo.
- Il primo passo è uguale al caso normale.
- Al secondo passo trasformo la prima tabella hash in una mappa. Inoltre scorro ancora tutte le transaction e costruisco un'altra tabella hash per ogni coppia di item di ogni transaction indicizzata da una funzione diversa, memorizzando il relativo contatore.
- Al terzo passo si trasfroma la secondo tabella hash in una bitmap. Le coppie candidate adesso devono essere composte da item frequenti e il loro hash deve portare a una posizione il cui bit è 1 in tutte e due le bitmap.
##### Osservazioni
Usando due tabelle hash indicizzate da funzioni diverse ho meno collisioni (o comunque collisioni diverse) e quindi avrò meno falsi positivi; in pratica avrò meno possibilità che l'aggregazione di più non frequent item mi dia un contatore maggiore della soglia due volte.
##### Costo
Il costo è calcolato tramite il numero di accessi al disco. Il disco è usato ogni volta che si fa un passaggio su tutti i dati; in questo caso si fa un passaggio in più e quindi si ha un costo maggiore.
# Multihash
L'idea è quella di usare il metodo multistage in un solo passaggio in [questo](PCY%20multihash.png) modo.
In pratica al primo passaggio si creano due tabelle hash indicizzate con tutte le coppie di item possibili per ogni basket e che usano due funzioni hash diverse.
##### Osservazioni
Si vogliono avere gli stessi vantaggi del metodo multistage ma con un passaggio sui dati in meno.