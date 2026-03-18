Ha l'obiettivo di prevenire o eliminare perdite di prestazioni relative ai ritardi o ad un throughput (Unità traferiva per istante di tempo) troppo elevato dei trasferimenti.
In una rete di telecomunicazioni la congestione si presenta quando il numero di pacchetti inviati è così alto da saturare la capacità dei collegamenti, arrivando molto lentamente o non arrivando a destinazione.
## CONTROLLO PREVENTIVO
Hanno come finalità quella di prevenire la congestione prima che si manifesti.
### Leaky bucket
Ha l'obbiettivo di mantenere costante la quantità massima di pacchetti inviati nella rete.
I pacchetti in arrivo su un nodo non sono traferisti subito, ma vengono inseriti in un buffer che rispetta la politica FIFO e che ha una capacità massima definita; quando i pacchetti in arrivo non trovano spazio nel buffer vengono scartati.
In questo modo si garantisce una frequenza massima di inoltro non superiore al valore di riferimento consentito dalla rete.
### Token bucket
Ho l'obiettivo di mantenere costante la quantità media di pacchetti inviati nella rete.
Nella soluzione leaky bucket si perdono le possibilità di inoltro quando il buffer è vuoto; con questa soluzione si permette di conservare queste autorizzazioni in un buffer; la quantità di autorizzazioni conservate è limitata in modo da impedire la monopolizzare della rete da parte di un nodo.
## CONTROLLO REATTIVO
Hanno come obiettivo quello di eliminare la congestione appena questa si presenta.
### Sliding window
Questo metodo richiede la definizione di un parametro, detto ampiezza di finestra, che rappresenta il numero di pacchetti che possono essere trasmessi in sequenza affinché il primo riceva il messaggio di ACK, cioè riceva la conferma di ricezione da parte del destinatario. Nel momento in cui non arriva il riscontro si cerca di gestire la congestione; lo si fa chiudendo la finestra, cioè non inviando più pacchetti.
Questo meccanismo non garantisce all’utente un numero minimo di pacchetti inviabili per unità di tempo.
Un caso pratico di questa tecnica è il modo in cui si gestisce la congestione nel protocollo [TCP](TCP.md): si da la possibilità ad un nodo di regolare in modo autonomo l’ampiezza della finestra (Self-clocking). Ci sono tre tecniche:
- Slow Start
	Quando si apre il collegamento la grandezza della finestra è di un pacchetto. Siccome non si può conoscere a priori la dimensione massima della finestra si raddoppia la sua dimensione ad ogni iterazione, fino a quando non si smette di ricevere il pacchetto ACK entro il tempo di time-out per ogni pacchetto inviato; quando questo si verifica si dimezza la dimensione della finestra, memorizzando il valore massimo raggiunto.
- Congestion avoidance
	Terminata la fase di slow start il protocollo TCP entra in questa fase. Ripartendo con una finestra pari alla metà della dimensione sulla quale si è arrestato il sistema slow start, le sue dimensioni vengono aumentate di 1 pacchetto ad ogni iterazione; quando un pacchetto non riceve il proprio messaggio di ACK, si dimezza la dimensione della finestra.
- Fast recovery
	Prevede l’identificazione di due tipi di congestione, una più lieve e una più forte: se il tempo di time-out è scaduto allora la congestione è maggiore e si riparte dalla tecnica di slow start o di congestion avoidance; se invece si ricevono i messaggi di ACK in ordine errato allora la rete è meno congestionata.