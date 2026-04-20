Il Network Configuration Protocol (NETCONF) è un protocollo usato nella comunicazione tra il [control plane](control%20plane.md) e il [data plane](data%20plane.md) in [sdn](sdn.md).
# Scopo
Il suo scopo è quello di preparare il dispositivo a livello fisico e semi permanente (i.e., si utilizza all'inzio o di rado) in modo che poi possa essere gestito l'instradamento tramite [OpenFlow](OpenFlow.md).
La configurazione del dispositivo consiste prevalentemente in installazione, manipolazione e cancellazione di configurazioni. Un esempio è l'accensione di una porta del dispositivo di rete.
# Comunicazione
I dati sono passati in formato XML (e.g., JSON o YANG) tramite chiamare RPC solitamente tramite ssh: il server invia i dati e il client risponde con un messaggio di conferma o errore.
##### Architettura
Ci sono due architetture di comunicazione che possiamo utilizzare:
- Client-server
	Il Network Management System (NMS) interroga il router sul suo stato.
	Le primite principali sono: $get-config$, $edit-config$, $copi-config$, $delete-config$ e $close-session$.
- Publisher-subscriber
	Il publisher è il controllor, mentre il dispositivo è il subscriber.
	Le primitive principali sono: $create-subscription$ e $notification$.