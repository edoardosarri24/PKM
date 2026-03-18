JAX-RS (Java API for RESTful Web Services) è una specifica standard di [Jakarta](Jakarta.md) per creare [servizi web](Service%20Oriented%20Architecture%20(SOA).md) con uno stile [REST](REST.md) lato server, serve quindi per esporre degli end point.
Se vogliamo definire dei clinet che consumano i servizi offerti da un server si può utilizzare la specifica di [MicroProfile](MicroProfile.md).
# Specifica vs implementazione
Trattandosi di una specifica non è un framework completo, ma solo un documento tecnico che definisce a cosa deve sottostare un'implementazione per poter essere utilizzata in un'applicazione Jakarta.
Le implementazioni più famoso sono Jersey, RESTEasy, CXF e Helidon.
##### Pacchetti
Quando Jakarta è passata sotto l'Eclipse Foundation i pacchetti, dove si trovano classi e annotazioni sono stati rinominati da $javax.ws.rs$ a $jakarta.ws.rs$.
##### Funzionamento
Essendo una specifica, una volta scelta l'implementazione quello che si fa è usare le annotazioni. Il server a questo punto scansione le annotazioni e si occupa del routing delle richieste, dependecy injection e generazione dei JSON o XML.
# Annotazioni
##### Risorse
La parte fondamentale di JAX-RS sono le risorse, cioè quelle classi annotate con $@Path("/path")$.
##### Metodi
I metodi delle classi risorse devono essere annotati con i nomi dei [verbi HTTP](HTTP.md#Verbi) e con il relativo percorso, cioè con l'annotazione $@Path$.
In base al tipo ti verbo si useranno annotazioni specifiche sui parametri, come $@PathParam$ e $@QueryParam$.
##### Input e output
Per specificare che tipo di dati (e.g., JSON, XML e text/plain) il servizio accetta e restituisce si usa:
- $@Consumes$
- $@Produces$