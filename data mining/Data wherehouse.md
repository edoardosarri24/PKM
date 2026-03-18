Quando qualcuno lavora con un DBMS si suppone che conosca SQL e che conosca come è strutturato il DataBase (DB). Solitamente le informazioni all'interno del database sono utili ai manager, che però non sanno come usare un DBMS.
# Definizione
I data werehouse voglio aggregare e sintetizzare i dati presenti nei DB per aiutare chi prende le decisioni a trovare pattern nei dati senza conoscere gli aspetti tecnici.
Un werehouse (WH) è composto da:
- Da un insieme di dati che:
	- Hanno un target e uno scopo preciso. Nel senso che non sono presi tutti i dati ma solo quelli che mi interessano.
	- Provengono da più sorgenti.
	- Ai quali si sono aggiunte informazioni (e.g. aggregazione).
	- Sono persistenti.
	- Sono usati da chi prende decisioni.
- Da un insieme di tool che:
	- Raccolgono dati
	- Fanno cleansing
		Nel WH si prendono dati da sorgenti (database) diverse che li memorizzano in modo diverso. Se vogliamo mettere le cose tutte insieme si deve trovare un formato comune. In pratica si modificano gli attributi in modo che siano consisterti all'interno del WH; nei database originali restano poi come sono.
	- Permettono di fare query, reporting e analisi.
	- Permettono di fare [data mining](data%20mining.md) e [data visualization](data%20visualization.md).
# Architettura
Ci sono due architetture di base.
##### Werehouse architecture
Nell'[architettura classica](werehouse%20architecture.png) abbiamo più sorgenti che integriamo in un unico grande database, su cui poi i client potranno fare analisi tramite query.
I meta dati specificano qual'è il formato del WH e che tipi di dati sono contenuti.
Le sorgenti possono essere molto diverse da loro: possono esser database relazionali e non relazionali, oppure possono essere informazioni prese da altre fonti come internet o un social. Per questo motivo nel momento dell'integrazione è importate fare il cleansing.
I vantaggi che si ottengono sono:
- Efficienza
	Le sorgenti non devono gestire il carico di query complesse fatte durante momenti di grande attività. Ovvero se un negozio sta facendo molti scontrini, si bloccherebbe a causa di query complesse fatte dai client.
- Privacy
	I proprietari delle sorgenti non vedono che query vengono fatte.
- Avaibility
	Può essere usato quando le sorgenti non sono online.
- Si possono aggiungere informazioni rispetto alle sorgenti iniziali.
##### Query-driven approach
Nell'[architettura basata su query](werehouse%20query-driven%20architecture.png) i dati rimangono nelle sorgenti. Vista la loro eterogeneità e la volontà di specificare le query nello stesso modo per ogni sorgente, sono fondamentali i wrapper, che trasfomano una query in un formato standard nel formato corretto per la sorgente in questione.
I vantaggi che si ottengono sono:
- Non necessita di spazio per memorizzare i dati delle sorgenti. Soprattutto non si memorizzano tutti i dati, ma si accede solo a quelli necessari.
- I dati su cui fare query sono più recenti.
# Sistemi
Ci sono due tipi di sistemi diversi, che hanno obiettivi diversi.
##### OLTP
È un sistema basato su transazioni del DBMS. Se guardiamo l'architettura è la parte inferiore, quella delle sorgenti. In un sistema di questo tipo:
- Ci sono prevalentemente inserimenti di record e quindi piccole transizioni.
- Si gestiscono dati grezzi.
- È fondamentale recuperare i dati nel caso di malfunzionamento.
- Gli utenti sono gli impiegati. Ad esempio i cassieri che passano i prodotti sul nastro e vengono levati dal magazzino.
##### OLAP
È un sistema basato su un processo di analisi fatto sul WH. Se guardiamo l'architettura è la parte superiore, quella dei client che fanno query dal WH. In un sistema di questo tipo:
- Ci sono prevalentemente letture.
- Si gestiscono dati composti da dati grezzi aggregati.
- Nel caso di malfunzionamento si possono ripristinare i dati dalle sorgenti.
- Gli utenti sono i manager.
# Modelli
Un modello in un WH è il modo in cui si organizzano i dati per farci poi ricerche in modo efficiente. Siamo quindi all'interno del sistema OLAP, dove appunti si fanno più letture che scritture.
Ci sono due elementi di base che ci serve capire:
- Fact table
	Sono le tabelle del WH che contengono i fatti (e.g. una vendita); sono tabelle molto grandi.
	Ogni riga è quindi un elemento. Le colonne invece rappresentano gli attributi, cioè le informazioni relative ai fatti (e.g. quantità, prezzo, id dello store e id del prodotto.): possono essere sia valori numerici che id usati come chiave esterne ad altre tabelle.
- Dimension table
	Le entry di queste tabelle specificano le carattestiche degli attributi delle fact table. Ad esempio per una città viene specificato il cap, la regione, la nazione e così via.
##### Normalizzazione
Normalmente nei DB relazionali le tabelle vengono normalizzate. Questo permette di evitare problemi nel momento di possibili aggiornamenti: non si vuole la situazione in cui se si deve aggiornare una riga allora se ne deve aggiornare anche altre.
Questo non avviene nei WH per vari motivi: i dati non si trovano effettivamente nel WH e si possono sempre recuperare; ci sono più letture che scritture (non si hanno i problemi di aggiornamento); si favorisce la rapidità rispetto alla duplicazione.
##### Modello a stella
È un [modello a stella](star%20schema.png) dove dalla fact table parte una dimension table per ogni attributo e poi fine.
Lo schema a stella può essere anche visualizzato come un cubo in più dimensioni, detto data cube.
Il valore del fatto in una fact table può essere visto come un punto di uno spazio dove le dimensioni sono date dagli attributi che lo definsicono.
Nella prima slide dell'[esempio](multidimensional%20view.pdf) vediamo due dimensioni, che sono definite dai due attributi prodld e soredld; nella seconda slide invece le tre dimensioni sono date da prodld, soredld e date. In questo modo un valore di amt è un punto nello spazio.
Sul data cube possono essere fatte varie operazioni:
- Slice/Dice
	Specificare una/due o più dimensione e ottenere un sottocubo.
- Rollup/Drill-Down
	Nel primo si raggruppano più attributi della tabella e si uniscono i valori dei fatti; il secondo è l'operazioni inversa. Praticamente è uno zoom in/out.
##### Modello a fiocco
È un [modello a fiocco](snowflake%20schema.png) dove da ogni dimension table possono partire altre dimension table.