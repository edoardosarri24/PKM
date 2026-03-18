È un [processo](system%20design.md) per lo sviluppo di sw.
Il fine è voler dare al cliente una versione iniziale grezza del prodotto e successivamente fornire delle versioni intermedie aggiungendo via via le funzionalità con priorità maggiore.
- Le [[system design#ACTIVITY|attività]] sono molto correlate e svolte in parallelo.
- Il team di sviluppo non deve essere numeroso e gli sviluppatori devono essere tutto capaci e dello stesso livello.
- Lo sviluppo si basa sul TTD (Test driven development); è necessario quindi un framework per i test unitari.
  Un problema è determinare quanto debba essere testato il SW: a livello di istruzione, a livello di ramo (Ogni if o else alla volta) o a livello di cammino (Ogni combinazione di if/else).
- La documentazione non è così fondamentale come nel caso del [[Plan-Driven process]], ma è ridotta al minimo. Infatti i requisiti diventano dei racconti (User stories) e la loro implementazione dipende dalla loro priorità e dal tempo a disposizione.
##### Usi
- Il cliente non ha un'idea concreta di cosa vuole e necessita di dare feedback continui.
- Il progetto non deve essere troppo grande.
- È una metodologia usata per la fase di sviluppo e non per quella di mantenimento: non si può mantenere il team unito per molto tempo.
##### Conseguenze
- I costi per assecondare nuove richieste del cliente sono molto inferiori rispetto al [[Plan-Driven process]]: non si deve tornare indietro all'interno delle azioni del processo ma si implementano a partire da quello che già abbiamo.
- Il refactoring è una parte fondamentale. La struttura del sistema tende a degradare a ogni nuovo rilascio e questo implica la necessità costante di rivedere il codice.
- A causa della non rigidità si possono introdurre errori e questo porta a reiterare su ciò che è già stato svolto.
# Agile
- Il cliente deve essere vicino la team di sviluppo: deve dare feedback e modificare le priorità di ciò che deve essere implementato (User stories).
- Le attività superflue, come la generazione della documentazione, e le regole troppo stringenti devono essere eliminate e limitate nel processo. Dove queste sono obbligatorie (Vedi gli standard) devono essere integrate nel metodo cercando di non cambiarne l'idea.
### Scrum
Metodologia Agile più diffusa che si basa su tre fasi:
- Si stabiliscono gli obiettivi del progetto (User stories) e si fa il design dell'architettura.
- Si seguono una serie di sprint, in ognuno dei quali si sviluppano una serie di funzionalità.
- Si chiude il progetto, si genera la documentazione e si istruisce il cliente all'uso.
##### Prodotto
È ciò che si sviluppa.
##### Proprietario
Chi richiede il prodotto: può essere il cliente ma anche il manager in una grande software house.
##### Product backlog
Insieme di PBI (Product backlog item), task di cui il team si deve occupare.
- Ogni item è classificato come: funzionalità (Implementare un'operazione), richieste del cliente, attività di sviluppo (Creare qualcosa) e miglioramento.
- Ogni item è classificato come: pronto per la considerazione (Si deve decidere se includerlo del progetto), pronto per il raffinamento (È chiaro che sarà sviluppato, ma non si è ancora parlato di come farlo), pronto per l'implementazione.
- Ad ogni item è assegnata una priorità e il tempo necessario per completarlo.
##### Team
- Gruppo composto da non più di 7 persone. Sono tutti sullo stesso piano e tutti sanno la situazione degli altri e la situazione del progetto.
- Viene nominato uno "scrum master" (Solitamente quello con più esperienza) responsabile di parlare col proprietario e di organizzare i daily scrum.
##### Daily scrum
- Rapida riunione giornaliera per parlare dei problemi del giorno precedente e di come approcciare il lavoro odierno.
- Si rivede lo sprint backlog, rimuovendo task completati e aggiungendone di nuovi nel caso un componente dei team sia senza lavoro.
##### Sprint
Iterazione di sviluppo. Dura 2-4 settimane (Costante e non può essere allungato in nessuna caso) e il cliente non è coinvolto (Può parlare solo con lo scrum master).
- All'inizio di ogni sprint si deve: rivedere e perfezionare il product backlog, modificando eventualmente le sue etichette; si decidono gli sprint backlog (Sottoinsieme dei PBI) che si vogliono implementare durante tale iterazione in base alla loro priorità e alle richieste del cliente.
- Può succedere che alla fine dello sprint non si sia implementato tutto; in questo caso gli sprint backlog non completati tornano del PBI e saranno aggiornati.
- Velocità
	Quantità di product backlog item svolti in uno sprint.
### Extreme programming
È un'estremizzazione dell'agile programming:
- Ci sono nuove funzionalità ogni giorno.
- Spesso c'è una riduzione della qualità del codice a vantaggio della velocità.
- Si usava (Ora non più) il pair programming; si aumenta l'efficienza, si diminuire l'introduzione di errori e si hanno le posizioni ridondate.
- Il cliente deve stare sul posto dove si sviluppa il prodotto in modo da dare feedback continui.
- Non si integra con la presenza di un manager: non è prevista documentazione e non c'è una piramide di responsabilità.