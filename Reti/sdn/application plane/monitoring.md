Il monitoring è un'applicazione dell'[application plane](application%20plane.md).
# Pre-SDN
Prima delle reti [sdn](sdn.md) il monitoraggio era strutturato con un'architettura client-server.
Supponendo di avere una serie di server collegati tramite router e un bounder router che espone il servizio all'esterno, allora possiamo eseguire il controllo su due entità:
- Bounder router
	Se monitoriamo il router che espone il servizio all'esterno allora oltre al traffico dati normale deve passare anche il traffico di monitoraggio. Questo può causare problemi se il controllo ha una grana molto fine o se il router è saturo o quasi saturo.
- Server
	Se il server deve elaborare anche le informazioni di monitoring e il carico è già elevato potrebbe essere scalato e creata un'altra macchina virtuale che realizza quel servizio. Il costo aumenta.
# Soluzione SDN
Con le reti SDN il monitoring diventa un'operazione distribuita nello spazio e nel tempo: non tutti gli switch si devono occupare del monitoring; possiamo scalarlo a seconda del momento.
##### Regole
La soluzione più semplice è quella di introdurre delle regole a livello di [OpenFlow](OpenFlow.md) che si occupano di rilevare una qualche condizione (e.g., una sorgente invia molti pacchetti secondo una qualche soglia), verificata la quale si manda un $Packet-In$ al controller. Il pacchetto al controller è fondamentale: uno [switch](data%20plane.md#Switch) è un dispositivo stupido, non può fare elaborazione o eseguire script: rileva solo le regole installate e notifica il controller.
##### Vantaggi
- Efficienza
- Scala bene per la gestione dinamica delle regole che possono essere aggiunte e rimosse.
- Scala bene da un punto di vista spaziale: possiamo aumentare la grana mettendo le regole in tutti gli switch.
# Classi di dispositivi
I dispositivi che si occupano del monitoring possono essere suddividi in due classi: monitors e watcher.
##### Monitors
Sono solitamente gli switch SDN. Gestiscono eventi tramite regole: quando un valore supera una data soglia catturano l'evento e lo inviano al controller tramite Packet-In.
In questo caso gli eventi sono definiti da counter che superano una data soglia: lo switch non può controllare flussi.
##### Watcher
Collezionano e analizzano flussi: ricevono una serie di pacchetti che compongono un flusso e lo analizzano per classificare e valutare il problema.
In questo caso non possono essere semplici switch SDN: devono raccogliere i dati ed applicarci algoritmi di machine learning per la classificazione dei problemi.
# Defense4All
È un esempio dove il monitoring è utilizzare per scopi di security, in particolare per prevenire attacchi DDoS. In [OpenDaylight](OpenDaylight.md) è incluso tra i moduli attivabili e già presenti.
##### Architettura
L'architettura su cui si basa defense4all è la seguente:
- Servizio
	È l'insime dei server che realizzano il servizio insieme agli switch SDN che collegano questi server.
- Controller
	È il controller ODL che gestisce il [data plane](data%20plane.md).
- Modulo
	È il modulo di defense4all. Solitamente gira nella stessa macchina del controller.
- Watcher
	Sono i server che elaborano le informazioni per classificare tramite algoritmi di machine learning i problemi. Possono essere server di terze parti oppure server interni all'azione che espone il servizio.
##### Pipeline
Il flusso è il seguente:
- Il modulo dice al controller di installare delle regole OpenFlow su pochi switch: il controllo è blando e sulla rete non passano molti pacchetti di controllo.
- I pacchetti raggiungono il modulo che li analizza. Quando questo rileva qualcosa che non va dice al controller di installare delle regole sugli swtich in modo che il traffico raggiunga anche gli watcher; magari non tutto il traffico ma campionandolo.
- Gli watcher eseguono analisi e classificano il traffico come DoS attack oppure no.
- Il modulo riceve l'informazione dell'analisi. Eventualmente dice al controller di installare delle regole per gestire l'attacco DoS.
- Gli watcher possono anche eseguire operazioni aggiuntive: ad esempio ripulire il traffico e mandarlo nuovamente sulla rete per raggiungere i server del nostro servizio; in questo modo non solo risolviamo l'attacco ma il servizio continua a essere funzionante.