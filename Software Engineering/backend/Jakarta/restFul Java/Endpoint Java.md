Un endpoint è un terminale che mette a disposizione delle [API REST](REST.md) per esporre dei servizi. Per la creazione degli endpoint si utilizza un'implementazione della specifica [JAX-RS](JAX-RS.md).
In questo documento valutiamo l'implementazione di un endpoint per un'applicazione [Jakarta](Jakarta.md). Per fare questo ci servono almeno due classi.
# MyApplication
Definisce la URI base per l'applicazione.
- Non necessita di metodi o campi.
- La classe è annotata con $@applicationPath("path")$.
# MyEndpoint
È responsabile di offrire il punto di accesso a una risorse tramite operazioni CRUD.
##### Annotazioni
- La classe è annotata con $@Path("path")$ e rappresenta l'URL per la risorsa gestita. Si possono annotare anche i metodi in modo da avere URL gerarchiche; in questo caso dobbiamo usare anche $@PathParam$ per gestire l'URL del parametro.
- Ogni metodo deve essere annotato con l'operazione CRUD corretta, in modo da soddisfare il [Richardson Maturity Model](HTTP.md#Richardson%20Maturity%20Model).
- Ogni metodo può essere annotato con $@Produce$ $@Consume$, a seconda se produce o consuma qualcosa.
##### Service core
Se vogliamo implementare un metodo $ping$ che restituisce una stringa, allora possiamo fare un $return\ Response.ok().entity("Online").build();$, dove:
- $Response$ è una classe importata da $javax.ws.re.core$.
- $entity(Object\ obj)$ imposta l'entity body della response HTTP e prende come parametro un qualunque oggetto.
- $ok()$ è un'abbreviazione del metodo $status(Response.Status.Enum)$, dove l'Enum è uno degli [stati di HTPP](HTTP.md#Codici%20di%20stato). Quando vogliamo implementare un early return il pacchetto $javax.ws.rs$ mette a disposizione una gerarchia di eccezioni che permettono di ritornare un codice di stato preciso con un messaggio personalizzato.