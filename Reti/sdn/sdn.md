Le normali [reti](Reti%20di%20telecomunicazioni.md) dove la gestione del [routing](routing.md) è all'interno dei router stressi ha dei grandi [limiti](routing.md#Limiti). Per risolvere questi soliti è stato introdotto il Software Defined Networking (SDN): si tratta di un paradigma (i.e., non è un protocollo) basato sulla separazione di chi prende le decisioni e di chi le esegue.
# Architettura
Si basa su due layer di astrazione [disaccopiati](decoupling%20of%20control%20and%20data%20plane.png): il data plane e il control plane.
![layers](layers.png)
##### Data plane
Il [data plane](data%20plane.md) è composto da dispositivi di rete, che si occupano solo di trasmettere i pacchetti basandosi sul proprio stato (i.e., tabelle di inoltro).
##### Control plane
Il [control plane](control%20plane.md) è il controllore che si occupa di definire lo stato del data plane, cioè di definire il protocollo usato per popolare le forwarding tables. A volte si può anche dividere in control plane e management plane: il primo impone la policy sui dispositivi di rete; il secondo, composto da umani, definisce la policy.
##### Application plane
Le applicazioni sdn sono dei programmi che utilizzano la vista astratta della rete per decidere come instradare i loro pacchetti nella rete.
Per implementare queste decisioni utilizzano l'[interfaccia nord](control%20plane.md#Nord) del control plane.
# Vantaggi
Tramite SDN possiamo avere numero vantaggi:
- Poter implementare ogni funzionalità nel modo più efficiente possibile, senza dover trovare un compromesso per tutte.
- Permette ai dispositivi di rete di fare una sola cosa in modo molto efficiente.
- Dare al control plane una visione globale della rete invece che avere dispositivi di rete che hanno una visione locale di essa.
- Poter creare una nuova funzionalità o aggiornarne una esiste in modo molto rapido.
- Usare una sola rete (i.e., un insieme di dispositivi di rete) per implementare più cose. Tale rete può essere composta da dispositivi di diversi produttori e il programmatore è agnostico a questo tipo.
# Casi d'uso
- Internet provider
	Gli internet provider fisici, cioè quelli che possiedono effettivamente i dispositivi di rete, possono vendere il loro utilizzo agli operatori virtuali. Questi dovranno riporgrammare i dispositivi per includere le informazioni che gli interessano.
- Parental control
	Il parental control può essere sviluppato direttamente sul dispositivo, in modo che non faccia passare certi tipi di pacchetto. Essendo il forwarding molto più veloce di un'analisi software, questa soluzione gode di prestazioni più elevate.