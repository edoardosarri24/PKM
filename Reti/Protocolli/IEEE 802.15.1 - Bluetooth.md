Bluetotth è stato sviluppato nel 1998 dalla Bluetooth Specila Interest Group (Bluetooth SIG) (apple, microsoft, intel, ect) ed è deinito nello standard IEEE 802.15.1. Il protocollo Bluetooth classico (BR/EDR) è stato definito per scambiare dati in modo wireless tramite trasmissione radio; il seccessore è il [Bluetooth Low Energy (BLE)](Bluetooth%20Low%20Energy%20(BLE).md).
# TOPOLOGIA
I dispositivi formano una WPAN (Wireless Personal Area network), detta Piconet, con una topologia a stella.
- È una rete "ad hoc": i dispositivi si organizzano tra loro creando una rete che non comprende collegamenti verso l’esterno.
- Ha una struttura gerarchica, dove il nodo master (solitamente è un device come cellulare o computer) gestisce la rete e gli altri nodi, detti slave, sono collegati a esso.
- A una Piconet possono essere connessi più nodi, ma solo 7 (più il master) possono essere attivi contemporaneamente.
- Più Piconet possono essere connesse tra loro, creando una Scatternet; in questo caso qualche nodo avrà una doppia funzione master/slave.
## ARCHITETTURA
### Livello radio
Si usa una bassa potenza e quindi il raggio di copertura è limitato a circa 10 metri.
Per ridurre la quantità di interferenze si usa la tecnica FHSS (Frequency Hopping Spread Spectrum): ogni secondo i dispositivi connessi tra loro cambiano canale 1600 volte secondo una modalità pseudocasuale; in questo modo le collisioni sono molto rare.
### Livello baseband
- Ha le stesse funzionalità del livello [MAC](IEEE%20802%20-%20LAN.md#MAC%20(Medium%20Access%20Control)) negli standard IEEE 802, cioè è il responsabile dell’accesso al mezzo condiviso. La gestione dell’accesso al mezzo fisico è basata sulla tecnica [polling](Tecniche%20di%20accesso%20MAC.md#POLLING), opportunamente gestita dal master.
- L'utilizzo della rete avviene con la tecnica TDD (Time Division Duplex) in modalità half-duplex: un dispositivo non può inviare e ricevere dati contemporaneamente; il master invia dati allo slave negli intervalli di tempo pari e lo slave risponde in quello successivo.
- Si prevedono due tipi di canali tra il master e gli slave:
	- SCO (Synchronous Connection Oriented)
		È adatto a collegamenti [connection oriented](Architettura%20a%20livelli.md#SERVIZIO) e viene instaurato prenotando gli intervalli in cui si vuole accedere. Si cerca di minimizzare il ritardo nella trasmissione a scapito degli errori che si possono introdurre.
	- ACL (Asynchronous Connectionless Link)
		Si cerca di salvaguardare la correttezza dei dati più tosto che la velocità con cui arrivano. Se il canale è inizializzato con questa tecnica viene sospesa quella classica FHSS.
## STATO DEI DISPOSITIVI
Un dispositivo bluetooth si può trovare nello stato connessione o stand-by: nel primo caso è connesso ad un master e sta scambiando dati; nel secondo caso o non è connesso a nessuna Piconet oppure è connesso ma non sta scambiando dati. Quando uno slave passa dallo stato di stand-by a quello di connessione il dispositivo si può trovare in uno dei seguenti sottostati:
- Active mode
	Sta effettivamente scambiando dati all’interno della Piconet.
- Hold mode
	Non interagisce con la Piconet per un periodo di tempo prefissato. È usato per far risparmiare energia.
- Sniff mode
	Ascolta il master ad intervalli di tempo prefissati. È usato per far risparmiare energia.  
- Park mode
	Appartiene sempre alla Piconet, ma non è attivo. Non può comunicare per sua volontà con il master, ma si limita ad ascoltare la presenza di messaggi broadcast provenienti da esso che indicano la presenza di un posto libero all’interno della Piconet e quindi la possibilità di connettersi.