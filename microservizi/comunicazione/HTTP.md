È un'implementazione del livello [applicazione](TCP%20IP.md#Application) della pila protocollare TCP/IP. Di default si appoggia sulla porta 80 di [TCP](TCP.md).
Si basa sul concetto di request/response e sul modello [Client-Server](Client-Server.md).
##### Data contract
I dati scambiati tra client e server, cioè tra frontend e backend devono seguire un formato prestabilito; quello più usato è il JSON.
Nel contratto ogni endpoint dovrebbe inserire le informazioni relative a:
- I servizi esposti con il relativo URI per raggiungere tale servizio.
- Schema delle request e delle response, cioè il formato dei singoli attibuti. SI può fornire o solo il tipo dei campi oppure anche degli esempi.
# Verbi
I metodi di base sono operazioni CRUD denominate come:
- GET
	Serve per il recupero di una o più [risorse](REST.md#RISORSE) ed equivale quindi ad un filtraggio.
	In questo caso nella richiesta HTTP compariranno i parametri della richiesta. I parametri nel metodo che saranno recuperati dalla richiesta devono essere annotati con $@QueryParam$ di [JAX-RS](JAX-RS.md#Metodi).
- POST
	È usato per due scopi principali:
	- Creare una nuova risorsa. In questo caso si deve specificare il tipo di dati che il metodo consuma tramite l'annotazione [JAX-RS](JAX-RS.md#Input%20e%20output).
	- Eseguire un'azone su una risorsa. In questo caso si deve specificare il percorso della risorsa: nell'anotazione $@Path$ del [metodo](JAX-RS.md#Metodi) si useranno le parentesi graffe e nel metodo i parametri sanotanno annotati con $@PathParam$ di [JAX-RS](JAX-RS.md#Metodi).
- PUT
	Serve per l'aggiornamento di una risorsa.
- DELETE
	Serve per cancellre una o più risorse.
##### Ausiliari
Ci sono altri metodi che possono essere utili per casi specifici. DI solito sono poco usati:
- PATCH
	Serve per fare aggiornamenti parziali su una risorsa.
- HEAD
	Serve per ricere le informzioni di una risorsa.
- OPTIONS
	Serve per ricevere quali metodi sono supportati da una risorsa.
# Codici di stato
Ogni messaggio HTTP ha un codice di stato:
- $2xx$
	È un messaggio di conferma.
- $4xx$
	È un messaggio di errore da parte del client.
	- 400 è una bad request.
	- 401 è un unauthorized e si usa quando le chiavi di autorizzazione sono sbagliate.
	- 403 è un forbitten e si usa quando l'utente è autenticato ma non ha i permessi per accedere alla risorsa.
- $5xx$
	È un messaggio di errore da parte del server.
# Richardson Maturity Model
È un modello che definisce quanto chiaramete sono esposti i servizi al clinet, cioè con quanta facilità un frontend rappresneta un backend.
Non sempre è necessario raggiungere il livello massimo, ma si dovrebbero esporre delle REST API che almeno soddisfano il livello 2.
- 0
	Il protocollo HTTP è solo usato per il trasporto. Le funzionalità legate alle risorse sono solo esposte tramite tale protocollo e non c'è un corretto uso dei verbi e delle risorse.
- 1
	Si include il concetto di risorsa in modo gerarchioco.
- 2
	Si usano corretamente i verbi HTTP.
- 3
	Si implementa i controlli ipermediali, cioè si implementa [HATEOAS](REST.md#RESTful).
# Versioni HTTP
Ci sono due versioni HTTP.
![HTTP 1 vs 2](HTTP%201%20vs%202.png)
##### HTTP/1
- Utilizza un formato testuale come JSON per scambiarsi informazioni
- Durante una connessione si fa una richiesta e si riceve una risposta alla volta.
##### HTTP/2
È usata ad esempio da [gRPC](gRPC.md).
- Utilizza un formato binario.
- Permette il multiplexing, cioè l'invio di più richieste contemporaneamente.