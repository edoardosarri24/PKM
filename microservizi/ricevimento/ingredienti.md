# Kubernetes
##### Scaling
- Orizzonale
	Lo scaling orizzontale di Kubernates è reattivo: una volta raggiunta una soglia o quando ci si allonta troppo da essa scatta l'aggiunta o la rimozione di repliche. Se mappiamo questo concetto nei modelli si può dire che Kubernetes usa la coda (in realtà l'utilizzo di cpu) come proxy per lo scaling: se il numero di job in attesa cresce allora si aumentano le repliche.
	Ci sono dei lavori che fanno scaling su base predittiva: in base al pattern di arrivo dell'ultimo intervallo di tempo si cerca di prevedere quando il carico aumenterà per fare scaling in modo anticipato.
- Verticale
	Si sta variando la quantità di risorse di un container, cioè di milliCPU.
# Container
##### Docker
# Modelli
Ci possiamo porre problemi tipi se ho un workload di una certa intensità cosa succede se mantengo invariato il numero di repliche. In base a questo decidere se aumentare o ridurre il numero di repliche.
##### Analisi predittiva
##### Analisi prescrittiva
# Kafka
È il broker di Apache. È lo stato dell'arte per la messaggistica asincrona.
L'idea è che kafka riceve quaclosa ad un primo livello. Ad un certo punto i messaggi vengono trasferiti su una coda in base a delle categorie dove i subscriber possono leggere (come si legge da un file C). Kafka garantisce una qualche replicazione delle categoria su più gode, ma non una replica 1:1.
##### Microservizi
Viene usato nei micro servizi nelle situazioni in cui si vuole disaccoppiare mittente e ricevente.
Viene usato anche per fare reliability, prorpio per il discorso delle repliche.
##### Batching
L'invio dei messaggi tra il primo livello e le code avviene o quando il primo livello raggiunge una dimensione $n$ di batch size, oppure quando scade un time out $T$ (si può discutere se il time out parte dall'ultimo invio o dal primo passeggero arrivato). Con kafka si pssono programmare sia T che batch size.
Questo concetto si può tradurre in un modello analitico dove nello stato inziale si soggiorna 0 (transizione immediata) e poi abbiamo due stati: il primo rappresenta la situazione in cui si inviano i messaggi perché è scaduto il time out; il secondo perché si è raggiunto il batch size.
- Primo
	Suppongo il batch size come x. Se gli arrivi sono esponenziali io faccio il time out se arrivano meno di x arrivi entro T. Siccome faccio 1000 volte un'esponenziale la probabilità di andare di qua è pari a un'earlang con fase x sia maggiore di T, cioè che x esponenziali ci mettano ad arrivare più di T.
	Dopo il time out non si sa quanti messaggi sono arrivati (sicuro meno di x). Il numero di messaggi arrivati entro T è una variabile aleatoria di Poisson (definita da $\lambda$) troncata al fatto che sia minore di x; in generale la distribuzione di poisson ci dice il numero medio di eventi in un intervallo di tempo.
- Secondo
	I messaggi trasmessi sono batch size, ma il problema è il tempo di soggiorno. Il tempo di soggiorno è una variabile aleatoria ed è quanto tempo ci vuole ad accumulare batch size messaggi, condizionata all'ipotesi che sia meno di T.
##### Overhead
Data la programmabilità di batch size e T, se si fa più grande il batchsize è più efficiente, perché se devo replicare un messaggio alla volta ogni volta si deve inizializzare la sessione e così via. Se però si fa bathcsize troppo grande si degrada la latenza cioè quanto il messaggio aspetta prima di essere inviato.
# Lavoro su Kubernetes
- Realizzare un test bed che mette dei servizi dentro container.
- Calcolare analiticamente le risorse da attribuire a ogni container. Si tratta di coordinated provvisioning, cioè evitare che qualcuno abbia più risorse del dovuto mentre qualcun altro abbia meno risorse di quelle necessarie. Calcolare i rapporti in modo da realizzare una configurazione iniziale bilanciata ci riesce bene.
- Generare carico. Per questo c'è un lavoro di un tesista (Tommaso) che ha fatto un sistema in cui si generano dei micro servizi che fanno cose fittizie (tengono solo occupata la CPU senza fare chissà cosa) e poi si può generare il grafo delle dipendenze, cioè quale è il flusso di richieste end2end. Infine si può calcolare il tempo end2end.
- Prendere delle misure su quello che succede. Il lavoro di Tommaso non prende chissà quale misura. Dato il grafo delle dipendenze non è banale calcolare la distribuzione dei tempi. Quello che si può fare è prendere delle misure in questo senso, cioè tramite qualche tool di telemetria; esempi di tool sono Jaeger, Istio e Grafana.
- Dopo le misure si può osservare se il modello teorico è in grado di prevedere il comportamento della realtà. Oppure, più interessante è vedere se le decisioni prese sul modello teorico sono valide nella realtà.
# Lavoro su Kafka
In letteratura è stato spiegato il compromesse tra batchsize e T solo in relazione a esempi concreti (cioè con una data configurazione), ma mai in modo analitico e generale.
L'esercizio sarebbe avere un kafka broker che gira un qualche container, qualcuno che genera carico (i conti esatti li sappiamo fare se il carico è poissoninano) che gira in un altro container e dei consumer (subscriber) che fanno cose. Il tempo che ci mettano a fare le cose i consumer non ci interessa, ma quello che potrebbero fare è fare delle misure. Ad esempio si possono fare delle misure di tempi end2end.