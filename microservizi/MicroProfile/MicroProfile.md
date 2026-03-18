Mentre [Jakarta](Jakarta.md) è un insieme di specifiche per applicazioni principalmente monolite e pesanti, [MicroProfile](https://microprofile.io) estende le sue specifiche per adattarlo ad architetture a [microservizi](microservizi.md).
Queste specifiche sono implementate, tra gli altri framework, da [quarkus](quarkus.md).
# Specifiche
MicroProfile eredita alcune specifiche da Jakarta (e.g., [JAX-RS](JAX-RS.md), [CDI](Context%20and%20Depencency%20Injection%20(CDI).md)) e le estende con altre specifiche relative ai microservizi.
Queste nuove specifiche si dividono in MicroProfile platform e standalone (e.g. [GraphQL](GraphQL.md)): le prime sono rilasciate tutte insieme e sono il core di MicroProfile; le seconde sono rilasciate singolarmente e sono state aggiunte in un secondo momento.
##### MicroProfile platform
Alcune delle specifiche core sono:
 * [REST client](REST%20client.md)
	Per chiamare altri servizi REST in modo dichiarativo.
- Config
	Per gestire la configurazione dell'applicazione da diverse fonti (file, variabili d'ambiente, etc.) in modo unificato.
- [health](health.md)
	Permette di esporre infromazioni binari sulla salute di un servizio.
* Fault Tolerance
	Fornisce meccanismi per rendere i servizi resilienti ai guasti in modo semplice tramite annotazioni.
* [metrics](metrics.md)
	Esporta automaticamente metriche di telemetria (e.g., tempi di risposta, numero di richieste) o metriche business in un formato standard (i.e., [Prometeus](https://prometheus.io)).
* OpenAPI
	Formato [OpenAPI](OpenAPI.md) per la documentazione delle API REST.
- Telemetry
	Specifica per il tracciamento delle telemetrie, metriche e logs che si appoggia a [OpenTelemetry](OpenTelemetry.md): non è stata ridefinita da MicroProfile ma utilizza lo stardard de facto OTel.
	Per le metriche Telemetry non è ancora stambile, quindi si usa di solito [MicroMeter](MicroMeter.md).
- JWT authentication
	Permette di autenticarsi e gestire le varie funzionalità.
# SmallRye
Se MicroProfile espone delle specifiche, SmallRye è l'implementazione standard di queste specifiche.
Il framework [quarkus](quarkus.md) lavora molto a contatto con SmallRye: le estensioni di Quarkus si basano su queste implementazioni e sono ottimizzate per permettere ad applicazioni Quarkus di risparmiare memoria e avere uno start time più efficiente.