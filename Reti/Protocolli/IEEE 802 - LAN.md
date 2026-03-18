Lo standard IEEE802 si occupa di sviluppare reti LAN (Local Area Network) e MAN (Metropolitan Area Network), cioè ret i cui sevizi sono limitati ad aree geografiche limitata (Qualche chilometro per le LAN e decide di chilometri per le MAN). Per garantire un'affidabilità maggiore vengono usati mezzi fisici di qualità superiore rispetto alle reti più distribuite.
## STANDARD
- [IEEE 802.11 - WiFi](IEEE%20802.11%20-%20WiFi.md) e [Wi-Fi HaLow](Wi-Fi%20HaLow.md)
- [IEEE 802.16 - WiMAX](IEEE%20802.16%20-%20WiMAX.md)
- [IEEE 802.15.1 - Bluetooth](IEEE%20802.15.1%20-%20Bluetooth.md) e [Bluetooth Low Energy (BLE)](Bluetooth%20Low%20Energy%20(BLE).md)
- [[IEEE 802.15.4]]
- [RFID](RFID.md)
- [DQDB](DQDB.md)
- [FDDI](FDDI.md)
## LIVELLI
L’IEEE802 ha standardizzato 3 livelli ([Figura 4.22](Figura%204.22.png)), che hanno un’analogia funzionale con il livello fisico e collegamento della pila protocollare [ISO/OSI](ISO%20OSI.md).
##### LLC (Logical Link Control)
È uno standard unico, che si interfaccia con tutte le possibili implementazioni del livello [MAC (Medium Access Control)](IEEE%20802%20-%20LAN.md#MAC%20(Medium%20Access%20Control)). Ha il compito di controllare l’integrità del flusso informativo; vista l’affidabilità dei mezzi fisici questo controllo avviene su base E2E.
Ci sono tre tipi di servizio:
- Logical data link
	È un servizio [connectionless](Architettura%20a%20livelli.md#SERVIZIO) senza riscontro: i datagram sono trasmessi in modo indipendente tra loro e senza la garanzia che arrivino nell’ordine in cui sono stati generati; una volta inviati il mittente non deve attendere nessuna conferma di ricezione; alla destinazione non sono previste funzioni per la correzione di possibili errori.
- Logical data link alternativo
	È un servizio [connectionless](Architettura%20a%20livelli.md#SERVIZIO) con riscontro: rispetto al precedente il mittente deve attendere la conferma di ricezione del datagram.
- Data link connection
	È un servizio [connection oriented](Architettura%20a%20livelli.md#SERVIZIO).
##### MAC (Medium Access Control)
Ha il compito di gestire la condivisione dell'accesso alla rete da parte degli utenti che ne fanno parte; in particolare deve risolvere le possibili collisioni, cioè i conflitti derivanti da accessi contemporanei che portano alla perdita di informazione.
Le tecniche di condivisione del canale sono:
- [Polling](Tecniche%20di%20accesso%20MAC.md#POLLING)
- [Aloha](Tecniche%20di%20accesso%20MAC.md#ALOHA)
- [CSMA](Tecniche%20di%20accesso%20MAC.md#CSMA)
- [CSMA/CA](Tecniche%20di%20accesso%20MAC.md#CSMA/CA)
##### PL (Physical Layer)
Si occupa della trasmissione/ricezione dei bit nel/dal canale di comunicazione, che è il mezzo fisico con cui si trasmettono i dati.