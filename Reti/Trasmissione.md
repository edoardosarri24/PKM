La trasmissione dell'informazione segue un percorso preciso: sorgente, trasmittente, canale, ricevente e destinazione. La trasmittente trasforma il segnale da generico ad elettrico (Rappresentato da bit) in modo che il segnale possa attraversare il canale fisico (Fibra ottica, rame, aria); la ricevente esegue il passaggio inverso.
## COMMUTAZIONE DI CIRCUITO (CC)
- Caratteristiche
	- Sono presenti le fasi di set-up e teardown e il percorso definito è eslusivo degli utenti connessi.
	- La linea viene usata su base E2E, senza la presenza di nodi intermedi.
	- L'informazione non può essere frammentata.
- Vantaggio
	Una volta stabilita la connessione, finché questa è attiva, le due parti possono comunicare in qualunque momento senza ritardi.
- Svantaggio
	- Elevata intermittenza temporale: nella maggior parte dei casi la connessione non viene utilizzata in modo proporzionale al tempo in cui essa è attiva.
	- Non si può elaborare il segnale per permettere di cambiare tipo di rete.
	- L’integrità del flusso informativo viene controllata solo a destinazione; se c’è anche un solo bit errato si deve iniziare da capo l'intero trasferimento.
	- In caso di guasti si deve ripetere la fase di set-up.
- Uso
	Se il tempo totale di collegamento è molto maggiore della somma dei tempi di set-up e teardown. Ci deve essere un elevato scambio di informazioni tra gli utenti collegati.
## COMMUTAZIONE DI MESSAGGIO (CM)
- Caratteristiche
	- Non sono presenti le fasi di set-up e teardown.
	- La linea viene usata su base L2L.
	- L'informazione non può essere frammentata.
- Vantaggi
	- Non presenza del tempo di set-up e di teardown: tempo di trasferimento e quello di connessione sono molto simili e l’utilizzo della rete è ottimale.
	- Possibilità di attraversare mezzi fisici diversi: i nodi infatti possono elaborare il segnale e propagarlo su reti di tipo diverso.
	- Alta robustezza per via della possiiblità di decidere il percorso migliore al momento.
- Svantaggio
	L’integrità del flusso informativo viene controllata solo a destinazione; se c’è anche un solo bit errato si deve iniziare da capo tutto il trasferimento.
## COMMUTAZIONE DI PACCHETTO (CP)
- Caratteristiche
	- I percorsi non sono mai eslusivi degli utenti connessi.
	- La linea viene usata su base L2L.
	- Il messaggio viene frammentato in unità indivisibili non troppo grandi dette pacchetti.
- Vantaggi
	- Possibilità di attraversare mezzi fisici diversi: i nodi infatti possono elaborare il segnale e propagarlo su reti di tipo diverso.
	- La frammentazione permette di ridurre il tempo di impiego di un link.
	- La frammentazione permette di rinviare solo il pacchetto corrotto e non l’intero messaggio in caso di errore all’arrivo.
### Circuito virtuale
- Caratteristiche
	- Sono presenti le fasi di set-up e teardown.
	- Connection oriented.
	- L'informazione non può essere frammentata.
- Svantaggio
	- In caso di guasti si deve ripetere la fase di set-up.
### Datagramma
- Caratteristiche
	- Non sono presenti le fasi di set-up e teardown.
	- Connection less.
	- L'informazione non può essere frammentata.
- Vantaggi
	- Non presenza del tempo di set-up e di teardown: tempo di trasferimento e quello di connessione sono molto simili e l’utilizzo della rete è ottimale.
	- Alta robustezza per via della possiblità di decidere il percorso migliore al momento.