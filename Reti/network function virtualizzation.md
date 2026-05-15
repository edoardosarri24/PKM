Le funzioni di rete sono solitamente installate in dispositivi di rete (e.g., ), detti dispositivi middlebox i quali implementano una sola funzionalità.
Con le Networ Function Virtualizzation (NFV) vogliamo virtualizzare tutte le funzioni di rete con l'idea di disaccoppiarle dall'hardware su cui eseguono; questa idea è già introdotta da [sdn](sdn.md), il quale però a livello teorico opera fino al livello 4 di [TCP/IP](TCP%20IP.md) e quindi si occupa dell'instradamento.
# Funzioni di rete
Le funzioni di rete virtuali (Virtual Network Functions, VNF) sono servizi costruiti per la rete e che senza di lei non avrebbero senso: elaborano quindi informazioni che sono in transito su di essa e non che sono ferme in un server.
##### Dimensioni
Ogni funzione di rete può essere scomposta in tre dimensioni:
- Rete
	Sono le funzioni di rete classiche, come il [forwarding](routing.md), il cambio di testate o il dropping di pacchetti.
- Computing
	Sono funzioni che hanno come scopo principale il calcolo e quindi hanno bisogno di potenza computazionale.
- Storage
	Sono funzioni basatew sulla memorizzazione (e.g., NAS).
##### Esempi
- NAT
	Si tratta di una funzione soprattutto di rete. Il suo obiettivo è quello di cambiare l'IP del mittente con il proprio IP, in modo che dall'esterno sia visibile sono un IP pubblico. Può essere anche un cambio che cambia di IPv4 a [IPv6](IPv6.md).
- Firewall
	È una funzione che sfrutta gli header di livello 3 (IP) e 4 (porta) per prevenire certi tipi di attacco (e.g., DoS).
- Load balancer
	È una funzione che inoltra un pacchetto a un server in modo da massimizzare una qualche metrica.
- Anonimizzatore
	Funzione che anonimizza l'IP di un host. È simile al NA ma il suo compito è focalizzato sull'uscamento, mentre il NAT è quello di permettere la comunicazione tra più host in una rete con l'esterno.
# Conseguenze
I problemi che ci sono nell'eseguire le funzioni di rete in middlebox, e che sono risolti da NFV, sono principalmente:
- Il costo iniziale. Spesso sono il 50% dei dispositivi di rete totali.
- L'installazione deve essere fatta in un preciso posto e successivamente lo spostamento è molto complesso.
- Solitamente i middlebox non parlando con dispositivi di altre marche. Questa comunicazione è invece consentita da router o switch.
- I middlebox fanno una cosa specifica e basta, non fanno più cose: se vogliamo cambiare funzioni o implementarne di nuove si devono comprare altri dispositvi o cambiare quelli esistenti.
##### Vantaggi
- Si disaccoppia hardware e software.
- Possiamo scalare le funzioni in quantità e posizione.
- Posisamo comprare le funzioni di rete da diversi furnitori software.
- Funzioni complesse (VNF) possono essere scomposte in attività più semplici (VNF component, VFNc) e poi connesse e orchestrate.

##### Svantaggi
- Le prestazioni diminuiscono perché l'implementazione passa da essere hardware (i.e., eseguite in un dispositivo specializzato) a essere software.






### Principi
I principi che stanno alla base di Network Function Virtualization (NFV) sono:
- Le Network Function (NF) complesse vengono decomposte in funzioni più piccole e riutilizzabili, cioè vengono gestite a livello sw.
- La gestione e l'organizzazione avviene tramite un orchestratore.
### Vantaggi
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