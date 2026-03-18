Solitamente le funzioni di rete (es. routing, firewall, NAT, bilanciamento del carico) sono installate sui dispositivi di rete proprietari (prodotti da una data azienda), che quindi integrano hw e sw in un solo dispositivo.
I problemi di questa soluzione sono: ogni dispositivo e ogni azienda espone le proprie API e richiede quindi competenze specifiche; questi dispositivi seguono standard proprietari e quindi l'integrazione è complessa.
[SDN](SDN.md) risolve questo problema, ma solo fino al livello 4 (di TCP/IP); opera solo sull'instradamento, ma non interviene in alcun modo sulla computazione e sullo storage.
### Principi
I principi che stanno alla base di Network Function Virtualization (NFV) sono:
- Le Network Function (NF) complesse vengono decomposte in funzioni più piccole e riutilizzabili, cioè vengono gestite a livello sw.
- La gestione e l'organizzazione avviene tramite un orchestratore.
### Vanaatggi
- Minimizza il tempo di attivazione di un nuovo servizio.
- Minimizza i costi per l'attivazione e la gestione di un nuovo servizio.
- Permette di utilizzare una singola rete per più applicazioni.
# ARCHITETTURA
La prima architettura NFV, e quella più importante, è [l'architettura di European Telecomunication Stardand Institude (ETSI)](ETSI%20atchitecture%20NFV.png).
### Network Functions Virtualization Infrastructure (NFVI)
È l'hw generico dove sono in esecuzione in modo virtualizzato molte e diverse NF. Non si parla di hw localizzato, ma che segue il principio del cloud: è ditribuito su una serie di nodi (server).
I componenti hw che appartengono a questo livello sono la potenza computazionale, la memoria e la rete.
### Funzioni VNF/VNFC
È la parte sw, eseguita come Saas nei cloud.
Il vari sw possono essere eseguiti o in una Virtual Machine (VM) o in un container: nel primo caso prendono il nome di Virtualized Network Function (VNF), mentre nel secondo di Virtualized Network Function Component (VNFC).
### Orchestratore MANO
Il MAnagement and Netowrk Orchestration (MANO) è la parte fondamentale dell'architettura NFV. Si divide in tre parti, ognuna delle quali opera a un livello diverso.
##### Orchestratore
Il suo compito è l'installazione di un nuovo servizio, gestendone l'intero ciclo di vita. Opera ad alto livello, disaccoppiando le VNF/VNFC dall'infrastruttura NFVI, cioè le funzioni dalle risorse del sistema.
##### VNF Manager
Gestisce il ciclo di vita di ogni istanza VNF/VNFC: configurazione, creazione, allocazione delle risorse e loro monitoraggio, migrazione e terminazione.
##### Virtualized Inrastructure Manager (VIM)
Gestisce l'interazione tra le VNF e le risorse di elaborazione, memorizzazione e rete e la loro virtualizzazione.
Gestisce la visibilità delle risorse e indica la loro disponibilità a chi ne fa richiesta.
Esegue la raccolta e l'analisi dei dati sulle prestazioni delle risorse, esegue il loro monitoraggio e l'ottimizazione.
- Per controllare la connessione di queste risorse di include nell'architettura NFV il controller SDN, che conoscendo la rete nel suo insieme può gestire al meglio i vari collegamenti.