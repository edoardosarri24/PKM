GraphQL è un framework che implementa le specifiche [MicroProfile](MicroProfile.md) e permette al client di specificare cosa richiedere. Si basa in ogni caso sull'architettura [REST](REST.md) e quindi su un meccanismo di comunicazione sincrono in cui viene inviata una request, magari anche complessa, a sequito di cui arriva un response.
# Vantaggi
Tramite il protocollo [REST](REST.md) un client non ha il potere di fare delle richieste al server: il client chiama un servizio del server e questo restituisce tutto quello che aveva previsto per quel servizio. Una volta che il client riceve la risposta, solitamente in un JSON, è lui che deve filtrare i dati in base a quello che gli interessa; questo viene fatto tramite la [deserializzazione](estensioni.md#Jackson) di un file JSON.
Tramite GraphQL il client specifica esattamente cosa vuole. In questo modo non è il client che deve filtrare quello che riceve, ma riceverà esattamente quello che gli serve.
In questo modo si evita l'overfeatching e l'underfeatching, cioè richiedere più informazioni di quelle necessarie o fare più richieste per ottenere più informazioni; per questo motivo GraphQL è ottimo per gli ambienti vincolati dove il numero di richieste alla rete deve essere limitato.
# Funzionamento
Il server espone uno schema GraphQL, cioè un contratto a cui il client deve sottostare per comunicare con il server; lo schema non dice altro come devono essere chiamate le operazioni che il server mette a disposizione.
Molti framework che implementano GraphQL, come [quarkus](quarkus.md), permettono di utilizzare le classi direttamente, senza dover tradurle nello schema corretto.
##### Annotazione
Quello che si deve fare è aggiungere un'interfaccia lato client e annotarla con $@GraphQLClientApi(configKey="key")$. La chiave se per riferirsi a quel client con un nome; in questo modo si può ad esempio impostare delle configurazioni personalizzate per quell'interfaccia client GraphQL. Ad esempio se si deve configurare la porta a cui il server deve rispondere.
Nell'interfaccia si dichiara i metodi del server che al client interessa chiamare e si annotato con l'annotazione dell'operazione corretta; il parametro dell'annoazione è una stringa che deve essere uguale al nome del metodo lato server che deve essere chiamato.
##### Injection
Se ci serve fare un'injection si deve utilizzare l'annotazione $@GraphQLClient("nome")$, dove $nome$ è uguale alla $configKey$.
# Data storage
GraphQL non si preoccupa di dove i dati sono memorizzati: potrebbero essere sia in un data base che in memoria centrale.
Questo permette al server di esporre un'interfaccia unica a prescindere dal modo, o dall'insieme di modi, che usa per memorizzare i dati.
# Operazioni
Ci sono tre tipi di operazioni messe a disposizione. I metodi che saranno implementati dalla classe client dovranno essere annotati con le relative annotazioni.
- Query
	È l'equivalente di una GET REST, cioè un'operazione read-only.
- Mutation
	È l'equivalente di una PUT o POST REST, cioè è un'operazione read-write.
- Subscription
	Non esiste un'operazione simile in REST. La risposta è uno stream potenzialmente infinito di oggetti. Viene solitamente implementato tramite delle socket.