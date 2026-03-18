REpresentational State Transfer (REST) è uno stile architetturale basato su [HTTP](HTTP.md) definito nel 2000 che ha l'obiettivo di scalare in termini di numero di client interattivi ed evoluzione del sistema.
È una delle possibilità per implementare un [SOA](Service%20Oriented%20Architecture%20(SOA).md#Implementazioni).
# RESTful
Un'applicazione RESTful segue determinati principi:
 - Si basa sul concetto di risorsa, cioè di informazioni e dati. Le risorse sono trasferite dal provider all'utente tramite una specifica sintassi dell'URL del servizio. I formati usati spesso sono i file JSON.
- Manipolazione delle risorse tramite la loro rappresentazione.
- Espone i propri servizi tramite le REST API, che i client possono chiamare e usare tramite richieste HTTP.
- Messaggi auto descrittivi.
	Nell'architettura REST è fondamentale che le richieste siano stateless, cioè sia il client che il server non devono memorizzare alcuna informazioni l'uno dell'altro.
	Ogni messaggio deve essere quindi auto descrittivo, cioè deve contenere tutte le informazioni necessarie per la sua elaborazione senza che il server sia a conoscenza del contesto (stato del client).
- HATEOAS (Hypermedia As The Engine Of Application State)
	Il web può essere modellato come un FSM (Finite State Machine). L'idea è che il client non sia a conoscenza della struttura delle API del server, ma possa comunque navigare nell'applicazione in modo dinamico tramite hypermedia.
	Il concetto è che il server (cioè il backend) includa nelle repose delle informazioni aggiuntive con le URI per i servizi offerti da quel momento.
	Un vantaggio è che il backend può cambiare le URI dinamicamente senza rompere il frontend.
# Risorse
Una risorsa è un'astrazione dell'informazione. Non mappa uno a uno un'entità del [domain model](domain%20model.md), ma può incorporare più o meno informazioni.
Le risorse e le operazioni possibili su di esse sono gestite da nodi detti [endpoint](Endpoint%20Java.md). Un endpoint rende fruibile un servizio in modo univoco e stabilisce i livelli di protezione di una risorsa.
##### Rappresentazione
La rappresentazione di una risorsa è il suo stato (il suo valore) in un dato momento ed è ciò che viene scambiato tra due end-point (client e server).
Il tipo con cui viene rappresentata una risorsa, uno dei tanti formati usti sul web (di solito JSON o XML), è espresso nell'header (HTTP Content-Type header) del messaggio che contiene la risorsa.
##### Identificativo
Una risorsa è identificata all'interno del web in modo univoco da un URI (Uniform Resource Identify). Tra URI e risorsa c'è una relazione $1:n$, cioè una risorsa può essere associata a più URI ma un URI è associato al meno una risorsa.
Ci sono due tipi di URI: l'URL (Uniform Resource Locator), che identifica una risorsa e permette chiamare metodi per agire su di essa; l'URN (Uniform Resource Name), che identifica una risorsa tramite un nome in un particolare dominio e che ci interessa il giusto.
Le URI possono essere:
- Gerarchiche
	$GET\ http://myhost.webapp.it/rest/myresources/$, dove:
	- $http://$ è il verbo HTTP.
	- $myhost.webapp.it/rest$ è la base URI.
	- $myresources/$ è il percorso dell'endpoint.
- Contestuali
	Si vuole poter trovare una risorsa in base a un contesto aprametrizzato. È la meno usata.