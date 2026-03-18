Quando parliamo di Wi-Fi si deve distinguere tra due enti:
- IEEE 802.11
	È l'ente che si occupa dello standard. Definisce le versioni (che sono taggate con l'anno in cui è stata rilascaita), che contengono tutti gli aggiormaneti dall'ultima versione, e gli emandement, cioè piccole modifiche relative all'ultima versione.
- Wifi Alliance
	È l'ente che si occupa del testing: un nuovo device riceve il bollino della Wi-Fi Alliance (che è quello che conosciamo e che vediamo) se soddisfa alcuni requisiti selezionati dello standard.
##### Popolarità
Il Wi-Fi è così popolare perché permette una certa mobilità, un buon data rate e un hw poco costoso.
Il problema del Wi-Fi è l'utilizzo di bande non licenziate ISM (Industrial, Scientific e Medical) (868MHz, 2,4GHz e 5GHz). Queste bande sono libere nel senso che tutti le possono usare senza dover pagare; comunque è stabilito che si deve avere un limite di potenza un minimo tempo di duty cycle. Questo non rende l'utilizzo del Wi-Fi adatto in constenti safety-critical.
# MAC
Nelle connessioni wired il MAC deve decidere chi trasmette e risolvere le collisioni che si verificano. Nel caso cablato gli attacchi più comuni sono tagliare il cavo e mettere insieme molti cavi in modo che questi si disturbino.
Nel mondo wireless il MAC deve:
- Decidere chi trasmette e risolvere le collisioni che si verificano.
- Fare access controll e fare encryption, cioè stabile se un determinato host può accedere alla rete. Anche se la rete è libera non vuol dire che un device debba accettare i pacchetti provenienti da ogni host; può fare filtraggio sulla base di una firma digitale (non è per la confidenzialità di dati, ma per l'identificazione del mittente).
# MODALITÁ
Ci sono due modalità di connessione:
- Infrastutturata
	C'è un Access Point (AP) che coordina la rete. Due dispositivi, detti station, comunicano solo passando dall'AP.
- Peer 2 peer
	I dispositivi possono comunicare direttamente senza passare dall'AP.
	È tipo delle reti ad-hoc, dove i dispositivi sono auto organizzati in una rete detta mesh. Non c’è un AP che coordina la rete, ma questo compito è svolto da degli owner; il cambio di questi (cioè che si possono disconnettere) è anche il motivo per cui implementare la security in una rete mesh è un probema enorme.
	È una soluzione molto studiata e usata in ambito accademico, ma anche molto poco in ambito industriale (la voglio tutti ma nessuno la prede). Il motivo è che non ci sono schede di rete che permettono la modalità ad-hoc.
# SEGNALE
Quello che ci interessa è la potenza di segnale con cui riceviamo il messaggio. Infatti questa potenza è direttamente proporzionale alla capacità di decifrare il messaggio stesso.
La formula è $P_{RX}=P_{TX}+G_{RX}+G_{TX}-L_{RX}-L_{TX}-L_{FS}-L_{M}$, dove:
- $P_{RX}$ è la potenza dl ricevente.
- $P_{TX}$ è la potenza del trasmittente.
- $G_{RX}$ e $G_{TX}$ sono il guadagno dell'antenna del ricevente e della trasmittente. È una quantità negativa, per questo si somma; le antenne perfette hanno questo valore pari a 0.
- $L_{RX}$ e $L_{TX}$ sono la perdita dell'antenna del ricevente e della trasmittente a causa del materiale usato.
- $L_{FS}$ è la perdita relativa alla distanza tra i due e all'ambiente circostante (spazio aperto vs cantina).
- $L_{M}$ è un'altra perdita.
##### Problema
Il problema collegato a questa formula è che è reversibile:
- Cambiare solo (non entrambi) $P_{RX}$ o $P_{RX}$ vuol dire che l'AP mi sente meglio, ma l'AP non sente meglio me; questo non migliora quindi la QoS.
- Cambiare invece uno degli altri parametri si riflette sia sul ricevente che sul trasmittente.
##### Attacco
Sicome siamo in un contesto wireless allora se un attaccante dispone di un'antenna buona (e grande) allora può ricevere i pacchetti di una Wi-Fi lontana, sembrano inoltre molto vicino all'AP. 
# JAMMING
È l'attacco per cui si inietta rumore di fondo (aumenta il rapporto noise/signal) nel canale al fine di flippare dei bit; in pratica stiamo aumentando $L_M$ della formula sopra. Osserviamo che basta cambiare la polirà di alcuni bit e non di tutti; il massimo che possiamo raggiungere è il 50% (se osse il 100% allora il ricevente riceverebbe l'informazione contraria e gli basterebbe rigirarla).
Il jamming è un attacco sempre possibile ed è molto difficile da contrastare. Le uniche cose che possiamo fare sono: trovare l'attaccante (che si sta palesando) e impedirgli di portare l'attacco; cambiare in continuazione canale, come succede nel caso di Bluetooth; usare una banda molto più larga del necessario in modo che si usi la porzione che l'attaccante non disturba.
# PACCHETTI
Ci sono tre tipi di pacchetti:
- Gestione
	Sono i pacchetti di autenticazione, deautenticazione e di beacon. Esistono a prescidere dal fatto che ci siano i pacchetti dati.
- Controllo
	Sono associati ai pacchetti dati, cioè non esistono senza di loro. Sono i pacchetti di RTS, CTS e ACK.
- Dati
	È il payload e contiene le informazioni vere e proprie. È l'unica parte che gode di crittografia.
### Cifratura
C'è un motivo per cui non si cifra tutto:
- Non si può: per cifrare i pacchetti di management dovrei avere una chiave condivisa con l’AP per cifrare e decifrare, ma questa chiave non ce l’ho perché ancora non sono collegato all’AP. In poche parole non si cifrare le cose della rete se ancora non se ne fa parte.
- Non si vuole: per compatibilità posso avere versioni di AP o device diversi.
### Terminale nascosto
Vediamo i pacchetti CTS e RTS cosa sono e a cosa servono.
Il problema del terminale nascosto è quando l’AP sente due device ma i due device non si sentono l’uno con l’altro; non sentendosi può succedere che comunichino contemporaneamente con l'AP e quindi che si verifichino collisioni.
Un pacchetto RTS é un pacchetto con cui si  comunica l'intenzione di parlare; con questo è meno probabile che si verifichino collisioni rispetto a usare un pacchetto dati (comunicazione classica con l'AP) perché è molto piccolo. Il CTS è un pacchetto inviato dall'AP che autorizza il destinatario a inviare pacchetti dati; chi lo riceve ma non è il destinatario non dovrebbe (in teoria) comunicare.
##### Attacchi
I successivi attacchi si evitano non usando i pacchetti CTS e RTS; per fortuna oggi giorno non si usano più grazie alle velocità raggiunte dal Wi-Fi.
- L'attaccante si finge l'AP, e invia a se stesso un CTS; gli altri dispositivi non parleranno perché non sono i destinatari del CTS. Se questo viene fatto sistematicamente allora nessuno parla mai.
- L'attaccante ascolta i CTS; appena ne percepisce uno che è destinato ad un host invia dati all'AP in modo che si verifichi una collisione.
# WEP
Quando un device vuole entrare in una rete protetta con WEP può attraversare [tre stati](stati%20WEP.png). Lo stato intermedio è stato aggiunto perché inizialmente i router non permettevano l'associazione di molto dispositivi contemporaneamente; siccome la fase di autenticazione è più lunga della fase di associazione si mettevano i dispositivi che non stavano usando la rete nella stato intermedio, in modo che quando volevano tornare online potesse farlo a basso costo.
### Autenticazione
Il WEP fa un'autenticazione di gruppo: l'AP manda in chiaro una challenge; la station la rimanda criptata; l'AP autentifica la station se la chiave di cifratura è corretta.
Un attaccante che ascolta il canale riceve in questo modo sia il messaggio cifrato che quello in chiaro; se ne riceve abbastanza i può fare cryptoanalisi per ottenere la chiave di cifratura.
Per ricevere più velocemente pacchetti di questo tipo un attaccante può cacciare dalla rete un utente per fargli rifare l'accesso.
### Stream cypher
Ci sono cifrature diverse:
- Stream-based
	Funziona bene su una sequenza lunga di byte (teoricamente infinita), come l’OTP. Per avere un corretta cifrature devo avere uno stream (cioè sapere dove si trovano i pacchetti nella sequenza); posso farlo sul TCP, ma non sull’UDP perché non ho uno stream ma ho dei pacchetti isolati.  
- Block-based
	Funziona bene su un blocco di byte. Funziona bene quindi su UDP perché ogni pacchetto è un blocco.
### Crittografia
Il processo di codifica nel protocollo WEP è il seguente:
- Dato un messaggio m si calcola un checksum di 32 bit e lo si concatena al mes- saggio stesso, producendo m′; questo passaggio serve solamente per poter indi- viduare errori casuali di trasmissione, e non per la sicurezza della trasmissione stessa.
- Il messaggio m′ viene passato al codificatore RC4 che lo mette in XOR bit a bit con il keystream, producendo m′′. Il keystream è prodotto da una funzione determin- istica i cui parametri sono la chiave simmetrica e un vettore di inizializzazione di 24 bit, l’Initialization Vector (IV).
- Infine, m′′ viene concatenato con l’IV (mandato in chiaro) e spedito al desti- natario.