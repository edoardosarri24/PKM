Il C è il linguaggio più usato all'interno dei sistemi embedded e per sistemi safety-critical. I motivi sono il massimo controllo sull'HW, la gestione della memoria e la sua velocità.
La filosofia del C è che i programmatori siano molto bravi e sappiano cosa stanno facendo.
## Idea
L'idea del MISRA C si basa su tre aspetti: i programmatori sbagliano e di solito lo fanno sempre sulle stesse cose; i compilatori C non eseguono staticamente ogni controllo possibile (Type checking); ci sono delle situazioni in cui il comportamento del compilatore non è ben definito (Undefined behavior) e può variare da uno all'altro.
Il Coding Standard può essere composto di:
- regole di 'buonsenso' (come usare solo valori signed o unsigned);
- ridurre il sottoinsieme linguistico (non usare goto e malloc);
- linee guida di stile (non usare il tab);
- convenzioni nei nomi (assicurarsi che tutte le funzioni pubbliche inizino con < filename>);
- metriche di qualità e complessità (basso numero ciclomatico delle funzioni).
Con queste assunzioni si vogliono definire delle regole per limitare l'utilizzo del linguaggio, in modo da evitare alcuni problemi e scrivere codice più pulito.
#### MISRA (Motor Industry Software Reliability Association)
Obiettivo di promuovere le $best$ $practice$ nello sviluppo di sistemi elettronici $safety-related$ nei veicoli stradali e nei sistemi embedded.
- MISRA-C:1998 -> linee guida per l'uso del C nei software basati sui veicoli.
- MISRA-C:2004 -> linee guida per l'uso del C nei critical system.
- MISRA-C:2012 -> linee guida per l'uso del C nei critical system (compatibilità estesa).
## Direttive
Sono delle guide linea, difficili da formalizzare e da identificare attraverso un analizzatore statico. Sono regole soggette a maggiore interpretazione e si rivolgono infatti più a processi.
Esempi sono: minimizzazione dei fallimenti a run-time; i valori passati a una funzione devono essere controllati; non si deve usare l'allocazione dinamica della memoria (Con la $malloc$ si potrebbe allocare più memoria di quella a disposizione); linguaggio assembly deve essere incapsulato (in C si può aggiungere un pezzo di assembly per questioni di efficienza); la dimensione dei tipi deve essere specificata nel nome del tipo tramite una $typedef$ [esempio](typedefExample.png); tutti i codici sorgenti devono compilare senza errori.
## Regole
Sono delle regole precise e chiare, identificabili da un analizzatore statico. Producono codice più pulito e con bug meno nascosti. 
- Non ci devono essere undefined behavior. In questo modo non ci si lega al compilatore e non si deve conoscere ogni suo comportamento specifico. Non devono verificarsi casi indefiniti o comportamenti critici non specificati ([esempio](undenided%20vs%20enspicified.png)).
- Non ci deve essere codice irraggiungibile. Un'eccezione è il $default$ in uno $switch-case$.
- Non ci devono essere commenti innestati (commenti in commenti, [esempio](commenti%20innestati.png)). 
- Non ci devono essere dichiarazioni uguali in blocco innestato e in quello che lo contiene
  ([esempio](variabili%20innestate.png)).
- Gli array vanno dichiarati insieme alla loro lunghezza oppure vanno inizializzati durante la dichiarazione.
- Le operazioni di shift per accedere ai singoli bit non devono permette di fare più giri della stessa struttura.
- Non si deve fare assunzioni sull'ordine dell'esecuzione delle operazioni in un'espressione.
- In una funzione ci deve essere un solo punto di uscita alla fine (non più di un return).
- Non si usa la ricorsione. Questo nei sistemi embedded è un problema perché si potrebbe eccedere la disponibilità di memoria.
- Le funzioni devono essere dichiarate nel file $.h$. Se una funzione non è dichiarata il compilatore assume che il tipo di ritorno sia un $int$.
- Non si dovrebbe copiare l'indirizzo di memoria di una variabile con $automatic$ $storage$ in un altra variabile che persiste dopo che la precedente cessa di esistere.
- Non si devono effettuare conversioni tra puntatori a funzioni e altri tipi. Si possono convertire puntatori a funzioni solo in puntatori ad altre funzioni compatibili con essi.
- Le funzioni di allocazione e deallocazione della $stdlib.h$ non dovrebbero essere usate. Si rischia un troppo utilizzo elevato della memoria e di avere puntatori non deallocati.
- Uno stesso file non deve essere aperto sia in lettura che in scrittura. Il puntatore si potrebbe muovere non uniformamene per entrambi gli stream.

## Conformità MISRA-C (compliance)
Obiettivo -> non violare regole.
- usare uno o più tool di controllo statico del codice;
- se una regola non può essere controllata da tool -> manual review;
- la matrice di conformità MISRA-C è una lista di ogni regola e di ogni violazione di tale regola individuata dai tool e dei compilatori ([esempio](compliance%20matrix.png)).
## Deviazione
A volte è necessario violare una regola. Questa può essere a livello di progetto, e quindi essere violata in un file o in tutto il progetto (accettato a inizio progetto), o a livello di specifica, cioè in una piccola parte circoscritta.
In questi casi si deve: dire quale regola è stata violata; motivare la scelta e quindi spiegare i motivi; elencare le potenziali conseguenze a cui questa scelta porta; dimostrare che la safety è comunque garantita.