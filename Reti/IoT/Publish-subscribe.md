Un'alternativa al tradizionale modello di comunicazione sincrono basato su request e response (RPC, Remote Procedure Call) visto per [CoAP](CoAP.md), è il così detto message system (MS).
È un modello asincrono che permette la comunicazione tra sender e consumer; questi sono disaccoppiati da un server, che fa da intermediario.
In questo modo si aumenta la scalabilità del modello RPC dove il server era il collo di bottiglia (si doveva attendere che rispondesse a chi era arrivato prima); adesso il collo di bottiglia è solo la banda della rete.
# Risorse
- [IBM](https://www.ibm.com/it-it/topics/message-brokers)
- [Kafka](https://kafka.apache.org)
- [Microsoft](https://learn.microsoft.com/en-us/dotnet/architecture/microservices/architect-microservice-container-applications/asynchronous-message-based-communication)
# PUBLISHER-SUBSCRIBER
Il publisher-Subscriber si basa sul concetto di MS. Gli attori sono:
- Publisher
	Invia messaggi al broker relativi a un topic (un topic è una risorsa, cioè un URI).
- Subscriber
	Si iscrive a un topic e riceve una copia di tutti i messaggi che sono stati invia dal publisher relativi a quel topic.
- Broker
	È l'intermediario, che sarà poi effettivamente un gateway. Viene implementato come un insime di code (nel senso della struttura dati) di messaggi, dove ogni coda è relativa a un topic.
### Conseguenze
I messaggi sono inviati a più consumatori. È simile a un dynamic multicast: dynamic perché i publisher e subscriber si possono aggiungere e si possono rimuovere senza problemi; multicast perché implementa una comunicazione uno a molti.
##### Vantaggi
- Disaccoppiamento
	Publisher e subscriber sono disaccoppiati: i primi non devono sapere chi riceve il messaggio e per assurdo neanche se qualcuno riceve il messaggio. Nel paradigma client server il client deve conoscere il server.
- Scalabilità
	Il broker non fa altro che inoltrare i messaggi e potrebbe essere duplicato per aumentare il volume dei dati scambiati.
- Leggerezza
	Publisher e subscriber possonoe essere oggetti leggeri, visto che il loro compito è solo quello di comunicare con il broker.
##### Svantaggi
- Non c'è una comunicazione diretta tra gli endpoint. Questo implica che non ci può essere una negoziazione di alcun tipo, come ad esempio nel formato dei dati.
- Ogni cambiamento sul formato dei dati è un problema lato subscriber. Per questo motivo l'evoluzione del sistema nel lungo periodo è complessa.
- Non c'è sicurezza E2E perché se un messaggio viene cifrato le chiavi non possono passare direttamente tra i due endpoint ma dovranno passare dal broker.
- I messaggi arrivano dopo rispetto al paradigma client/server a causa del broker. Si deve evitare che il broker abbia picchi di carichi in entrata o in uscita.
- Il broker diventa un single point of failure e il collo di bottiglia del sistema.
# MESSAGING QUEUE
È una versione del MS diversa dal publisher-subscriber.
È un modello asincrono di comunicazione che disaccopia qundi il sender e il consumer.
Il server che disaccoppia sender e consumer riceve messaggi e li inserisce in una coda. I messaggi nel server vengono salvati finchè non arriva un consumer che li preleva.