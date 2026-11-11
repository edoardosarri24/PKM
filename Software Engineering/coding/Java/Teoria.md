## GENERALE
### Ereditarietà
- Quando si esegue l’override di un metodo, il tipo di ritorno può essere un sottotipo di quello dichiarato nella superclasse. Si dice che i tipi di ritorno sono covarianti.
- Sui metodi d’istanza Java esegue la ricerca dinamica del metodo: parte a cercare nella classe effettiva dell’oggetto su cui è chiamato il metodo e risale le sue superclassi finché non trova l’implementazione più specializzata.
- Se una classe estende una classe e implementa un’interfaccia in caso di ambiguità di nomi la superclasse ha la precedenza.
## STATIC
### Variabili
- Quando si indica una variabile di una classe come $static$ si intende che di questa ne esiste solo una per classe e che sarà condivisa dalle varie istanze.
- Se una variabile statica ha bisogno di più istruzioni per essere inizializzata lo si può fare con i blocchi di inizializzazione statici (Gli stessi che esistono per le variabili d’istanza), che saranno eseguiti quando la classe viene caricata inizialmente. Si usa $static \{ statement; \}$.
### Metodi
I metodi statici sono metodi che non operano sulle istanze della classe, cioè non operano sugli oggetti. Per questo motivo da un metodo statico non si può accedere a variabili d’istanza.
Chiamare un metodo statico su un oggetto è legale, ma è una caduta di stile.
## CLASSI NIDIFICATE
Si usano per limitare la visibilità di una classe o per non fare confusione in un pacchetto con nomi troppo generici.
Per riferirsi alla $ClasseInterna$ nidificata in una $ClasseEsterna$ si usa la sintassi $ClasseEsterna.ClasseInterna$.
### Classi nidificate statiche
Una classe nidificata statica può essere sia $public$ che $private$. Nel primo caso non c’è nessuna differenza nel nidificare una classe o inserirla all’interno di un pacchetto.
Si usano quando un oggetto della classe nidificata statica non ha bisogno di essere a conoscenza di quale istanza della classe esterna lo ha creato.
Un’istanza della classe nidificata statica non può accedere a nessun membro d'istanza della classe esterna.
### Classi interne
Non usando _static_ si permette alla classe interna di sapere a quale istanza della classeEsterna appartiene. Quello che succede è che ogni istanza della classe interna ha una specie di variabile nascosta che fa riferimento all’istanza della classe esterna che la contiene: il riferimento a questa istanza è raggiungibile con $ClasseEsterna.this$.
Un’istanza della classe interna può chiamare un metodo d'istanza della classe esterna.
Si può chiamare il costruttore della classe interna da qualsiasi istanza della classe esterna con la sintassi $istanzaClasseEstrerna.new\ ClasseInterna$; in questo modo $ClasseInterna$ "punterà" a $ClasseEsterna$.
## JAVADOC
Il javadoc uno strumento contenuto nel [JDK](Java%20Platform.md) che genera documentazione HTML. Prima la documentazione era in un file a parte e dopo poco tempo ovviamente diventava obsoleta: in questo modo sorgente e documentazione si trovano nello stesso file ed è molto più comodo aggiornare uno in relazione all’altro.
- I commenti si inseriscono iniziando con $/*$ e terminando con $*/$.
- La prima riga dovrebbe essere una breve descrizione.
- All’intento dei commenti si usano dei tag preceduti da $@$.
- Per evidenziare codice (Nomi di membri all’intento del codice) si usa il modificatore HTML $<code>$ all’inizio e $</code>$ alla fine.
- Il commento $@deprecated$ indica che il membro è deprecato.
- Il commento $@see \ riferimento$ è un link ad un altro pezzo di codice.
- Autore e versione si indicano rispettivamente con i tag $@autor$ e $@version$.
- Un parametro si descrive con $@param \ descrizione \ variabile$.
- Il valore di ritorno, se non è $void$, si descrive con $@return descrizione$.
- Un’eccezione si descrive con $@throws \ descrizione \ classe$.
## INTERFACCE
### Costanti
Le variabili in un’interfaccia non possono essere d’istanza perché questa non specifica lo stato di un oggetto, ma specifica solo un comportamento.
Se dichiariamo una variabile in un’interfaccia queste sono automaticamente $public static final$.
### Metodi statici
Sono consentiti e servono per aggiungere funzionalità aggiuntive ad un’interfaccia.
### Lambda
Una lambda è in pratica una struttura dati formata dal codice, dai suoi parametri (Quelli prima della $->$) e dalle variabili libere (Quelle che non sono parametri e variabili locali). Questo permette ad una lambda di poter essere eseguita in differita, cioè anche dopo che il metodo che l’ha chiamata è ritornato: quello che succede è che la lambda si salva nella sua struttura le variabili libere (Spesso si dice che una lambda cattura i valori e non le variabili), che altrimenti non potrebbero essere accedute. Questo è anche il motivo per cui le variabili all’interno di una lambda devono essere $final$ o *effectively final*.
##### Funzioni
Java è un linguaggio OO, dove ogni cosa è un oggetto. Le funzioni sono istanze di oggetti che implementano un’interfaccia: le lambda, che sono un blocco di codice (Che si può assegnare, passare o restituire), servono per cerare facilmente queste “istanze funzioni”. Un metodo può restituire una lambda (Cioè una funzione): in questo caso è una funzione di ordine superiore.
##### Riferimenti a metodi
Se esiste già un metodo che fa quello che vogliamo dalla nostra lambda possiamo passarlo al posto di essa. Ci sono tre modi: $Classe::metodoIstanza$, $Classe::metodoStatico$, $oggetto::metodoIstanza$. Ci possiamo riferire ad un costruttore con $Classe::new$.
### Classi locali e anonime
Se si vuole passare, ritornare o assegnare una classe che implementa un’interfaccia funzionale allora il modo migliore è usare una lambda. Se i metodi da implementare però sono più di uno (Non si parla più di interfaccia funzionale) allora non si può usare una lambda.
##### Locale
È una classe, il cui nome viene specificato, definita all’intento di un metodo. Il suo uso è confinato alle situazioni dove il chiamante del metodo è interessato all’interfaccia dichiarata in tale metodo, ma non alla classe che la implementa.
Non serve usare modificatori di visibilità perché la sua visibilità è soltanto all’interno del metodo stesso.
Cosi facendo si ha il vantaggio che i metodi della classe locale possono accedere alle variabili circostanti: i modi in cui possono farlo (Come il concetto di _effectively final_) sono esattamente come quelli delle lambda.
##### Anonime
Si può usare una classe locale senza definirne il nome attraverso la sintassi $new \ Interface()\{ methods\}$. Tale istruzione $new$ significa costruisci una classe che implementa l’interfaccia e restituisci una sua istanza.
## REFLECTION
### Class
In fase di compilazione possiamo assegnare oggetti a variabili di un supertipo (Anche astratti); a run-time però qualunque oggetto avrà un tipo concreto preciso: questa informazione (Solo a run-time) si modella con la classe $Class<?>$.
Per ottenere un oggetto di classe $Class<?>$ possiamo usare: il metodo d’istanza $istanza.getClass;$ il metodo statico $Class.forName(String name)$. Da l’oggetto che ci venere restituito possiamo ottenere varie informazioni sulla classe concreta.
La JVM gestisce un oggetto univoco di tipo $Class$ per ogni classe concreta esistente. Questo permette per esempio di usare l’operatore $==$ per controllare l’uguaglianza.
## ECCEZIONI
Ogni eccezione è un’istanza di una sottoclasse di $Throwable$. Da $Throwable$ partono due rami:
- Error
	Sono situazioni dove il programma non riesce ad andare avanti in nessun modo e si può solo farlo presente all’utente.
- Exception
	Sono tutte le eccezioni pensate per essere gentile dal programmatore. Si dividono in:
	- Exception
		Sono le eccezioni controllate in fase di compilazione: devono essere dichiarate o gestite. Si deve prevedere che qualcosa potrebbe non andare come previsto per un motivo esterno al nostro codice. Ad esempio le $IOException$.
	- RuntimeException
		Sono le eccezioni non controllate. Sono quelle che non dovrebbero accadere e sono provocate di solito da una cattiva programmazione. Un esempio è $NullPointerException$.
### Risorse
Se lavoriamo con delle risorse (Che in quanto tale implementerà l’interfaccia $AutoCloseable$) chiamando dei metodi che sollevano eccezioni e vogliamo essere sicuri che alla fine del blocco $try$ queste risorse siano chiuse, possiamo inserirle tra parentesi tonde dopo la keyword $try$.
In questo modo quando si esce dal blocco $try$, perché terminato o perché è stata sollevata un’eccezione, si chiamerà il metodo $close$ dell’interfaccia $AutoCloseable$ sulle risorse.
### Concatenare eccezioni
Le eccezioni in Java seguono un meccanismo di concatenazione. Questo è particolarmente utile nei sistemi a layer: se un’astrazione di alto livello fallisce per un’eccezione sollevata in un livello più basso, non è buona norma che quest’ultima sia propagata direttamente nel livello superiore; si tende a concatenarla ad un eccezione di più alto livello. Questo si può fare con un costruttore a cui si passa un $Throwable$, oppure con il metodo distanza $initCause$.
Possiamo invece accedere alle cause concatenate con il metodo d’istanza $getSuppressed$, che sarà chiamato sull’eccezione principale.
## COLLEZIONI
Le classi del framework $Collection$ che implementano l’interfaccia $RandomAccess$ (Interfaccia di marcatura) garantiscono l’accesso diretto (Efficiente) ad una data posizione della struttura.
### Collection
- $List$ implementa collezioni sequenziali.
	- $ArrayList$
		Implementazione usando un array.
	- $LinkedList$
		Implementazione usando una lista.
- $Set$ implementa collezioni senza ordine. Ricerca efficiente.
	-  $HashSet$
		Implementazione usando una tabella hash. Molto efficiente. 
	- $TreeSet$
		Implementazione usando un albero binario. Permettono l’attraversamento: attraverso l’ordine di inserimento (Per default); attraverso l’ordine passato al costruttore; attraverso l’ordine della classe degli elementi se questa implementa $Comparable$.
	- $BitSet$
		Raggruppa i bit in un array di long. Ha metodi efficienti per leggere e manipolare i vari bit. Si usa spesso al posto di un array di boolean o per indicare quali dei primi n interi sono presenti in un insieme.
	- $SortedSet$
		Consente l’iterazione secondo l’ordinamento. 
	- $NavigableSet$
		Permetter di accedere ai vicini di un elemento.
- $Queque$ implementa collezioni con politica FIFO.
	- $Deque$
		Collezione con politica FIFO con due capi di inserimento ed estrazione. Si usa per implementare anche le pile (LIFO).
### Iteratori
- $Iterable$
	Consente di avere un iteratole con cui iterare una collezione.
- $Iterator$
	Consente di rimuovere gli elementi.
- $ListIterator$
	Consente di aggiungere andare avanti e indietro. In questo caso se ci sono più iteratori su una collezione e questa viene modificata può essere sollevata una $ConcurrentModificationException$.
### Map
Coppie chiave-valore. Si possono avere delle viste che però non sono delle copie, ma riferisco alla mappa vera e propria. Modifiche in esse si rispecchiano sulla mappa stessa.
- $HashMap$
	Molto efficienti.
- $TreeMap$
	Permettono l’attraversamento: attraverso l’ordine passato al costruttore; attraverso l’ordine definito dalla classe degli elementi, che deve implementare l’interfaccia $Comparable$.
- $LinkedHashMap$
	Ricordano l’ordine di inserimento ed è molto efficiente.
- Properties
	È una classe che implementa una mappa di tipo $<Object, Object>$ (Meglio non usare il metodo $get$ che restituire un $Object$). La caratteristica è che sono facilmente salvabili e caricabili come semplice testo; sono spesso usate per salvare le configurazioni dei programmi.
- $WeakHashMap$
	Coopera con il garbage collection, che segue gli oggetti attivi e libera la memoria da quelli non attivi. Queste mappe fanno si che una coppia chiave-valore venga eliminata quando la sua chiave non è più accessibile.
### Viste
Una vista (Implementa $Collection$) di una collezione è un oggetto leggero perché non fa una copia dei dati della collezione che vuole rappresentare. Le viste sono un meccanismo che consente di rappresentare e lavorare su dei dati senza dover creare una nuova collezione per condividerli. Si ottengono spesso da metodi statici di $Collections$.
- Normalmente una modifica alla vista comporta una modifica anche alla collezione di partenza.
- Si possono restituire viste non modificabili: la chiamata di un metodo mutatore solleva un eccezione.
- Si possono restituire viste vuote o singleton.
## GENERICI
### Metodi generici
Il parametro di tipo è inserito tra modificatori e tipo di ritorno. Un tipo generico può fare da upperbound (Cioè $T extends Type$ è legale), ma non da lowerbound (Cioè $T super Type$ non è legale).
### Wildcard
I metodi generici sono utili ma non versatili (Per esempio i parametri di tipo non fungono da lowerbound). Se non vogliamo usare i tipi generici ma i tipi dei parametri e i tipi di ritorno variano con delle restrizioni possiamo usare le wildcard.
Il loro meccanismo è detto varianza in sede d’uso: si dichiara un tipo come coartante o controvariante quando lo si usa in qualche contesto. L’opposto è la varianza in sede di dichiarazione: quando si crea una classe si dichiara che essa è covariante o controvariante; in questo modo chi la usa non deve tutte le volte specificarlo usando le wildcard. Il meccanismo usato da Java è meno comodo ma più potente.
##### Consumatori
Siamo nella situazione in cui dobbiamo leggere degli oggetti da una classe generica (Come ad esempio una lista) passata come parametro; questi oggetti hanno però una limitazione superiore. Quello che facciamo è usare la wildcard $? extends Type$.
Perché non possiamo scrivere? A runtime la lista conterrà oggetti di un sottotipo di $Type$. Potrebbero voler aggiungere un oggetto che è sì un sottotipo di $Type$ (E questo sarebbe legittimo), ma che è un supertipo del tipo effettivo della lista (E questo non è consentito).
Si dice che i tipi dei parametri sono covarianti, cioè è possibile passare un sottotipo ad un metodo.
##### Produttori
Siamo nella situazione in cui dobbiamo consumare degli oggetti da una lista attraverso un’operazione, passata come parametro; si vuol poter passare anche una funzione che elabori oggetti meno specifici (Cioè che sono supertipo). Quello che facciamo è usare la wildcard $? super Type$.
Si dice che le funzioni sono controvarianti nei tipi (Se ci si aspetta una funzione che  elabora oggetti di un tipo se ne può passare una che elabora invece oggetti di un supertipo).
##### Tipi generici e wildcard
Siamo nella nella situazione dove il tipo dei parametri o il tipo di ritorno di un metodo non sono tipi concreti ma sono generici.
In generale il meccanismo non cambia: è legale fare $? extends T$ oppure $? super T$.
Molti si ricorda PECS: produttori extends, consumatori super.
##### Type erasure
I tipi generici vengono compilati come raw nel byte code della JVM, cioè vengono sostituiti con il tipo più specializzato che si possa dedurre: un tipo $T$ diventa $Object$; un tipo $T extends Comparable$ diventa $Comparable$.
Questo ci fa capire l’importanza dei type bound: se non mettessimo delle restrizione ai tipi generici e alle wildcard allora tutto il codice generico sarebbe compilato come $Object$.
Nonostante tutto durante l’esecuzione avviene la type erasure, cioè la cancellazione dei tipi generici. Senza questo meccanismo tutti i tipi generici sarebbero tipi diversi e ci sarebbe un grande aumento delle dimensioni del codice.
## STREAM
Quando si lavoro con gli stream il flusso di lavoro è: creare uno stream; fare delle operazioni intermedie; fare un’operazione terminale che restituisce un risultato.
- Uno stream non memorizza gli elementi.
- Non modifica la sorgente dei dati.
- Le operazioni sono lazy, cioè vengono fatte solo quando il loro risultato è necessario.
### Creazione
- Da una collezione con $stream$ o $parallelstream$.
- Da un array con $Arrays.stream$.
- Stram infiniti con $Stream.generate$.
- Stream di sequenze con $Stream.iterate$.
### Trasformazioni
- Filtrare con $filter$.
- Trasformare con $map$ o $flatmap$.
- Prendere o scartare con $limit$, $skip$, $takeWhile$, $dropWhile$.
- Ordinare con $sorted$.
- Invocare funzioni con $peek$.
- Non duplicati con $distinct$.
### Riduzioni (Operazioni terminali)
- $count$
- $max$
- $findFirst$
- $anyMatch$
### Raccolta di risultati
- Applicare funzioni con $forEach$, $forEachOrdered$.
- Collezione con $collect$, che usa $Collectors$. $Collectors$ può restituire collezioni e mappe. Per quest’ultime si può restituire mappe i cui valori sono collezioni.
## LOGGING
### Creazione
Il logger di default si ottiene conio metodo statico $Logging.getGlobal$ e ci si legga le informazioni con il metodo d’istanza $info$.
Se si vuole creare un logger con un nome (Per esempio quello della classe) si usa $Logger.getLogger(String nome)$ dove $nome$ è il nome del logger; se richiamiamo la stessa istruzione con lo stesso nome viene restituito lo stesso logger.
### Livelli
Ci sono 7 livelli, ma 4 sono quelli importanti: $SEVERE$ per gli errori, $WARNING$ per gli warnings, $INFO$ per le informazioni e infine $FINE$ per il debug. Di default sono attivati i primi 3 livelli; si può ampliare i livelli in cui un logger registra con il metodo d’istanza $setLevel$. Per ogni livello si può usare il metodo d’istanza con il nome corrispondente per logore (Per info il metodo $info$ e così via).
### Eccezioni
Si può lograre le eccezioni prima che vengano sollevate direttamente (Cioè con l’istruzione _throw_) con il metodo $log$ (Che prenderà dei parametri specifici per le eccezioni) oppure $throwing$.
### Handler
DI default il logger scrivere nello stream $System.err$ (E quindi sulla console), cioè il suo gestore è un $ConsoleHandler$.
Si può impostare un altro gestore con il metodo d’istanza $addHandler$.