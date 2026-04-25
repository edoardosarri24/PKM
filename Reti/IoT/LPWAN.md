Sono le Low Power Wide Area Networks (LPWAN): vogliono soddisfare le esigenze di copertura molto ampia, basso consumo energetico e bit rate non necessariamente alto. Sono nate diverse soluzioni:
- Proprietarie
	Lo standard è brevettato e non si può leggere; i dispositivi possono parlare solo tra loro, cioè tra quelli che implementano quello standard. Solitamente chi utilizza standard proprietari utilizza bande senza licenza.
- Standard
	Sono soluzioni libere, cioè l'implementazione si può vedere e leggere. Sono le soluzioni del 3GPP e utilizzano bande su licenza.
### Vantaggi
Nonostante i seguenti vantaggi questa tecnologia ha anche degli aspetti negativi: un basso bit rate, un payload piccolo e un alto ritardo (a causa dello schema di modulazione, anche nell'ordine dei secondi).
##### Long range
Grazie all'utilizzo di bande Sub-Giga Herts, si ha che:
- Il raggio di copertura è nell'ordine del chilometro.
- Sono molto buone le trasmissioni in luoghi difficili da raggiungere (come cantine) perché penetra bene i materiali.
##### Low power
Il basso consumo è reso possibile grazie a:
- La topologia a stella. Le connessioni vanno infatti dalla base station al device.
- L'utilizzo del duty cycle, che permette di non utilizzare la trasmittente quando il dispositivo non deve comunicare.
- Utilizzo di tecniche ad accesso casuale (come aloha) a livello MAC.
##### Scalabilità
Per parallelizzare le comunicazioni si utilizzano sia sistemi multi antenna che multi canale.
Si permette di scegliere dei parametri per configurare il canale di comunicazione, in modo da poter regolare l'utilizzo delle risorse in base alle esigenze.
# 3GPP RELEASE
Sono standard rilasciati prima dello standard [5G](5G.md).
### NB-IoT (Neural Band IoT)
##### Canale
In questo standard la banda può essere di più tipi:
- Standalone
	Si usa un canale di 200kHz è all'interno di una banda non più usata di GSM (standard telefonico vecchio), cioè di una banda con licenza (si dovrà pagare un operatore per usarla).
- Guard-band
	Si usa una porzione di spettro da 180kHz al bordo della banda usata da LTE.
- In-band
	Si usa una porzione di spettro da 180kHz all'interno della banda usata da LTE.
##### Proprietà
- Si ha una copertura radio molto elevata.
	Si può ottenere tramite:
	- Un picchio di potenza all'interno di un canale utilizzato.
	- Utilizzo di schemi di modulazione efficienti.
	- Molte ripetizioni dei dati.
- Durata della batteria superiore a 10 anni.
	Si può ottenere grazie alle tre modalità che un device può avere:
	- Connected
		Può inviare e ricevere messaggi.
	- Idle
		Monitora il canale e gli può essere segnalato dalla base station di essere posto in connected mode.
	- Power saving
		Non è raggiungibile da nessuno; la rete deve attendere che lo sleep time (predeterminato) finisca.
# LORA E LORAWAN
Definiscono una soluzione che è un [pacchetto completo](LoRa%20e%20LoRaWAN.png): chi ci aderisce può costruirci on top un application layer specifico, ma il resto è già definito.
LoRa è lo standard che definisce il livello fisico; è proprietario, sviluppato da Semtech. LoRaWAN è lo standard che definisce il MAC layer e la restante architettura di rete; è open e definito dalla LoRa Alliance.
### Lora
È una tecnologia di trasmissione wireless proprietaria di Semtech. Utilizza le bande libere ISM (in Europa 868MHz), per cui non è necessario pagare la licenza.
##### Chirp Spread Spectrum (CSS)
È la tecnica di modulazione utilizzata.
La frequenza aumenta o diminuisce col passare del tempo; per trasmettere il segnale si utilizza tutta la larghezza di banda. In pratica si parte da una frequenza $f_0$; questa cresce linearmente finché non si raggiunge $f_{max}$ che delimita la banda; si riparte da $f_{min}$ finché non si torna a $f_0$.
All'interno della banda ci sono $2^{SF}$ (Spreading Factor) livelli di frequenza; ogni livello corrisponde a un codifica dell'informazione (sequenza di bit).
Tenere costante la larghezza di banda e aumentare SF vuol dire diminuire il bit rate ma aumentare la potenza di ricezione e quindi la distanza.
### LoraWAN
Abbiamo detto che LoRa gestisce la modulazione del segnale, cioè come il segnale è trasmesso (il livello fisico).
Per i livelli superiori una soluzione è applicare il protocollo LoRaWAN alle trasmissioni LoRa; LoRaWAN deciderà il livello MAC, cioè come le base station possono coordinarsi, come i dati sono codificati e come sono gestiti gli indirizzi.
##### [Architettura](LoRaWAN%20architecture.png)
- Dispositivi
	- Più dispositivi possono comunicare contemporaneamente e lo fanno in modalità broadcasting. La tecnica di accesso al canale è un'Aloha.
	- Un dispositivo non è associato a uno specifico gateway, ma quanto trasmesso può essere ricevuto da più gateway.
	- La gestione della ritrasmissione è a carico del dispositivo.
- Gateway
	- Ricevono più segnali da più dispositivi tramite un canale LoRa. Passano le informzioni ricevute ai server tramite una connessione non definita (si segue le regole di TCP/IP, ad esempio ethernet).
	- Possono supportare le tecnologie multicanale in ricezione.
	- Sono trasparenti per i device; essi infatti permettono solo di aggregare il traffico e avere un'affidabilità maggiore.
- Network server
	È la parte backend che gestisce i task complessi. Ad esempio deve rilevare pacchetti duplicati, fare filtraggio, decriptare i messaggi e controllare il data rate.
- Applicaztion server
	È l'applicazine IoT, cioè quell che implementa le caratteristiche funzionali dell sistema.
##### Classi di dispositivi
- All (A)
	Sono sensori alimentati a batteria.
	La comunicazione è iniziata da loro (uplink); dopo ogni trasmissione ci sono due piccoli periodi di ricezione preceduti da un piccolo periodo di attesa; al termine di questi due periodi di ricezione il dispositivo va in sleap per risparmiare batteria.
- Beacon (B)
	Sono attuatori alimentati a batteria.
	La comunicazione è iniziata da loro (uplink); dopo ogni trasmissione ci sono due piccoli periodi di ricezione preceduti da un piccolo periodo di attesa; al termine di questi due periodi di ricezione c'è una terza finistra di ricezione; successivamente il dispositivo va in sleap per risparmiare batteria.
	Durante questa comunicazione il dispositivo riceve il beacon frame, che permette di sincronizzarsi con server i successivi tempi di comunicazione.
- Continuos (C)
	Sono attuatori alimentati da una presa.
	La ricezione è sempre attiva (tranne quando trasmettono).
	La trasmissine può essere fatta sia in unicast che in multicast.
##### Security
Prima che un dispositivo inizi a comunicare esso deve essere attivato. La richiesta di attivazione avviene tramite OTAA: il dispositivo comunica al server una $Join\ Request$ con il proprio identificativo univoco; il server invia una $Join\ Activation$ che contiene:
- Device address univoco nella rete di 32 bit. È fornito dai due server.
- Una chiave di 128 bit condivisa dal device e dal network server. Garantisce la riservatezza nella rete.
- Una chiave di 128 bit condivisa dal device e dall'application server. È usata per cifrare il payload e garantisce la riservatezza del messaggio.