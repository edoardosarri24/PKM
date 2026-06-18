Le funzioni di rete sono solitamente installate in dispositivi di rete (e.g., ), detti dispositivi middlebox i quali implementano una sola funzionalità.
Con le Networ Function Virtualizzation (NFV) vogliamo virtualizzare tutte le funzioni di rete con l'idea di disaccoppiarle dall'hardware su cui eseguono (Point of Presence): possiamo quindi rappresentare funzioni di rete come un call graph di componenti che interagiscono. Questa idea è già introdotta da [sdn](sdn.md), il quale però a livello teorico opera fino al livello 4 di [TCP/IP](TCP%20IP.md) e quindi si occupa solo dell'instradamento.
# Funzioni di rete
Le funzioni di rete virtuali (Virtual Network Functions, VNF) sono servizi costruiti per la rete e che senza di lei non avrebbero senso: elaborano quindi informazioni che sono in transito su di essa e non che sono ferme in un server.
Le VNF sono [virtuali](virtualizzazione.md), cioè eseguite su Virtual Machine o Docker: una volta definita una funzione di rete si può eseguire su più nodi.
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
	Funzione che anonimizza l'IP di un host. È simile al NAT ma il suo compito è focalizzato sull'oscuramento, mentre il NAT è quello di permettere la comunicazione tra più host in una rete con l'esterno.
# Network service
Dal punto di vista concettuale un servizio di rete è ciò che viene fornito all'utente. Da un punto di vista tecnico un servizio di rete è un grafo di funzioni di rete connesse da un'infrastruttura.
##### Network Fucntion Forwarding Graph
Una funzione di rete (VNF) è un insieme di VNFc (VNF component), attività più semplici che sono connesse tra loro e orchestrate.
L'ordine con cui sono eseguite le funzioni e gli end point che le usano sono definite dal Network Fucntion Forwarding Graph (NF-FG). I collegami in un NF-FG sono le interfacce logiche (vedi architettura).
Un NF-FG permette di astratte completamente dal livello fisico, senza preoccuparsi quindi di come i dati vengono scambiati dalla rete, e dove la funzione di rete è effettivamente eseguita. Inoltre possiamo creare NF-FG che sono astrazioni di altri grafi, dove quindi un nodo è a sua volta un sottografo.
##### Architettura
L'architettura su cui si basano i servizi è composta da:
- End point
	Sono i device dei clienti. L'operatore non ha nessuna influenza su questi.
- Network Function
	Sono le funzioni di rete che implementano servizi per l'utente.
- Infrastruttura fisica
	Sono i nodi su cui le funzioni di rete virtuali eseguono realmente e le connessioni tra loro. Una funzione di rete virtuale può eseguire su più nodi e uno stesso nodo può gestire più funzioni di rete.
- Interfacce logiche
	Le funzioni di rete e gli end point comunicano tra loro con canali virtuali: vuol dire che possono parlare ma i dati effettiva passano dall'infrastruttura fisica.
# Architettura
L'architettura del framework NFV è composta da varie parti: NFVI, VNF e NFV-MANO. La prima e la più importante referenza a questo è lo standard ETSI.
![NFV architeccture](NFV%20architeccture.png)
##### NFVI
Si tratta della NFV Infrastructure, un insieme di computing, storage e rete.
Sopra il livello fisico abbiamo un layer di virtualizzazione; chi interagisce con l'infrastruttura non ha conoscenza delle vere risorse fisiche e il livello di virtualizzazione.
- Le risorse di computing sono general purpose, dove ogni funzione può eseguire in ogni nodo.
- Le risorse di rete servono per trasportare i dati tra i vari nodi.
##### NFV-MANO
Il MANO è il MAnagement and Orchestration layer che ha visione sia delle NFV e dell'NFVI e si occupa orchetsrare il ciclo di vita dell VFN.
È composto da tre blocchi:
- VIM
	È il Virtualized Infrastructure Manager. Gestisce le risorse fisiche e la loro virtualizzazione: si occupa di conoscere lo stato dei dispositivi di rete.
	In ambienti complessi ci possono essere più VIM, ognuno dei quali gestisce una partizione di nodi.
- VNFM
	È il Virtual Network Function Manager. Gestisce il ciclo di vita delle funzioni di rete, visto che ognuna di queste può essere composta da una o più VNFc.
	In pratica si occupa di: istanziare nuove VNF; modifica e aggiornamento del codice di una VNF; scalilabilità delle VNF; raccolta dei dati a runtime; terminazione delle VNF; conoscenza di quali VNF ci sono.
- NFVO
	È l'NFV Orchestrator che orchestra i due ambiti:
	- Risorse
		Gestisce (i.e., autorizza e impegna) le risorse di rete, assegnandole ai vari VIM.
	- Servizi
		Crea su base E2E i servizi di rete, gestendo quindi l'interazione dell VNF per la creazione di tale servizio.
##### VNF
In questo livello sono presenti:
- Le NF effettive.
- EM
	È l'Element Manager, un componente che si occupa di gestire i falli, le configurazioni, le performance e la sicurezza di una NF.
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
##### Svantaggi
- Le prestazioni diminuiscono perché l'implementazione passa da essere hardware (i.e., eseguite in un dispositivo specializzato) a essere software.