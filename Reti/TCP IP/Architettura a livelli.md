Una rete solitamente definisce un insieme di protocolli, cioè delle regole che i dispositivi devono rispettare per essere in grado di comunicare. Questi protocolli sono solitamente molto numerosi e per questo sono organizzati in insiemi in base alla loro relazione; gli insiemi formano i vari livelli di un architettura protocollare ([Figura 4.2](Figura%204.2.png)). Alcuni esempi sono il modello [ISO/OSI](ISO%20OSI.md) e quello [TCP/IP](TCP%20IP.md).
- Peer-to-Peer
	Ogni layer del dispositivo mittente comunica (Indirettamente) solo con il rispettivo layer del dispositivo ricevente.
- Vantaggi
	- Flessibilità
		Si può modificare e aggiornare uno strato senza compromettere gli altri.
	- Information hiding
		Si nasconde l’implementazione di un layer ai suoi adiacenti.
## SERVIZIO
Un servizio è un insieme di primitive (Operazioni base) che un livello offre ai suoi adiacenti tramite un'interfaccia. Si può definire come:
- Connection oriented
	È necessario che i dati arrivino con lo stesso ordine di invio.
- Connectionless
	Non è necessario che i dati arrivino con lo stesso ordine di invio.
- Affidabile
	Si richiede una notifica di corretta ricezione del messaggio inviato. Potrebbe introdurre dei ritardi.
- Non affidabile
	Non si richiede una notifica di corretta ricezione del messaggio; il mittente non ha la certezza che la comunicazione sia andata a buon fine.
## INCAPSULAMENTO SUCCESSIVO
La trasmissione dell'informazione tra i vari livelli ([Figura4.6](Figura4.6.png)) avviene tramite l’incapsulamento successivo.
- Invio
	Il messaggio inviato al livello sottostante, detto packet data unite (PDU) è composto da due parti: una dati (D) e una di controllo, detta header (H); la parte di header contiene le informazioni utili al pari livello di un altro nodo per interpretare i dati. Il livello sottostante prende il messaggio (Sia la parte H che la parte D), lo inserisce nella parte di dati, imposta un nuovo header e trasmette il tutto al livello sottostante. Questa tecnica prosegue fino al livello Data Link, che introduce due parti di controllo (H e T).
- Ricezione
	In ricezione ogni livello usa l’header per interpretare i dati e trasmette il tutto al livello superiore, provvedendo prima ad eliminare la parte di header che ha già utilizzato e quindi non necessaria ai livelli sovrastanti.
### Concetti
L'incapsulamento successivo si basa sui concetti di:
- Separation of concern
	Non si devono duplicare le funzionalità all'interno dei vari livelli. L'unica eccezione può essere quella del rilevamente degli errori.
- Information hiding
	Ogni livello non deve essere black box con gli adiacenti, cioè non deve sapere cosa riceve, ma solo elaborare la sua parte di informazione.