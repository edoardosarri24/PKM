Il modello TPC/IP è un modello chiuso che definisce lo standard pratico per le [architetture di rete a livelli](Architettura%20a%20livelli.md); è quindi l'implementazione del modello [ISO/OSI](ISO%20OSI.md) che viene effettivamente usata.
Nasce per la richiesta di un modello funzionante e leggero: i calcolatori del tempo avevano pochissima potenza e ISO/OSI era molto pesante e ridondante. TCP/IP cerca di eliminare tutte le cose superflue: i livelli sessione e trasporto, e l'information hiding che portava a trasmettere molto header rispetto ai dati effettivi.
# STRUTTURA
![stack TCPIP](stack%20TCPIP.webp)
La pila protocollare TPC/IP sta sopra il data link layer (L2 dell'ISO/OSI), quello che in questo stack è l'[host to network](TCP%20IP.md#HOST%20TO%20NETWORK). Si assume che la rete farà il massimo per trasferire i pacchetti dalla sorgente alla destinazione; non si fa nessuna altra assunzione.
I partecipanti sono:
- Host (L7 di ISO/OSI)
	- Sono la sorgente e la destinazione dei dati.
	- Devono essere identificati in modo univoco con un IP.
- Router/Gateway (L2 di ISO/OSI)
	- Sono i nodi intermedi che indirizzano i pacchetti.
	- Hanno bisogno di un'interfaccia IP per ogni subnet a cui sono collegati.
# STACK PROTOCL
##### Application
Raggruppa tutti i livelli superiori al livello trasporto della pila protocollare [ISO/OSI](ISO%20OSI.md).
Rappresenta l'interfaccia tra l'utente e la rete, stabilendo e gestendo (aprire e chiudere) le sessioni (attività svolta tra due entità per trasferire dati in entrambi i sensi per tutta la durata del collegamento) di lavoro.
Esempi di implementazioni sono ftp, smtp e http.
##### Transport - [UDP](UDP.md) - [TCP](TCP.md)
Operando su base E2E ha il compito di rendere disponibili al livello application i servizi di tipo [connectionless](Architettura%20a%20livelli.md#SERVIZIO) e [connection oriented](Architettura%20a%20livelli.md#SERVIZIO), cosa non offerta nativamente dal livello internet.
Il livello internet non distingue la tipologia del dato che trasporta, ma si limita a consegnare le informazioni. Il livello di trasporto deve invece interpretare queste informazioni; questo concetto si traduce con il numero di porta, che identifica a livello astratto un processo SW nella macchina host.
Siccome TCP/IP elimina il livello sessione di ISO/OSI, alcune delle sue funzionalità sono state implementate in qusto livello.
##### Internet (IP) - [IPv4](IPv4.md) - [IPv6](IPv6.md)
Ha il compito di consentire lo scambio di informazioni tra nodi di rete ([router](Dispositivi%20di%20interconnesione.md#ROUTER)) che sono connessi tramite diversi tipi di rete (Canale di comunicazione), cioè dell'[instradamento](Routing.md) dei pacchetti.
Trasporta i dati in pacchetti che hanno una dimensione multiplo di 8 byte, chiamati datagram e composti da header e payload. Si usa la [commutazione di pacchetto a datagramma](Trasmissione.md#Datagramma): questo implica che il livello IP offra nativamente solo un servizio [inaffidabile](Architettura%20a%20livelli.md#SERVIZIO) e [connectionless](Architettura%20a%20livelli.md#SERVIZIO).
##### Host to network
Raggruppa tutti i livelli collegamento (data link) e fisico della pila protocollare [ISO/OSI](ISO%20OSI.md). A volte si dice che TCP/IP ha 5 livelli perchè questi due si separano.
Ha il compito di nascondere ai livelli sovrastanti le caratteristiche fisiche del mezzo fisico usato per traferire l’informazione. Rende quindi i protocolli TCP/IP compatibili con più tipi di rete.
# MIDDLEWARE
Il livello IP funziona sopra ogni tipo di rete, ma non con tutte è molto efficiente; ci sono dei servizi, come la sicurezza che non sono destinati a nessun layer. Per questi motivi tra un layer e un altro (qualunque) ci può essere il middleware, un livello destinato ad aggiungere o modificare qualcosa di già esistente.
Non è standardizzato, quindi nessuno ci dice come implementarlo, dove, o per cosa dovrebbe essere utilizzato.
# ADDRESS
Un indirizzo del livello fisico non esiste; l'indirizzo è un qualcosa di logico e il livello fiico è composto da materiale fisico (rame o radio). Ci sono tre tipi di indirizzi:
##### MAC
È l'indirizzo della scheda di rete, dove ogni scheda ha almeno un indirizzo.
- Si tratta dell'indirizo usato sulla rete locale. Nei contesti di virtualizzazione si hanno anche più indirizzi MAC, l'importante è che siano all'interno della LAN.
- Non sempre è costante (Bluetooth e wifi).
- Ad un indirizzo MAC si possono assegnare più indirizzi IP.
##### Numerical (IP)
È l'indirizzo necessario per il routing e che identifica un host nella rete.
- Deve essere univoco globalmente.
- È lungo 32 bits nell'IPv4 e 128 nell'IPv6. Deve essere trattato tramite bit e non byte (tipo un vettore di 4 o 16 elementi).
- Spesso è assegnto dal provider internet.
- A un indirizzo IP teoricamente si possono assegnare più indirizzi MAC, ma nella pratica l'unico motivo per farlo è avere una macchina ridondata nel numero delle schede (dove comunque una serve solo di backup e di solito non risponde).
- Prima (1984) si parlava di [classful netwoking](classful%20netwoking.png). Adesso si parla di CIDR (Classless Interdomain Routing):
	- Usato nell'IPv6.
	- Non si fa più differenza tra netID e subnetID.
	- Il suo scopo è quello di comprimere le tabelle di routing. Due indirizzo che differiscono solo nel bit meno significativo vengono accorpati (le due entry 150.217.8.0/24 e 150.217.9.0/24 possono essere accorpate in 150.217.8/23).
##### Alphanumeric
- Non ha vincoli.
- È usato perché si ricorda meglio dell'IP e non deve cambiare se cambio IP (ad esempio che cambio la posizioen del server o se cambio provider).
- Può essere legato a più IP. Magari uno stesso servizio mi risponde in base a dove sono da macchine diverse.