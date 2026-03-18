È il primo protocollo nato per l'implementazione del livello [internet](TCP%20IP.md#INTERNET%20(IP)%20-%20[IPv4](IPv4.md)%20-%20[IPv6](IPv6.md)) della pila protocollare [TCP/IP](TCP%20IP.md).
## HEADER
L’header IPv4 ([Figura4.9](Figura4.9.png)) ha una dimensione minima di 20 byte; è una matrice composta da almeno 5 righe di 4 byte l'una. I campi sono:
- Version
	Indica la versione del protocollo IP usata.
- IHL (Intermediate header length)
	Indica il numero di righe di cui è composto l’header. Essendo composto da 4 bit si hanno al massimo $2^4-1=15$ righe.
- Type of service
	Indica il tipo di dati trasportato dal datagram. È importante perché tipi di dati diversi devono essere trattati in modo diverso.
- Total length
	Indica il numero di byte totali (Header e payload) di cui è composto il datagram.
- Identification
	Un insieme relativamente grande di dati potrebbe dover essere [frammentato](TCP%20IP.md#Frammentazione) in più frammenti; questo campo indica l’identificativo dell’insieme di dati a cui il frammento appartiene.
- Flags
	Ogni bit di questo campo ha un significato specifico:
	- D
		Indica se il datagram può essere [frammentato](TCP%20IP.md#Frammentazione). Se la rete non dovesse supportare la dimensione di un datagram non frammentabile allora si può semplicemente scartare in quanto il livello IP è non affidabile.
	- M
		Indica se il datagram è l’ultimo di una serie che compone un insieme di dati frammentato.
	- Il terzo bit non è utilizzato.
- Fragment offset
	Indica l’offset, espresso in quantità di 8 byte detti ottetti, a partire dal primo byte dell’insieme di dati che è stato [frammentato](TCP%20IP.md#Frammentazione) in cui il datagram deve essere inserito per riassemblare l'intero messaggio.
- Time to live
	È un contatore a decremento che indica il numero di hop (Passaggi dell'informazione da un router all’altro) che un datagram può eseguire prima che venga eliminato dalla rete. Serve per porre fine ad un eventuale loop di un datagram all’interno della rete (Cosa possibile perché essa è inaffidabile).
- Protocol
	Indica il tipo di protocollo usato nel livello [trasporto](TCP%20IP.md#TRANSPORT%20-%20[UDP](UDP.md)%20-%20[TCP](TCP.md)).
- Header checksum
	Serve al router per verificare che nell’header non ci siano errori. La presenza di questo campo ha un impatto sul tempo di trasferimento di un datagram, dovuto all’obbligo di essere riscritto ogni volta che le informazioni nell’header subiscono variazioni.
- Source/Destination address
	Sono gli indirizzi IP del mittente e del destinatario. La loro dimensione di 32 bit rappresenta il motivo per cui senza l'[IPv6](IPv6.md) si sarebbe raggiunta la saturazione della rete.
- Options
	Può avere vari usi: implementare servizi di sicurezza; capire quale è la migliore strada da percorrere per arrivare alla destinazione tramite la tecnica di instradamento dalla sorgente; capire la [congestione](Congestione%20reti.md) della rete attraverso il tempo che un datagram fittizio impiega per arrivare in un determinato punto.
- Padding
	Sono bit fittizi usati in due casi: se la riga non è composta da 4 byte; se il datagram non ha dimensioni multiple di 8 byte.
## FRAMMENTAZIONE
Il compito dell'IPv4 è offrire la possibilità di spedire informazioni attraverso tipi di reti diverse; questi tipi di rete permettono l’invio di MTU (Maximum Transmission Unit) diverse. Se un datagram non può essere spedito per intero perché é troppo grande allora si procede alla frammentazione.
Questa consiste nell’identificazione del valore binario dei campi identification, flags e fragment offset per ogni frammento.
La ricostruzione dell’intero insieme di dati, detta assemblaggio, è a carico all’utente finale, cioè avviene su base E2E.
## INTERPRETAZIONE INDIRIZZO
Ogni interfaccia di rete, cioè il confine tra utente e collegamento fisico, ha un indirizzo IP specifico; esso è formato dall’hostid e dal netid, che identificano un host all’interno di una rete. La rappresentazione usata è quella decimale puntata: ogni byte è separato da un punto e scritto in notazione decimale.
Nel tempo sono state usate tecniche diverse per interpretare l’indirizzo, con l'obiettivo di aumentare il numero host indirizzabili:
- Il primo è stato il classful networking. Ogni indirizzo IP apparteneva ad una determinata classe ([Figura](classful%20netwoking.png)), identificata dai primi bit; le classi si differenziano tra loro per il numero di bit destinati al netid e all’hostid. Il problema di queste soluzione era il grande numero di indirizzi sprecati: gli host per rete erano prefissati in base alla classe e se questi non servivano allora restavano inutilizzati.
- Si è pensato poi di suddividere gli host appartenenti ad un singolo dominio (Rete) in sottoinsiemi (Sottoreti); in questo modo si riesce ad avere più flessibilità sul numero di indirizzi allocabili e si separano le richieste di interconnessione locali con quelle dell'intera rete. Questo concetto si implementa dividendo l'hostid in due, il subnetid e l’hostid (Il netid rimane lo stesso), che aggregate vengono definite parti locali; in questo modo in siti (Insieme di strutture aggregate sotto la stessa entità) dove ci sono molte reti (Singola struttura) si associa un netid all'intero sito e ogni rete ottiene il proprio subnetid. Questa soluzione ha portato alla definizione di maschera di sottorete (O maschera di rete): è la notazione in cui i primi $y$ bit, che indicano la quantità di bit destinati al netid e al subnetid, sono posti a 1 e i restati $32-y$, che indicano la quantità di bit destinati all’hostid, sono posti a 0.
- Il successivo sistema, detto Classless Inter-Domain Routing (CIDR), permise di eliminare i vincoli delle dimensioni dei campi dedicati al netid e al subnetid+hostid. Per capire le dimensioni dei due campi, negli indirizzi IP di questo tipo si usa la sintassi a.b.c.d/y; il prefisso di rete, cioè y, indica il numero di bit destinati al netid; i restanti 32-y bit sono destinati al subnet+hostid. In questa soluzione i concetti per la maschera di sottorete rimangono gli stessi.
## FORWARDING
L’operazione di inoltro (Forwarding) viene gestita da un router sulla base di un database locale, che contine gli accoppiamenti tra submask e indirizzi IP per ogni porta di uscita del router. I concetti sono i seguenti:
- L’indirizzo IP di destinazione viene messo in AND logico con le maschere contenute nel database finché il risultato ottenuto non corrisponde all’indirizzo IP legato alla submask analizzata.
- Se non si trova corrispondenza nel database locale il datagram viene inviato ad router di default, che ricomincerà l’invio del pacchetto nella rete (Non all’infinito grazie al campo "time to live").
- È possibile, sopratutto per i router più lontani dagli host, che ci siano più corrispondenze; in questo caso si sceglie come posta di uscita quella relativa alla maschera più lunga, cioè con più 1 in serie.
## ALLOCAZIONE DEGLI INDIRIZZI
I modi in cui un indirizzo IP può essere allocato sono due.
### Statico
L’amministratore della rete assegna in modo definitivo un determinato indirizzo IP ad un dispositivo.
Stiamo legando il numero dei dispositivi indirizzabili al numero degli indirizzi IP disponibili; visto che nella maggior parte dei casi la connessione di un dispositivo alla rete non è continua nel tempo potrebbe non essere la scelta migliore.
Può avere comunque senso per i dispositivi fissi che sono collegati sempre allo stesso router.
### Dinamico
L’allocazione DHCP (Dynamic Host Configuration Protocol) serve per collegare alla rete un dispositivo in modo ubiquo, cioè in modo che non possa essere riconosciuto solo da quei router che conoscono il suo indirizzo (che dovrebbe essere sempre lo stesso); permette inoltre di non legare il numero dei dispositivi indirizzabili al numero degli indirizzi IP disponili. È un sistema client-server plug&play (L’utente non deve sapere come funziona) con il client che è embedded in ogni dispositivo.
Quando il dispositivo entra per la prima volta nella rete vengono eseguiti una serie di passaggi:
- Richiesta
	Il dispositivo invia una richiesta broadcast (Con indirizzo destinazione 0.0.0.0) a tutti i server DHCP raggiungibili entro il time to live. A questa richiesta viene legata un’etichetta (Transaction ID) univoca di 4 byte, in modo che il mittente possa essere identificato.
- Offerta
	Ogni server raggiunto invia la propria offerta al richiedente; questa è comporta da prezzo e credenziali IP, quali indirizzo, maschera e tempo di validità. Tale scambio avviene tramite invio broadcast perché al client non è stato ancora associato un indirizzo IP.
- Scelta
	Il client sceglie l’offerta di un server e lo notifica. Questo passaggio deve essere fatto senza ancora usare l’indirizzo IP proposto, in quanto potrebbe esserci qualche altro client che è stato più veloce e a cui sono già state assegnate le credenziali.
	Basandoci sull’idea che le offerte sono tutte diverse si può inviare un messaggio broadcast specificando quella accettata.
- Conferma
	Il server conferma al client tramite un messaggio ACK l’assegnazione delle credenziali IP per il tempo prestabilito.