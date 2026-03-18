Il backend è dove viene eseguita la logica del nostro sistema. L'ide aè che il backend gestisca la logica, esponga dei [servizi](Service%20Oriented%20Architecture%20(SOA).md) che il [frontend](frontend.md) mostra all'utente.
Il più enterprise oriented è [Jakarta](Jakarta.md), ma ci sono moltre [altre soluzioni](linguaggi.md).
È formato da più layer.
# Presentation layer
È la user interface, cioè la parte di frontend. Presenta le informazioni all'utente e permette l'interazione con il sistema. Non ci piace lavorare cono il presentation layer.
##### DTO
Spesso il frontend non deve presentare tutte le informazioni che una classe del domain model contiene, ma solo una sua versione semplificata.
L'idea del DTO è quella di non filtrare i campi necessari nel frontend ma di scambiare solo le informazioni necessari. Inoltre in architetture [SOA](Service%20Oriented%20Architecture%20(SOA).md) i dati tra front e back end vengono inviati tramite operazioni [REST](REST.md) in formato json: scambiare meno dati è sempre un vantaggio.
Quello che facciamo è quindi scambiare oggetti leggeri rappresentati in formato json; tali oggetti sono istanze di classi semplificate rispetto a quelle del domain. Gli oggetti DTO sono restituiti dai metodi del domain model.
- Per passare da oggetti complesso a oggetto sempliicato e viceversa si usano i mapper, che svolge appunto il ruolo di convertitore bidirezionale. Oltre ad implemetarci i nostri mapper ci sono delle tecnologie che lo fanno per noi (es: Gson, maptruct): se il contesto è semplice e non abbiamo bisogno di personalizzazione allora possono andare bene, ma appena nasce il bisogno di personalizzare le cose queste soluzioni sono un disastro.
# Business logic/Service layer
È la logica applicativa, ciò che contraddistingue l'applicazione: due applicazioni possono basarsi usllo stesso database e domain model, ma avranno sicuramente due business logic diverse. A questo livello:
- Si implementano le regole di business, cioè i vincoli e la logica stabile da chi richiede l'applicazione. Si gestisce l'integrità dei dati assicurandosi che le operazioni rispettino i vicnoli stabiliti. Alcuni esempi sono: un utente può prenotare un servizio solo se è autenticato; il totale di un ordine viene calcolato in base a sconti attivi; un pagamento può essere effettuato solo se il saldo è sufficiente
- Si coordina il flusso dei dati dal presentation layer fino al database.
##### Business logic
Quando parliamo di business logic, intendiamo che vogliamo realizzare un'architettura di tipo [StateFul](Jakarta%20stateful%20architecture.png), cioè dove il backend ha la capacità di memorizzare informazioni che non fanno esplicitamente parte dell'applicazione. In questa soluzione frontend e backend sono accoppiati e devono essere sviluppati dalle stesse persone.
Sono le appliazioni monolitiche.
- User interface
	In questo tipo di architettura ha poche responsabilità: deve ricevere una richiesta dall'utente e inoltrarla al backend; si tratta di un'applicazione frontend semplice. Il browser riceve i dati in formato $http+css$ e li eroga all'utente.
	È strettamente accoppiata al controller della business logic (e quindi al backend): quando una Page riceve un comando lo passa al relativo Controller, che decide se accettarlo ed eventualmente esegue le operazione a esso legato.
- Business logic
	Il flusso è che prima si costruisce la Page dell'user interface, poi si definisce il relativo Controller (che quindi dipenderà dalla Page) e poi si appoggia il controllore su tutto il backend.
	Durante l'interazione tra frontend e backend, cioè durante una sessione, si accumulano informazioni (es: informazioni di login) che non fanno parte dell'applicazione (non finiranno nel database), ma comunque devono essere memorizzate da qualche parte. Queste informazioni relative allo stato transiente sono salvate nella business logic all'interno dei Session Beans.
##### Service layer
Quando parliamo di service layer, vogliamo realizzare un'architettura di tipo [RestFul](Jakarta%20restful%20architecture.png), caratterizzata dall'assoluta assenza di stato transiente all'interno del backend. In questa soluzione frontend e backend sono disaccoppiati.
Le operazioni che mettiamo a disposizione sono rese pubbliche tramite degli [endpoint](Endpoint%20Java.md) che espongono operazioni [REST](REST.md). Per costruire servizi REST ogni applicazione che vuole seguire quanto dettato da Jakarta deve sottostare alle specifiche definite da [JAX-RS](JAX-RS.md).
- User interface
	In questo tipo di architettura diventa una e vera applicazione complessa. L'interazione con il backend avviene tramite richieste http.
	Siccome il backend non salverà nessuno stato transiente, questo deve essere memorizzato nel frontend; per questo motivo lo spazio usato per la memorizzazione è a carico del frontend.
- Service layer
	Non deve memorizzare lo stato transiente o gestire sessioni (ogni richiesta è a se state). Agisce solamente come un endpoint: espone API [HTTP](HTTP.md), riceve richieste e le inoltra al resto del backend.
# Persistence layer
Il persistance layer si occupa di correlare gli oggetti Java con il mondo della persistenza, cioè con i database relazionali.
È composto da:
- [domain model](domain%20model.md)
- [ORM](ORM.md)
- Data layer
	Gestisce la memorizzazione persistente e il recupero dei dati tramite operazioni CRUD. Solitamente è il collo di bottiglia e porre l'attenzione sulle [Prestazioni](Prestazioni.md) è importante.
	- Le tecnologie usate sono principalmente MySQL.
# Processo
Vediamo il [processo](system%20design.md) che porta alla realizzazione di un applicazione sotfware basata su Jakarta.
##### Analisi
Si parte dalla struttura dell'architettura del sistema. Si definisce per prima l'interfaccia, per poi definire la business logic e poi implementare i vari [DAO](Jakarta.md#ORM) e l'[entity manager](Jakarta.md#ORM) e infine si modella il [domain model](Jakarta.md#Domain%20model/Domain%20logic).
- Per definire i metodi da inserire nella business logic si utilizza lo [Use-case diagram](Use-case%20diagram.md). Questi sono infatti i metodi che chiama l'utente e quindi che nella fase di requisiti sono stati identificati con il cliente.
- Nell'implementazione del domain model si inizia con un primo [Class diagram](Class%20diagram.md) astratto (cioè non molto utile nella fase implementativa). Una volta stabilite le classi si definiscono i metodi da inserire nelle classi; questi sono solo quelli che modificano lo stato delle classi (e quindi delle tabelle del database) e si specializza il class diagram in modo da essere utile con l'implementazione. Ecco un [esempio](esempio-creazione%20del%20domain%20logic.pdf).
##### Development
- Durante lo sviluppo si dovremmo organizzare le nostre classi in pacchetti. L'idea è quella che all'interno dello stesso pacchetto ci stiano le classi che sono coese, cioè quelle che soddisfano il [SRP](single%20responsability%20principle.md): se c'è una modifica a un componente devo cambiare al più le classi presenti all'interno di un pacchetto. Questo permette di avere una manteinance molto più semplice.
- L'idea è quella di costruire prima un monolitico e poi passare in modo incrementale a un'applicazione a microservizzi.