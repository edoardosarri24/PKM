Il modello OSI (Open System Interconnection), definito da ISO (International Organizzation for Standardization), è un modello aperto che definisce lo standard teorico per le [architetture di rete a livelli](Architettura%20a%20livelli.md); effettivamente lo stadard che viene usato a livello mondiale è l'architettura [TCP/IP](TCP%20IP.md).
## LIVELLI
Ci sono 7 livelli totali. Le informazioni tra i livelli vengono scambiate tramite [l'incapsulamento successivo](Architettura%20a%20livelli.md#INCAPSULAMENTO%20SUCCESSIVO), tipico delle strutture a livelli.
##### Primitive
Nella teoria ci sono 4 [primitive](primitive%20ISO%20OSI.png) (nella pratica, come nelle socket TCP, queste primitive sono diverse per nome e numero). Non è necessario che ci siano tutte, ma l'ordine deve essere: request, indication, response e confirm.
### Host layer
I 4 più alti sono detti "livelli utente" o "livelli applicativi" e sono:
- Application
	Consente la cooperazione tra utenti diversi nell’ambiente ISO/OSI fornendo servizi che richiedono una rete per funzionare. È l’utente di tutti i livelli e offre un’interfaccia solo al livello sottostante.
- Presentation
	Consente di far cooperare gli utenti che stanno usando una rappresentazione dei dati in formati diversi tra loro.
- Session
	Consente a utenti diversi di realizzare una sessione per la comunicazione.
- Transport
	Rende compatibile la modalità [connection oriented](Architettura%20a%20livelli.md#SERVIZIO) alle reti (Livelli inferiori) che non la supportano.
### Media layer
I 3 più bassi sono detti “livelli di rete” e sono:
- Network
	Deve gestire il percorso tra gli utenti, cercando e scegliendo i collegamenti da usare. È responsabile del [Routing](Routing.md), cioè dell’interpretazione degli indirizzo logici.
- Data link
	Si divide in livello LLC (Link logical control) e MAC (Medium access control). Ha due compiti:
	- Garantire l’integrità della trasmissione dei dati, correggendo eventuali errori introdotti dalla trasmissione.
	- Gestire la condivisione del canale da parte di più utenti, in modo da ottimizzare il collegamento e risolvere l’intermittenza temporale.
- Physical
	Si occupa della trasmissione/ricezione dei bit nel/dal canale di comunicazione, che è il mezzo fisico con cui si trasmettono i dati.
## NODI DI TRANSITO
I livelli utente comunicano virtualmente solo con i pari livello della destinazione finale; si dice che operano su base E2E (End2End). I livelli di rete sono attivi in tutti i nodi del percorso, compresi i nodi di transito; si dice che operano su base L2L (Link2Link). I compiti svolti dai nodi intermedi ([Figura4.5](Figura4.5.png)) sono quindi solo quelli di:
- Network
	Esegue il [Routing](Routing.md) e individua percorsi alternativi a quello scelto in partenza nel caso ci siano guasti sulla rete.
- Data link
	Esegue controlli di affidabilità sui dati ricevuti in modo da non trasmettere un flusso di dati affetto da errori in modo irreparabile.
- Physical
	Svolge la funzione di ripetitore in modo da limitare il disturbo del canale di comunicazione.