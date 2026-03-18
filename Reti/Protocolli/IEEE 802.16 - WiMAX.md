Lo standard IEEE 802.16, conosciuto anche come WiMAX (Worldwide Interoperability for Microwave Access), è nato per portare la connessione dati in aree rurali, dove le compagnie incaricate della distribuzione del [WiFi](IEEE%20802.11%20-%20WiFi.md) non avevano ritorni economici sufficienti per implementare il loro servizio. Per abbattere i costi, non potendosi installare le classiche antenne, si è pensato di usare il segnale televisivo già presente nelle abitazioni e di intallare delle antenne sui tetti o in ambienti facilmenter raggiungibili.
Prima di questo standard la potenza del segnale veniva regolata in base alla distanza con il disposavo da connettere, in modo una qualità del servizio constante in tutto il raggio di copertura; con questo protocollo invece più vicini siamo alla BS (Base Station) più la velocità di connessione è maggiore.
Un caso d'uso di questo standard è negli aereoporti.
## ACCESSO
Il WiMAX prevede una BS (Base Station) a cui i propri utenti (Subscriber Station) possono collegarsi. Ci sono due modalità di utilizzo della rete:
- TDD (Time Division Duplex)
	Le fasi di trasmissione dalla BS agli utenti, detta downlink, e dagli utenti alla BS, detta uplink, sono sembrate su base temporale: ogni intervallo di tempo è suddiviso in due sotto intervalli, separati da un intervallo di guardia utile per eseguire la commutazione del servizio; nel primo, destinato al downlink, la BS comunica in modalità broadcast; nel secondo, destinato all’uplink, gli utenti comunicano su risorse dedicate con la BS.
- FDD (Frequency Division Duplexing)
	Vengono assegnate bande diverse per i flussi di downlink e di uplink, che a questo punto possono essere eseguite in contemporanea.
## SERVIZI
I servizi di questo protocollo sono offerti in modalità [connection oriented](Architettura%20a%20livelli.md#SERVIZIO) e sono basati sulla [commutazione di circuito](Trasmissione.md#COMMUTAZIONE%20DI%20CIRCUITO%20(CC)).
Ci sono quattro tipi di servizio diversi:
- Bit-rate costante
	È rivolto ai servizi voce. Vengono riservate delle risorse dedicate, cioè in uso esclusivo, per il tutto il tempo in cui il servizio è richiesto.
- Bit-rate variabile in tempo reale
	È rivolto al traffico multimediale e alle applicazioni che lavorano in real time. La BS interroga con cadenza regolare gli utenti e adegua il bit-rate in base alle necessità di quest'ultimi.
- Bit-rate variabile non in tempo reale
	È rivolto allo streaming di dati che non richiedono il real time. La BS interroga con cadenza non regolare gli utenti che stanno usando questo servizio, con l’idea di adattare il bit-rate in base alle necessità. Se un utente non risponde viene rimosso dalla lista di utilizzatori e non sarà interrogato in futuro.
- Best-effort
	È rivolto al traffico rimanete. La BS non interroga nessun utente, ma questi si contendo le risorse disponibili in base al servizio che richiedono.