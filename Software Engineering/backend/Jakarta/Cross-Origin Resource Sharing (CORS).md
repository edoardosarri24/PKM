La Cross-Origin Resource Sharing è un meccanismo di sicurezza per controllare se un dominio può fare richieste a un dominio diverso.
Di default i browser seguono la same-origin policy: se una richiesta [HTTP](HTTP.md) deriva da un dominio diverso da quello di arrivo allora la richiesta è bloccata.
Implementare un meccanismo che riscriva correttamente il comportamento del same-origin policy è quindi necessario.
# Flow
- Il client (chi fa la richiesta) inivia una richiesta a un server da qualche dominio.
- Il sevrr risponde con l'ACAO (Access-Control-Allow-Origin), cioè una lista di domini che possono fare richieste sul quel server.
- Il clinet, se è nell'ACAO esegue riesegue la richiesta, altrimenti no. Se non è nell'ACAO ed esegue comunque la richiesta non riceverà risposta.
# Implementazione
L'implementazione dell'ACAO deve avvenire lato server.
Ci deve essere una classe che implementa $javax.ws.rs.container.ContainerResponseFilter$ e ed esegua l'override del metodo filter.
Nel caso si voglia poter rispondere a tutte le richieste possiamo usare la wildcard $Access-Control-Allow-Origin:*$.