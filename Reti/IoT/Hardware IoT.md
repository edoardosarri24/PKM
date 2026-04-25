- Comunicazione
	È la scheda di rete. Deve fornire una trasmittente radio o una connessione cablata.
- Microcontroller
	Un piccolo micro processore che elabora del codice e quindi un comportamento.
- Sensori e attuatori
	Per acquisire dati e interagire con il mondo esterno.
- Batteria
	Per fornire potenza ai circuiti elettrici.
- Memoria
	- ROM (Read Only Memory).
		Serve per memorizzare il codice che codifica il comportamento dell'oggetto.
		Solitamente non è modificabile ed è scritta al momento della fabbricazione (questo implica che l'oggetto è programmabile fino ad un certo punto); i microprocessori più moderni permettono di sovrascrivere la ROM tramite aggiornamenti sw.
	- RAM.
		Serve per memorizzare dati durante l'esecuzione del software.
- Timer
	Usato dal codice per qualunque tipo di compito richieda un timer.
# RFC7228
In generale l'hw di un dispositivo IoT è molto vincolato. Questo documento ufficiale lo divide in tre categorie sulla base dei vincoli.
Il vincolo più grande e che è presente in tutte le categorie è quello della batteria; sulla base di questo infatti si articolano anche gli altri.
Visti questi vincoli, e visto che comunque è necessario far interagire i dispositivi con internet, si è dovuta rivedere la pila protocollare di rete per permettere l'integrazione.
### Classe 0
- Non si interfacciano con l'esterno o comunque pochissimo. Se lo fanno è impossibile avere messaggi cifrati.
- Hanno grandi vincoli sulle capacità computazionali e di memoria.
- Sono gestiti semplicemente tramite segnali binari e indicazioni sullo stato.
- Esempi
	- TelosB: microcontroller da 10kB di RAM e 48kB di flash memory; chip radio; sensore di umidità di luce; attuatori led.
	- Zolertia Z1: microcontroller da 32kB di RAM e 512kB di flash memory; chip radio.
### Classe 1
- Non implementano l'intera pila protocollare di rete, ma utilizzano versioni specifiche come [CoAP](CoAP.md) tramite UDP. Si possono interfacciare con l'esterno e sulla rete, ma si riconosce che sono dispositivi particolari.
- Hanno delle limitazioni sulle capacità computazionali e di memoria.
### Classe 2
- Supportano tutti i protocolli di un pc o server. Si interfacciano con l'esterno e non si riconosce che sono dispositivi particolari.
- Sono dotati di buone capacità di memoria e computazionali.
- Esempi
	- Arduino: kit di microcontrollori per captare e interagire con il mondo; programmabile in C/C++; scheda (board) che può essere estesa; OS come Linux.
	- Raspeberry Pi: microcomputer; possibilità di inserire più OS; cache di livello 2; programmabile in C/C++.