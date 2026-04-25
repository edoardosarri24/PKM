##### SIP
Il Session Initialization Protocol è il protocollo standard per iniziare, terminare e modificare le sessioni a livello applicazione.
Quando una sessione è stabilita i due end point si scambiano media (es: chiamate VoIP) tramite protocolli dell'application layer.
L'analisi dei pacchetti è text-based (esattamento come HTTP).
##### CoSIP
È stato pensato per portare SIP all'interno delle LLN (Low power and Lossy Network), cioè per gestire sessioni in modo leggero per dispositivi constrained.
Come [CoAP](CoAP.md) aveva fatto per HTTP, CoSIP mantiene la struttura di SIP, ma analizza i pacchetti informato binario e non testuale.
# PACCHETTI
![pacchetti CoSIP](pacchetti%20CoSIP.png)
##### Header
- Version (2 bit)
- Type (2 bit)
- TKL (4 bit)
- Code (8 bit)
	Codifica i metodi per le request e per le response.
- Message ID (16 bit)
##### Messaggi
I messaggi CoSIP si baano sulla struttura dei messaggi SIP:
- Si può avere una richiesta o una risposta.
- Una sequenza di SIP header field, che contengono le informazioni aggiuntive necessarie per gestire la sessione. Questi sono codificati in binario (non text-based come SIP) nel campo opzione dell'header CoSIP.
- Se presente, il corpo del messaggio. È contenuto nel payload.
# SCENARI
##### Service discovery
Tramite CoSIP si può registrare e trovare i servizi che un altro dispositivo offre tramite il protocollo [CoAP](CoAP.md#Resource%20directory); i servizi sono registrati e presi da una resource directory (RD).
- Registrazione
	Il dispositivo, che ha un server CoAP, invia una richiesta REGISTER alla RD tramite il protocollo CoSIP; la richiesta include l'indirizzo del servizio e l'URL per accedere alla risorsa. La RD conferma la registrazione del servizio con OK.
- Scoperta
	Un dispositivo fa una GET a */.well-known/core* e riceve una lista di servizi in un messaggio 2.05 tramite CoAP.
##### Creazione della sessione
Una sessione viene creata quando due dispositivi vogliono scambiarsi informazioni. Una volta che la sessione viene stabilita i due end point si scambiano informazioni in modo p2p.
Durante la creazione della registrazione i due end point possono negoziare i parametri della comunicazione, che saranno poi usati per tutte le comunicazioni successive.
- Un dispositivo registra il proprio indirizzo di contatto presso un gateway IoT usando CoSIP.
- Il dispositivo che vuole iniziare una sessione invia una richiesta INVITE al gateway IoT che la gira al primo dispositivo; quest'ultimo risponde OK e l'altro finalizza la creazione della sessione con un ACK.
- A questo punto i dispositivi possono scambiarsi informazioni.
##### Subscribe/notify
CoSIP grazie al fatto che è derivato da SIP permette di implementare in modo nativo il concetto di Subscribe/notify.
- I notificatori i registrano al server CoSIP; questa fase è detta publishing.
- Quando un dispositivo vuole richiedere le notifiche invia una SUBSCRIBE al notificatore, che risponde con un OK; quando lo stato del notificatore cambia invia una NOTIFY al subscriber; quest'ultimo invia un OK per conferma.