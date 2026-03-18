Le estensioni in [quarkus](quarkus.md) rappresentano il meccanismo per aggiungere le funzionalità che sono necessarie all'applicazione. L'idea è di aggiungere solo ciò che è strettamente necessario, in modo da avere un'applicazione leggera con tempi di [building](building.md) e che occupa poca memoria.
# Architettura
Da un punto di vista del programmatore, un'estensione è una libreria che in modo modulare aggiunge funzionalità rispetto al core di Quarkus.
Da un punto di vista dall'[architettura Quarkus](microservizi/quarkus/file/architettura.png), queste si trovano al livello più alto e sono ciò con cui il programmatore interagisce maggiormente.
##### ArC
L'unica estensione che sta nel core di Quarkus è $ArC$. Questa è un'implementazione della specifica del [context and Depencency Injection (CDI)](Context%20and%20Depencency%20Injection%20(CDI).md).
Se non ne abbiamo bisogno possiamo anche rimuoverlo. qa
# CLI
Per lavorare le estensioni la cosa migliore è usare il CLI tool.
Ciò che facciamo con le estensioni da termianle ha un impatto solamente con il file $pom.xml$, il file di configurazione di Maven e quindi potremmo modificare direttamente anche solo quello.
##### Comandi
Il comando principale è $\texttt{quarkus extension}$. A questo si può concatenare altri parametri:
- $\texttt{list}$
	Serve per vedere tutte le estensioni installate.
- $\texttt{--installable}$
	Serve per vedere tutte le estensioni installabili.
- $\texttt{categories}$
	Serve per suddividere tutte le estensioni installabili in categorie.
- $\texttt{--installable --categories "categogy"}$
	Serve per filtrare tutte le estensioni installabili per categoria.
- $\texttt{add "name-extension"}$
	Serve per installare l'estensione $name-extension$.
- $\texttt{remove "name-extension"}$
	Serve per rimuovere l'estensione $name-extension$ già installata.
# Guide
Sul [sito di Quarkus](https://quarkus.io/guides/) ci sono le extensioni che possiamo installare.
Per osservare esempi di codice, o come usarle o per la documentazione approfondita è possibile osservare la guida.
# Estensioni
##### Rest
L'estensione REST che solitamente si utilizza è $quarkus-rest$ lato server e $quarkus-rest-client$ lato client.
##### Jackson
Per gestire la serializzazione dei JSON delle richieste delle risposte REST la libreria standard (la più veloce) in Java è Jackson. L'estensione che si usa e che include Jackson è $quarkus-rest-jackson$ lato server e $quarkus-rest-client-jackson$ lato client.
La libreria Jackson richede che la classe da serializzare abbia un costruttore vuoto. Funziona grazie a oggetti di tipo $ObjectMapper$ tramite il metodo d'istanza $\texttt{mapper.writeValueAsString}$ e $\texttt{mapper.readValue}$-
La serializzazione è il processo con cui si trasforma un oggetto in una sequenza di byte, cioè in testo; la deserializzazione è il processo inverso. Tramite questo meccanismo il server manda tutte le informazioni che ha, mentre il client le filtra. Questo vuol dire che se il modello lato client è più snello, cioè ha meno campi, di quello lato server, solamente le informazioni che sono presenti lato client veranno memorizzate.
##### OpenAPI
Per generare documentazione si può seguire lo standard [OpenAPI](OpenAPI.md) di [MicroProfile](MicroProfile.md) tramite l'estensione $quarkus-smallrye-openapi$.
Questa estensione permette, solo in [dev mode](development%20mode.md) (di default, ma si può cambiare nel file $application.properties$), di visualizzare gli [endpoint](Endpoint%20Java.md) e i servizi offerti tramite una UI all'indirizzo $localhost:8080/q/swgger-ui$.
##### GraphQL
È un framework usato per la comunizione tra servizio che da potere al clinet invece che al server. In questo modo [GraphQL](GraphQL.md) è ottimo per quelle situazioni vincolate, dove il numero di accessi alla rete è un problema.
L'estensione da usare è $quarkus-smallrye-graphql$ lato server e $quarkus-smallrye-graphql- client$ lato client. Questa mette a disposizione anche un'interfaccia UI con cui poter testare il servizio; è raggiungibile tramite l'indirizzo $localhost:8080/q/graphql-ui$.
##### gRPC
Il framework [gRPC](gRPC.md) è rilasciato da Google che permette una minor latenza nella comunicazione rispetto allo standard [REST](REST.md).
L'estensione è chiamata $quarkus-grpc$.
##### Health
Sono le metriche binarie definite dalla specifica di [MicroProfile](MicroProfile.md). L'implementazione è offerta da [SmallRye](health.md#Check) ed è disponibile in quarkus con l'estensione $smallrye-health$.