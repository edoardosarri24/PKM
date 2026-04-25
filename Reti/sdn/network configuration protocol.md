Il Network Configuration Protocol (NETCONF) è un protocollo usato nella comunicazione tra il [control plane](control%20plane.md) e il [data plane](data%20plane.md) in [sdn](sdn.md) per la configurazione dei dispositivi di rete.
# Configurazioni
Una configurazione è l'insime dei parametri che definiscono il comportamento del sistema.
Esempi di configurazioni che NETCONF gestisce sono:
- Nome dello switch.
- IP del controller, utile durante la fase di [link discovery](control%20plane.md#Link%20discovery).
- Configurazione di [OSPF](algoritmi%20con%20tabella.md#OSPF%20(Open%20Shortest%20Path%20First)), [BGP](border%20gateway%20protocol.md).
# Comunicazione
I dati sono passati in formato XML (e.g., JSON o YANG) tramite chiamate RPC protetto da ssh o TLS.
##### Client-server
È il modello di comunicazione request/responce implementato tramite chiamate RPC protetto da ssh o TLS: il server invia i dati e il client risponde con un messaggio di conferma o errore.
Le primitive principali sono:
- $get-config$
- $edit-config$
- $copy-config$
- $delete-config$
- $close-session$.
##### Publisher-subscriber
È il modello asincrono usato solitamente per le telemtrie.
Le primitive principali sono:
- $create-subscription$
- $notification$