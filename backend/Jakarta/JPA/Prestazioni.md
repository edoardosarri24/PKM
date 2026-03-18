Le prestazioni non influiscono sugli aspetti funzionali del sistema, ma sulla capacità di rispondere rapidamente alle richieste. Siccome i dati sono sempre di più si deve cercare di accederci e manipolarli in modo efficiente.
# N+1 Query problem
È il problema che si verifica quando una query primaria viene trasformata in $N+1$ query per prendere gli stessi dati; in pratica si farebbe una query per ogni elemento della tabella da cui si vuole caricare.
Nel seguito usiamo questo esempio:
![questo esempio](n+1%20query.png)
##### Join fetch
Supponiamo di voler prendere dal database tutti gli elementi di $ProductReview$. Se scriviamo la query JPQL mostatata nell'esempio sopra, allora saranno generate tante SELECT quante sono le istanze del database. La soluzione per evitare questo è usare $join fetch$ nel codice, in [questo modo](n+1%20query%20solution.png).
##### Lazy policy
Quando usiamo la join fetch stiamo sovrascrivendo la policy per il recupero dei dati con eager (di default EAGER per le annotazione con $x2Many$): non c'è modo di non prendere a cascata tutti gli oggetti collegati (nell'esempio prendiamo anche tutti i $Porduct$), anche se nell'annotazione usiamo $@ManyToOne(fetch=FetchType.LAZY)$.
Quello che solitamente si fa è usare l'annotazione lazy e avere due metodi: quello senza join fetch, che recuperarà i dati in modo lazy ma non evita il problema delle $n+1$ query, sarà usato in qualche caso d'suo.
# Pagination
Vogliamo risolvere il problema per cui le istanza di una tabella possono crescere molto nel tempo e presentare tutti i risultati di una query in un'unica pagina può non essere efficiente.
Nel mondo relazione abbiamo LIMIT e OFFSET per mitigare questo problema, ma in questo caso il problema alla base è che i dati cambiano nel tempo e l'uso di questi valori statici porta a dover riscrivere il codice.
##### Soluzione
Su una query si chiama $setMaxResult()$ e $setFirstResult()$, come in [questo esempio](pagination%20solution.png).
La lunghezza della pagina deve essere un tradeoff: troppo lunga non si risolve il problema; troppo corta servono troppe query.
# Column redundancy
Quando un tabella ha molte colonne ci possono essere problema di prestazioni sugli updte o sulle select: durante gli update anche le colonne che non sono modificate saranno riscritte; durante una select invece saranno caricate tutte le colonne.
##### Soluzione
Se usiamo una versione nuova (dopo la 4.0) di hibernate allora questo è un non problema, perché hibernate aggiorna solo le colonne modificate; se si usa invece una versione vecchia (possibil perché ci sono molte architetture legacy) allora dobbiamo essere a conoscenza del problema.
# Data Tranfer Object (DTO)
Quando abbiamo tabelle con molti attributi ma vogliamo caricare solo alcune informazioni vorremmo farlo in modo efficiente. Questo viene fatto implementano una classe che non appartiene al [domain model](domain%20model.md) e che ha solo i campi che vogliamo caricare. Una classe di questo tipò essere implementata come classe vera e propria oppure tramite il tipo $Tuple$.
##### Soluzione
Usando una classe vera e propria la soluzione è [questa](DTO%20solution.png).
Se vogliamo informazioni da più tabelle diverse che però sono referenziate allora la soluzione è [questa](DTO%20con%20referenziamento.png).
# Batching
Quando vogliamo fare la stessa operazione su una sequenza di oggetti, allora farle in modo sequenziale un oggetto alla volta può non essere una buona soluzione, visto che questo vorrebbe dire fare una query per ogni oggetto.
##### Soluzione
Per migliorare questo approccio possiamo raccogliere tutte le query di un batch ed eseguirle tutte in un'unica query. È importate che tutte le query di un batch si riferiscano alla stessa tabella.
Hibernate di deafult non supporta questo meccanismo, ma dobbiamo impostarlo nel file $persistance.xml$. Viene raccomandato un valore che varia tra 10 e 50 a secondo degli use case.
# Caching
Esiste.