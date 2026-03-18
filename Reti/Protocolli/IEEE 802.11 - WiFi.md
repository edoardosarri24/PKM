Lo standard IEEE 802.11, conosciuto anche come WiFi (Wireless Fidelity), prende il nome dalla WiFi Alliance, l’organizzazione che certifica tutti i dispositivi in grado di creare una rete [LAN](IEEE%20802%20-%20LAN.md) senza fili (WLAN), cioè una rete dove i nodi sono collegati tramite un canale radio (Aria); ogni WLAN è identificata attraverso il suo SSID (Service Set Identifier), cioè un nome solitamente scelto dall’utente.
Ci possono essere tre tipi di WLAN:
- Infrastrutturata
	I nodi, detti client, sono collegati solo all’AP (Access Point) e comunicano tra loro solo tramite esso.
- ESS (Extended Service Set)
	È una rete composta da più AP collegati in modo cablato tra loro.
- Ad hoc
	Non si prevede l’uso di un AP; i client si organizzano tra loro creando una rete che può non comprendere collegamenti verso l’esterno.
## CONSEGUENZE
- Vantaggi
	- Facilità d'installazione.
	- Sccesso ubiquo, cioè in ogni zona.
- Svantaggi
	- Difficolta nel garantire l'integrità delle informazioni. Per questo lo standard raccomanda la frammentazione dei pacchetti in piccole unità, in modo da limitare la possibilità di introdurre errori nei singoli frammenti.
	- Scarsa sicurezza, in quanto il canale è accessibile da chiunque in qualunque momento.
## STORIA
L'obiettivo con cui lo stardard si è evoluto è quello di aumentare la velocità di trasmissione e la portata del segnale.
- 802.11a/b
	È la prima generazione di WiFi ed è stata rilasciata nel 1997.
- 801.11g
	Lavora sulla frequenza 2.4GHz eha una portata di circa 100m.
- 802.11n
	È noto come WiFi4 e lavora sulle frequenze 2,4GHz e 5GHz.
- 802.11ac
	È noto come WiFi5 e permette velocità pari a 1,3Gbps.
- 802.11ax
	È noto come WiFi6.
## ACCESSO
La tecnica di accesso al canale è la [CSMA/CA](Tecniche%20di%20accesso%20MAC.md#CSMA/CA).
Il motivo per cui nelle reti WiFi non si usa la tecnica [CSMA](Tecniche%20di%20accesso%20MAC.md#CSMA) definita dal livello [MAC](IEEE%20802%20-%20LAN.md#MAC%20(Medium%20Access%20Control)) dello standard IEEE802, è la presenza di interferenze molto maggiori nei canali radio rispetto ai mezzi fisici cablati; per questo motivo rimanendo in ascolto sul canale dopo la fase di accesso (Come nella tecnica CSMA/CD) si possano rilevare delle false collisioni.
## CONSUMO ENERGETICO
I nodi di una rete WiFi sono prevalentemente dispositivi mobili, quindi il consumo energetico è un’aspetto fondamentale. L'idea è ridurre al minimo il consumo di energia del dispositivo quando non è necessario accedere alla rete.
In un contesto generale si puà usare il NAV (Network Allocation Vector), cioè un contatore a decremento attivato da un terminale quando riceve il messaggio CTS e che blocca il terminale stesso; in questo modo esso non proverà inutilmente ad utilizzare la rete quando questa è occupato.
In ambiti infrastrutturati invece  ci sono due soluzioni ulteriori:
- Beacon frame
	L’AP invia un messaggio di beacon ad ogni terminale ogni lasso di tempo (Che i terminali devono conoscere), informandoli riguardo alla presenza di pacchetti ad essi destinati.
	I nodi si mettono in ascolto solo in questi istanti di tempo: se ci sono dei pacchetti da ricevere rimangono attivi e comunicano con l'AP, altrimenti tornano in modalità risparmio energetico.
- APSD (Automatc Power Safe Delivery)
	L’AP memorizza i pacchetti destinati ad un terminale in un buffer.
	Quando un nodo comunica con l’AP allora esso rimanere attivo anche per ricevere i pacchetti pendenti.