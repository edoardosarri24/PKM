Un algoritmo standard opera su dati interamente presenti in ram. Quando però i dati non stanno tutti in memoria principale si devono cambiare gli algoritmi e le osservazioni sulla complessità.
##### Complessità
Negli algoritmi standard il costo derivava dal numero di confronti o di operazioni in memoria.
Quando valutiamo un algoritmi che opera su un [disco](disco%20di%20memoria.md) (o su più dischi, eventualmente distribuiti) si deve considerare il costo di accesso al disco (cioè quante volte portiamo i dati dal disco alla memoria); quante operazioni poi facciamo con i dati che abbiamo portato in memoria è trascurabile.
# Strutture dati
Vediamo due strutture dati utili quando i dati non stano nella ram.
##### External hash
Quando usiamo una tabella hash in un algoritmo normale, ogni elemento della tabella è un valore; se abbiamo una collisione e usiamo le catene per la memorizzazione allora ogni elemento della tabella è una catena di valori.
Nella versione delle hash table usate per i dischi ogni elemento è un bucket, cioè un insieme di valori che sta sul [disco](disco%20di%20memoria.md) (in pratica un settore del disco). In questa soluzione le collisioni ci interessano relativamente, perché in un settore non ci deve stare solo un dato ma più dati; quando il settore diventa si utilizza una lista concatenata di bucket, cioè di settori.
##### B+tree
Dato un albero binario posso pensare di inserire in un [settore del disco](disco%20di%20memoria.md) l'insieme di nodi che ottengo dividendo l'albero in orizzontale.
In questo modo è come se raggruppassi più elementi (i valori contenuti nel settore) in un nodo dell'albero (il settore) che ha più puntatori (ad altri settori). Quello che ottengo in questo modo è un albero bilanciato con una profondità molto bassa; per arrivare alle foglie devo fare pochi accessi al disco.
# File system distribuiti
Per gestire grandi quantità di dati in passato si costruiva un singolo calcolatore che aveva molta memoria e prestazioni elevate; questo però non era scalabile rispetto alla fault tollerance, cioè quando la macchina si rompeva andava ricomprata tutta. Adesso si usano molti calcolatori (server) uguali ed economici in modo da scalare in prestazioni e memoria: aggiungere risorse ha un costo lineare.
L'obiettivo è gestire grandi dati, cioè dati che non stanno in ram, ma probabilmente neanche in un singolo disco. I dati sono suddivisi in chunk; ogni chuck è memorizzato nel disco di un server. Per problemi di fault tollerance i chunk devono essere duplicati, cioè non possono stare in un solo disco; questi dischi con gli stessi dati non si troveranno sullo stesso rack, ma saranno su macchine distribuite (dette nodi), che quindi si trovano in posizioni diverse.
##### Map reduce
A questo punto nasce il problema di dove eseguire le operazioni: se i dati sono suddivisi in più dischi, non ha senso portarli vicino a un unico calcolatore, ma si faranno operazioni sul server che li contiene e poi si useranno questi risultati intermedi per arrivare alla soluzione. La strategia che porta a questo risultato è detta [Map/Reduce](Map%20reduce.md).