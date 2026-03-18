È uno standard che specifica [fisico](IEEE%20802%20-%20LAN.md#PL%20(Physical%20Layer)) e livello [MAC](IEEE%20802%20-%20LAN.md#MAC%20(Medium%20Access%20Control)) per reti WPAN (Wireless Personal Area Network) a basso bit rate; è usato ad esempio nelle reti di [sensori](WSN%20-%20Wireless%20Sensor%20Networ.md). È caratterizzato, oltre al basso bit rate, da una bassa potenza del segnale e un basso consumo energetico.
I dispositivi che interagiscono con questo standard sono di due tipi:
- RFD (Reduced Function Device)
	Sono dispositivi molto semplici con una limitata potenza di calcolo. Non possono comunicare tra loro, ma solo con i dispositivi FFD.
- FFD (Full Function Device)
	Sono dispositivi più complessi e possono svolgere un ruolo di coordinatore della rete. Possono comunicare qualsiasi altro dispositivo all’intero della loro portata radio.
# ARCHITETTURA
La comunicazione tra pari livello nei vari nodi avviene sempre su base L2L.
### Fisico
Il livello fisico offre due tipi di servizio:
- Data service
	Si occupa della trasmissione di pacchetti, che prendono il nome di PPDU (Packet protocol Data Unit).
- Management service
	Si occupa di fornire servizi ai livelli superiori, come la ricezione e la trasmissione dei pacchetti oppure l’attivazione e disattivazione della trasmittente.
### MAC
Il livello MAC fornisce due tipi di servizi:
- Data service
	Si occupa di trasmettere i pacchetti MPDU (Che quando passano o al livello fisico prendono il nome PPDU) verso il livello fisico.
- Management service
	Fornisce diversi servizi: gestisce l’accesso condiviso al canale, si occupa dell’invio dei messaggi di riscontro ACK e dell’associazione e dissociazione dei dispositivi dalla rete.
# ACCESSO
La tecnica di accesso si basa sulla casualità della [CSMA/CA](Tecniche%20di%20accesso%20MAC.md#CSMA/CA). Sono previste due strategie:
- Beaconless
	Il nodo che vuole accedere alla rete ascolta il canale: se lo trova libero trasmette, attendere un certo periodo prima di ritentare l’accesso.
- Beacon-enabled
	Il coordinatore usa dei pacchetti di sincronismo (Beacon frame) per dividere il tempo in intervalli detti superframe. Ogni superframe può ospitare sia fasi CAP (Contention Access Period) con contesa, sia fasi CFP (Contention Free Period) senza contesa.