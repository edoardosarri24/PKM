La rete DQDB (Distributed Queqe Double Bus) appartiene allo stardard [IEEE802](IEEE%20802%20-%20LAN.md). Si rivolge alle reti MAN ad accesso multiplo con topologia a doppio [bus](Topologia.md#BUS): i due bus sono unidirezionali e hanno verso di propagazione opposto; l’accesso alla rete è ordinato e avviene tramite politica FIFO.
Un bus viene acceduto per la prenotazione della rete e un’altro per la trasmissione delle informazioni; il bus di prenotazione è quello che ha verso opposto rispetto alla direzione di invio dei dati. L’accesso ai due bus avviene su base temporale: in ogni slot di tempo un nodo può accedere ad un solo pacchetto; l’header ha due bit, uno di stato (S) e uno di prenotazione (P); il primo indica se il payload contiene dati e il secondo se un nodo ha già prenotato quel pacchetto.
## NODI
Ogni nodo della rete DQDB ha una struttura ([Figura 4.27](Figura%204.27.png)) particolare:
- Contatore add/drop
	Ogni volta che nel bus di prenotazione il nodo osserva un pacchetto prenotato incrementa il valore; ogni volta che nel bus di trasmissione il nodo osserva un pacchetto che non contiene dati decrementa il valore.
- Contatore drop
	Viene decrementato ogni volta che nel bus di trasmissione passa un pacchetto che non contiene dati.
- Buffer
	È la memoria dove vengono memorizzati i dati in attesa di essere trasmessi.
## ACCESSO
Quando un nodo vuole accedere alla rete per trasmettere dati deve seguire una procedura precisa: osserva il campo P di ogni pacchetto che trastita nel bus di prenotazione e quando ne trova uno settato a 0 esegue la sua prenotazione; in questo momento il valore del contatore add/drop viene trasferito al contatore drop, che in questo momento rappresenta il numero di nodi che si sono prenotati prima di quello in esame; questo contatore viene decrementato ogni volta che il nodo osserva nel bus di trasmissione un pacchetto con il campo S settato a 0; quando il contatore drop assume valore 0 il nodo può usare il prossimo pacchetto con campo S settato a 0 per effettuare la trasmissione.
Questo modello non da la stessa priorità a tutti i nodi della rete: i più vicini agli estremi dei due bus, potendo prenotarsi prima degli altri, possono monopolizzare la rete. Per risolvere questo problema si è pensato di limitare il numero di slot prenotabili per ogni singolo nodo.