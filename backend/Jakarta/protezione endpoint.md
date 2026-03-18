Se riscriviamo il comportamento del [CORS](Cross-Origin%20Resource%20Sharing%20(CORS).md) rendiamo automaticamente il nostro sistema e le risorse al che gestisce tramite metodi [REST](REST.md) aperto a qualunque richiesta provenga dai domini indicati.
Dobbiamo quindi proteggere in qualche modo il nostro endpoint. Si deve quindi capire chi l'utente sia tramite l'[identificazione](Identificazione.md) e poi verificare che sia esattamente chi dice tramite l'[autenticazione](Autenticazione.md).
##### Tecniche
Ci sono varie tecniche:
- Coppia username password.
- OAuth2.0
	È uno standard aperto absato sullo scambio di parametri.
- OIDC
	È un'estensione di OAuth2.0 che include le informazioni utente.
- JSON Web Token (JWB)
	È uno standard open basato sullo scambio di access token in formato JSON descritto nell'[RFC-7519](https://datatracker.ietf.org/doc/html/rfc7519).
# Autenticazione
L'autenticazione solitamente avviene tramite JWT (JSON Web Token).
Si tratta di un token compatto e URL safe (i.e., trasportabile tramite protocollo [HTTP](HTTP.md)) che rappresenta le informazioni (claims) tra client e server. È identificabile in modo sicuro, cioè senza accedere al database.
È composto da tre parti: un header che contiene il tipo di token (in questo caso JWT) e l'algoritmo per la cifratura; il payload che contiene i claims, cioè le informazioni dell'utente; la firma, cioè la cifratura.
##### Flow
Il flow da seguire è: l'utente fa il login e il backend, se username e password coincidono con quelle memorizzate nel databse, autentica e genera un JWT; il client riceve il token e lo salva; lo invia insieme alle richieste successive in modo che il backend che espone il servizio possa capire chi è il richiedente e che permessi ha.
##### Scadenza
Il backend deve definire una scadenza per ogni JWT: in questo modo ogni token dopo un po' scade e non si deve controllare la sua presenza in una blacklist.
# Autorizzazione
Se l'applicazione gestisce gli utenti secondo ruoli diversi, allora dobbiamo implementazione autorizzazione.
Questa può essere role-based o condition-based.
##### Role-based
Per questo tipo di autorizzazione la specifica mette a disposizione delle classi e delle annotazioni che permettono di implementarla abbastanza semplicemente tramite degli intercettori di azioni.
I ruoli che sono ritenuti validi sono deifniti all'inntenro di un file $web.xml$.
- $javax.annotation.security.RolesAllowed$
	Definisce l'annotazione $@RoleAllowed$ definita sopra un metodo o una classe che prende come parametro una lista di stringhe.
	Vuol dire che quel metodo o tutti i metodi della classe possono essere invocati solo da quei ruoli.
- $javax.ws.rs.core.SecurityContext$
	È un'interfaccia che permette di definire il comportamento di metodi fondamentali.
- $javax.ws.rs.container.ContainerRequestFilter$
	È un'interfaccia che permette di personalizzare il comportamento che JAX-RS adotta a runtime in seguito di ogni request.
	Il metodo da implementare per forza è $filter$.
- Flusso:
	- L’utente invia una richiesta a cui allega il token JWT.
	- Il $ContainerRequestFilter$ intercetta la richiesta, verifica il token e costruisce un $SecurityContext$.
	- JAX-RS analizza le annotazioni $@RolesAllowed$.
	- Se l’utente ha almeno uno dei ruoli richiesti, l’accesso è consentito, altrimenti il server restituisce un codice http 403.
##### Condition-based
Servono per proteggere le risorse secondo delle condizioni aggiuntive. Ad esempio un utente con un certo ruole può accedere solo ad un certo tipi di risorsa.