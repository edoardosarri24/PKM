Sono algoritmi di [routing](Reti/routing/routing.md).
# Random
Un pacchetto, una volta che ha raggiunto un router, viene inoltrato su una porta di uscita scelta su base statistica (Di solito con distribuzione uniforme).
Sono algoritmi robusti perché si adattano bene ai cambiamenti della rete non previsti; non garantiscono però la scelta ottimale del percorso da seguire.
# Flooding
Un pacchetto, una volta che raggiunge un router, viene inoltrato su tutte le porte di uscita del router.
Riduce al minimo l’elaborazione all’intento del router e rende molto sicura la consegna del pacchetto al destinatario, tanto che viene usata in situazioni in cui l’affidabilità è necessaria (Applicazioni militari e Safety-Critical).
Lo svantaggio principale è il [broadcast storm](Reti%20di%20telecomunicazioni.md#Broadcast%20storm); per limitare questo problema ci sono due variazioni:
- Utilizzo del campo [TTL](IPv4.md#HEADER) nell’header IPv4 o [Hop limit](IPv6.md#HEADER) nell'heaader IPv6. Questo permette di non far circolare troppe copie del pacchetto all’interno della rete.
- Un router memorizza una copia di ogni pacchetto ricevuto; se lo stesso paccheto ritorna ad un router già attraversato può essere scartato.
##### Flooding selettivo
È una variante del flooding e si basa sull'idea che il pacchetto sia destinato ad un roter che si trova in direzione opposta a quella del mittente. Quando un router riceve un pacchetto lo invia quindi nelle porte di uscita che hanno direzione opposta a quella di ricezione.
# Direct diffusion
I dati presenti nella rete sono contrassegnati da un’etichetta costituita dalla coppia valore-attributo. È utilizzata nella rete di [sensori](WSN%20-%20Wireless%20Sensor%20Networ.md).
Quando un nodo vuole ricevere un’informazione, invia nella rete, attraverso la modalità flooding, il tipo di informazione che richiede (La coppia sopra specificata). I nodi che possiedono l’informazione gli comunicano le condizioni (Qualità dei dati, affidabilità, bit rate) con cui possono inviare i dati richiesti. Il richiedente sceglie quella che più lo soddisfa inviando sulla rete le caratteristiche scelte; il nodo che ha un riscontro procede con l’invio.
Il richiedente più eseguire anche un reinforcement, ovvero può diventare più specifico sulle condizioni che il percorso deve rispettare.
# Spin
Quando un nodo riceve o genera un pacchetto invia su base flooding i suoi metadati, cioè le informazioni su cosa il pacchetto contiene. I dati veri e propri vengono inviati solamente ai nodi che sono effettivamente interessati. È utilizzato nelle reti di [sensori](WSN%20-%20Wireless%20Sensor%20Networ.md).
In questo modo si riduce notevolmente il [broadcast storm](Reti%20di%20telecomunicazioni.md#Broadcast%20storm).
La nota negativa è che non tutti i nodi hanno la possibilità di ricevere il pacchetto, come nel caso in cui un nodo interessato ha tutti vicini che non ricevono il pacchetto perché non interessati.
# Source routing
Il nodo sorgente specifica all’interno dell’header il percorso che il pacchetto inviato deve seguire. Questa informazione viene acquisita dal mittente in due modi diversi:
- Path server
	Viene contattato un server che ha memorizzati tutti i percorsi sorgente/destinazione possibili. È una soluzione di tipo centralizzato e statica: se il server ha dei problemi l'interà rete si blocca; se la rete ha un problema e il server non viene aggionrato allora alcuni percorsi non saranno più dispobili.
- Path discovery
	Il nodo sorgente invia in flooding un pacchetto esploratore nel quale viene specificato solo l’ID del mittente e quello del destinatario; ogni volta che il pacchetto attraverso un nodo della rete si memorizza il suo ID; il primo pacchetto che raggiunge il nodo destinazione è quello che ha attraverso il percorso migliore; viene quindi rinviato al mittente seguendo il percorso inverso.