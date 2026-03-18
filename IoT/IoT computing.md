![gerarchia cloud fog edge](gerarchia%20cloud%20fog%20edge.png)
# CLOUD COMPUTING
Definiamo il cloud computing: permette l'accesso on demand a un pool condiviso di risorse, che sono rapidamente configurabili e rilasciati con il minimo sforzo di gestione o interazione con il fornitore di servizi.
Questo vuol dire che:
- Abilita l'accesso alle risorse, quindi si tratta di un server.
- Le risorse sono configurabili in modo autonomo senza che ci sia uno sforzo o un controllo da parte di chi fornisce queste risorse.
- Le risorse sono potenza di computazione, memoria e rete.
##### Server
In generale quando si hanno più server collegati tra loro tramite una intranet si parla di data center. I data center diventano cloud quando si espongono all'esterno tramite internet.
### IoT e cloud
Quando si parla di Cloud of Things (CoT) si parla spesso di bigdata techniques: i dispositivi non comunicano tra loro, ma il loro unico scopo è quello di inviare dati al cloud; i server cloud memorizzano dati eterogenei e su questi eseguono analisi complesse oppure predizioni (se siamo nel mondo del ML). Sono servizi scalabili e robusti.

Implementare uno scenario IoT tramite il cloud computing (CoT), cioè un servizio real time, solleva determinati problemi.
- Il cloud è lontano, mentre noi vorremmo un'elaborazione più vicina all'edge per ridurre al minimo la latenza.
- Nel cloud si perde il contesto, sia del luogo dove vengono generate le informazioni sia del momento che degli spostamenti (mobilità).
- Il cloud è usato per fare analisi su grandi quantità di dati dopo che sono stati depositati.
### Modelli di servizio
##### IaaS
Sono le Infrastructure as a service, con cui si vende potenza computazionale, reti e macchine virtuali.
Gli utenti hanno il controllo sull'OS, sullo spazio di memoria e sulle applicazioni.
Un esempio è AWS.
##### PaaS
Sono le Platform as a service, con cui si vende anche l'OS e lo spazio di memoria.
Gli utenti hanno il controllo sulle applicazioni.
##### SaaS
Il cliente acquista un servizio e non un prodotto software: la proprietà del software rimane del produttore, che si dovrà occupare anche della manutenzione e degli aggiornamenti.
L'utente acquista il permesso di usare il software per un determinato lasso di tempo; se è gratuito allora il modello di business si basa sulla raccolta dati.
# FOG COMPUTING
Si vuole computare più vicino ai dispositivi IoT rispetto al cloud, evitando quindi i relativi [problemi](IoT%20computing.md#IoT%20e%20cloud) del CoT. In questa idea i dati non sono inviati direttamente al cloud, ma vengono elaborati da server fog.
I server fog sono solitamente a pochi hop dai dispositivi. I vantaggi che si ottengono sono:
- Poca latenza, perché la distanza è poca (proximity), ottendno risultati e analisi in real time.
- I server fog sono a conoscenza del contesto (context awarness).
- Si percepiscono ancora i flussi di dati e l'IP del destinatario (stream oriented), sapendo esattamente da chi viene quale informaioni.
### Sincronizzazione
Ovviamente un solo server fog non ci basta per problemi di fault tollerance (le cose si rompono) e per scalabilità (per un traffico maggiore serve più potenza computazionale e memoria). Per questo motivo un aspetto fondamentale è quello della sincronizzazione.
I vari server fog, che possono mettere in relazione anche reti IoT diverse, devono essere orizzontali ed essere coordinati (sincronizzati). L'obiettivo è quello di apparire al livello edge come se fossero una macchina singola, cioè i dispositivi non devono pensare che le risorse siano distribute.
Questo è un aspetto molto diverso rispetto al livello cloud: esso è infatti molto verticale, nel senso che cloud diversi non sono in relazione gli uni con gli altri, ma c'è una centralizzazione dei servizi offerti.
##### Proxy
Per la sincronizzazione i dispositivi fondamentali sono i proxy, già visti e descritti nell'architettura [CoAP](CoAP.md#PROXY).
Il compito principale di questi proxy è quindi quello di far parlare tra loro reti IoT diverse e di farle comunicare con il cloud.
Una possibile soluzione è implementare i proxy all'interno del [boarding router](IoT%20computing.md#Comunicazione%20con%20l'esterno).
### Server fog
Ci possono essere due tipi di server fog:
- Fog nodes
	I server fog sono server general purpose su cui installare una o più virtual machine e su cui poi distribuire i processi necessari.
	Chi gestisce le VM è l'hypervisor, un componente sw che risiede sopra l'OS e che permette di astrarre le risorse del sistema. Il suo obbiettivo è quello si installare più VM, duplicarle e sincronizzarle, così che le risorse locali del nodo siano condivise da più OS.
- IoT Hub
	Un fog node è un server fog generico, sul quale sono installate VM che poi gestiscono processi SW per svolgere i vari compiti.
	Un container non si appoggia sull'hypervisor perché interagisce direttamente con il kernel.
	Usando i containers si realizza qualcosa di più leggero e specializzato; i nodi che utilizzano queste tecniche sono detti IoT Hub. Ci stiamo in questo spostando da un paradigma centralizzato a uno decentralizzato.
### Stack
I livelli dello stack che un server fog deve implementare sono:
- Network L3 - Deve svolgere funzionalità di routing tra le varie reti IoT.
- Application L5 - Deve svolgere le funzionalità del proxy C2C (CoAP2CoAP) e H2C (HTTP2CoAP).
### Funzionalità
- Resource directory
	È la [Resource directory](CoAP.md#Resource%20directory) di CoAP. Al link avevamo detto come funzionava, adesso capiamo a chi viene chiesta.
- Cache
	Memorizza le risorse viste frequentemente e di recente; possono essere anche predette se si usano tecniche di ML. Evita un eccessivo carico di memorizzazione sui dispositvi IoT e minimizza la latenza per ottenere queste risorse.
- Replica manager
	Sincronizza e coordina gli IoT Hub e le loro repliche.
- Broker
	Nel contesto del publish/subscriber l'IoT Hub ha anche la funzione di broker.
##### Conseguenze
-  Quando abbiamo detto che il [collo di bottiglia](publish-subscribe.md#Svantaggi) nel sistema publish/subscriber era il broker, ci rifacevamo all'IoT Hub. Inoltre è anche un single point of failuer perché se crolla questo le reti IoT diventano non raggiungibili e perdono la possibilità di fare analisi in real time.
- I vari IoT Hub possono essere replicati garantendo che ogni replica abbia le stesse funzionalità. In questo modo si permette l'accesso fisico a più hub, ma logicamente questo è uno solo. Si riesce così a distribuire il carico su tutte le repliche, garantendo disponibilità di accesso alle risorse. In questa situazione per far comunicare i vari IoT hub si utilizza un metodo di comunicazione suplisher/subscriber (come [MQTT](MQTT.md)).
##### Esempi
- [observing resource](observing%20resource%20fog%20example.jpeg).
- [polling resource](polling%20resource%20fog%20example.jpeg).
- [pushing resource](pushing%20resource%20fog%20example.jpeg).
# EDGE COMPUTING
Quando si parla di edge computing si intende la comunicazione e l'elaborazione dei dati all'interno della rete formata dai dispositivi IoT. Siamo quindi in un modello dove gli oggetti si bastano da soli e sono indipendenti da altre reti.
Nella realtà è troppo stringente perché i dispositivi IoT sono semplici e non possono includere molte funzionalità necessarie.
##### Implementazioni
Come implementazioni del livello applicazione dei vari dispositivi IoT possiamo avere diverse soluzioni:
- La prima alternativa che possiamo considerare è quella diutilizzare il modello client/server dove l'application layer è HTTP. In questo modo non si ha successo per la verbosità e la pesantezza di HTTP.
- Una soluzione successiva che è stata considerata, sempre basata sul modello clinet/server, è quella di utilizzare [CoAP](CoAP.md). In questo modo si ottengono tutti i vantaggi di CoAP, ma siamo ancora in un modello che impone la presenza di un server per compiere elaborazioni pesanti.
- L'ultima alternativa considerata è quella di utilizzare il modello [publisher/subscriber](publish-subscribe.md#PUBLISHER-SUBSCRIBER) e una delle sue implementazioni come [MQTT](MQTT.md).
##### Comunicazione con l'esterno
Le singole reti IoT per eseguire calcoli più complessi devono entrare in relazioni sia con livelli più astratti (fog e cloud) sia con altre reti.
Il primo dispositivo che mette in cumunicazione la rete IoT con l'esterno è detto boarding router.
# LAMBDA ARCHITECTURE
![lambda architecture](lambda%20architecture.png)
Si tratta di un paradigma nato nel 2015, che è strutturato in tre layer:
- Batch
	È il livllo che rappresenta il cloud. Memorizza l'intero dataset dati immutabile e che vengono aggiunti in batch uno di seguito all'altro (appends-only).
- Serving
	Carica i dati del dataset in batch e mette a disposizione degli strumenti per fare delle query su questi batch. Cerca di rispondere con bassa latenza a interrogazioni su tutto il dataset.
- Speed
	Mantiene solo gli ultimi dati, quelli che ancora non sono stati caricati nel dataset. Permette di fare richieste in real time sui dati che ancora il serving layer non ha visto.
### Proprietà
- Fault tollerance
	Il batch layer contiene tutti i dati e impedisce così gli errori umani.
- Scalabilità
	È scalabile perchè a ogni livello si può aggiungere altro HW oppure altri componenti SW tipo altre viste.
- Ad-hoc query
	Il batch layer può essere interrogato per eseguire query ad-hoc, sempre che non si desideri una bassa latenza (in questo caso si deve interrogare il serving layer o lo speed layer).
- real-time query
	Grazie allo speed layer si possono eseguire operazioni e richieste in real time.
- Machine learning
	Si possono aggiungere soluzioni di ML in out al sistema, cioè nella parte di interrogazione. L'IoT invece è dalla parte di input e produce nuovi dati.
### Esempio
Un esempio di applicazione della lambda architecture è quella legata al così detto kafka broker.
Si usa in sistemi distribuiti basati su microservizi. I microservizi sono elementi fondamentali di un’applicazione complessa che devono essere orchestrati.
Per la comunicazione si sceglie il modello publisher/subscriber; in questo caso il topic diventa il microservizio (siamo in un'architettura service-oriented, SOA).
Come si vede dall'[esempio](kafka%20lambda%20example.jpeg) quando arriva un nuovo dato il dispositivo IoT viene etichettato come publisher. Lo speed layer e il batch layer sono invece subscriber che ricevono il dato. Le viste, che necessitano dei dati di questi due layer, possono essere a loro volta subscriber.
- [Altro esempio](kafka%20example.jpeg)