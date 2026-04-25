# LIVELLI (ARCHITETTURA)
L'obiettivo delle organizzazioni (es: IEEE, IETF) è quello di standardizzare il dominio dell'IoT per evitare un mercato molto frammentato (con soluzioni diverse) e per aumentare l'interoperabilità tra dispositivi eterogenei.
Si vuole quindi rivedere la [pila protocollare](stack%20di%20IoT%20architecture.png) di internet per definire un'architettura specifica per il mondo IoT.
### Livello fisico e MAC
- IEEE 802.15.4
	È una specifica tecnica per reti wireless a basso consumo (WPAN, Wireless Personal Area Network). Si basa sulla comunicazione a breve distanza con basso consumo energetico e bassa larghezza di banda. Un esempio è il protocollo zigbee.
- IEEE [802.11ah](Wi-Fi%20HaLow.md#802.11ah)
	È noto anche come Wi-Fi HaLow ed è progettata per dispositivi a bassa potenza. Copre lunghe distanze (1km) e serve molti dispositivi ($10^5$).
- [Bluetooth Low Energy (BLE)](Bluetooth%20Low%20Energy%20(BLE).md)
	È il bluetooh low energy. Copre una distanza piccola (100m) e usa poca potenza (batteria di 1 anno). Ha una topologia a stella, utile in sistemi master-slaves.
### Livello network
Necessario per identificare ogni dispositivo in modo univoco. Si usa:
- IPv4 + NAT
	È una soluzione complessa che vuole garantire la raggiungibilità in situazioni di limitata scalabilità.
- IPv6
	Permette moltissimi indirizzi, sia link-local che global. Integra inoltre il lato di sicurezza.
- 6LoWPAN
	È un adaptation layer che permette di usare IPv6 sfruttando poca energia.
### Livello trasporto
La scelta è UDP.
Si vuole infatti una comunicazione che sfrutti poca energia, leggera e non intensiva per la CPU; quindi TCP non è la soluzione.
### Application layer
##### WoT
Quando si parla di Web of things (WoT) si intende basare la gestione delle informazioni come fa il web, cioè usando il paradigma [REST](REST.md): i server collezionano documenti; i client, che sono leggeri e contengono poche informazioni, eseguono richieste ai server.
Le caratteristiche principali del web sono il suo essere distribuito e molto poco accoppiato con i suoi componenti, la sua scalabilità (nel numero di dispositivi che possono interagire al suo interno), la disponibilità (le informazioni sono sempre disponibili), l'interoperabilità, la sicurezza e l'evoluzione (aggiornare il server implica aggiornare i client).
Basandoci sul web tradizionale si possono creare applicazioni e servizi che scalino nel tempo per numero di oggetti connessi. Inoltre, grazie all'utilizzo di soluzioni ampiamente rodate, risulta molto più semplice manutenere e aggiornare il sistema.
##### Protocolli
Sulla base di quanto detto sopra si aggiungono le funzionalità dell'IoT tramite l'application layer.
I protocolli dell'application layer utilizzati sono:
- [CoAP](CoAP.md)
- [[CoSIP]]
- [[LwM2M]]
- [[MQTT]]
- [[AMQP]]