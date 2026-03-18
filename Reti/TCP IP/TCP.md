È un protocollo per l'implementazione del livello [trasporto](TCP%20IP.md#TRANSPORT%20-%20[UDP](UDP.md)%20-%20[TCP](TCP.md)) della pila protocollare [TCP/IP](TCP%20IP.md).
Il protocollo TCP (Trasmission Control Protocol) ha lo scopo di realizzare collegamenti [affidabili](Architettura%20a%20livelli.md#SERVIZIO) tra processi remoti secondo il servizio [connection oriented](Architettura%20a%20livelli.md#SERVIZIO). Deve quindi presentare al livello application i datagram secondo l’ordine di generazione: il mittente deve usare due buffer, uno per i datagram da inviare e uno per quelli ancora non ricevuti e che potrebbero dover essere inviati nuovamente; il ricevente usa un buffer per riodinare i datagram prima di presentarli al livello supeiorie.
È un protocollo che permette di avere un collegamento [full duplex](Reti%20di%20telecomunicazioni.md#Full%20duplex).
## HEADER
L’header TCP ([Figura 4.18](Figura%204.18.png)) non ha dimensione ma fissa, ma essa varia da 20 a 60 byte. I campi sono:
- Porta sorgente/destinazione
	Indica il numero di porta usato dal processo mittente/ricevente, cioè i processi che si devono interfacciare.
- Numero di sequenza
	Il mittente numera i datagram generati in modo che il destinatario possa poi riordinarli. Le numerazioni tra flussi di dati diversi sono indipendenti.
- Numero di riscontro
	È preso in considerazione solo se il bit ACK è posto a 1 e ha significato solo per chi sta inviando il flusso informativo: il ricevente pone questo campo pari al numero del datagram ricevuto aumentato di uno; in questo modo il mittente conosce quale datagram il ricevente sta aspettando. Questa tecnica è nota come piggyback.
- Lunghezza header
	Indica il numero di byte di cui è composto l’header.
- Riservati
	Sono 6 bit riservati ad usi futuri.
- Flag
	Ogni bit ha un significato specifico:
	- URG
		Indica se il datagram è urgente e quindi se deve essere inoltrato immediatamente ai livelli superiori, anche senza rispettare il suo ordine.
	- ACK
		È il bit di riscontro, detto bit di Acknowledgement.
	- PSH
		Indica la richiesta di un invio di dati immediato.
	- RST
		Indica la richiesta di una chiusura del collegamento per un motivo inaspettato.
	- SYN
		Indica la richiesta di avvio di un collegamento.
	- FIN
		Indica la chiusura del collegamento in corso.
- Finestra
	Aiuta il protocollo a gestire operazioni di riordino e controllo della [congestione](Congestione%20reti.md): è il numero di byte che il ricevente può accettare senza che ci sia congestione.
- Checksum
	È usato per controllare la correttezza dell'header; a differenza dello stesso campo dell'header [UDP](UDP.md#HEADER) è utile per implementare l’[affidabilità](Architettura%20a%20livelli.md#SERVIZIO) del servizio.
- Puntatore urgente
	È rilevante solo se il flag URG è posto a 1 e indica il numero di byte che sono urgenti.
- Opzioni
	Ha una lunghezza variabile. Sono delle funzionalità aggiuntive, ma solitamente sono poco usate.
## CONNESSIONE
Prima di spedire il flusso informativo si deve stabilire una connessione virtuale con il destinatario tramite una fase di handshake.
Siccome il protocollo TCP può inviare flussi di grandi dimensioni in questa fase si gestisce la frammentazione, cioè la suddivisione dell'intero flusso in MTU compatibili con il livello [internet](TCP%20IP.md#INTERNET%20(IP)%20-%20[IPv4](IPv4.md)%20-%20[IPv6](IPv6.md)), in modo da non doverla richiede al livello sottostante.
La connessione avviene usando una procedura di handshake a tre fasi (Three-way handshake):
- Il processo client che vuole instaurare una connessione manda un messaggio detto "SYN" al server con cui si vuole collegare; questo messaggio è un datagram con il bit SYN posto a 1 e un numero di sequenza casuale all’interno del range consentito.
- Il server risponde con un messaggio detto "SYN+ACK"; questo messaggio è un datagram con i bit SYN e ACK posti a 1, un numero di sequenza casuale e un numero di riscontro pari al numero di sequenza ricevuto aumentato di 1.
- Il client risponde un messaggio detto "ACK"; questo messaggio è un datagram con il bit ACK è posto a 1 e il numero di riscontro è pari al numero di sequenza ricevuto aumentato di 1. Questo datagram, per velocizzare l’invio del flusso informativo, può contenere dati.
## DISCONNESSIONE
Quando il client vuole chiudere la connessione può scegliere due strade:
- Three-way handshake:
	- Il client manda un messaggio detto "FIN" al server; questo messaggio è un datagram con il bit FIN posto a 1 che può contenere dati.
	- Il server manda un messaggio detto "FIN+ACK" al client; questo messaggio è un datagram con con i bit FIN e ACK posti a 1 e può contenere gli ultimi dati da inviare.
	- Il client risponde con un messaggio detto "ACK"; questo messaggio è un datagram con il bit ACK posto a 1 e non può contenere dati. Da questo momento la connessione è chiusa.
- Se ci si vuole assicurare che il client riceva tutti i dati che il server vuole inviare si usa la la tecnica di four-way handshake:
	- Il client manda un messaggio detto "FIN" al server; questo messaggio è un datagram con il bit FIN posto a 1 e può contenere dati.
	- Il server risponde con un datagram il cui bit ACK è posto ad 1. In questo modo il client si mette in modalità ricezione e aspetta che il server abbiamo inviato tutti i dati.
	- Quando il trasferimento è finito il server invia un datagram al client con i bit FIN e ACK posti a 1.
	- Il client risponde con un datagram il cui bit ACK posto a 1. Questo datagram non può contenere dati e da questo momento la connessione è chiusa.