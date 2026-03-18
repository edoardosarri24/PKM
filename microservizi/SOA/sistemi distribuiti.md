Un sistema distribuito è un insieme di elementi software o hardware, detti nodi, ognuno dei quali è indipendente, ma che devono collaborare per uno scopo comune. In pratica è un sistema che esegue su più server invece che su una sola macchina.
Un sistema distribuito può essere implementato tramite [servizi](Service%20Oriented%20Architecture%20(SOA).md) locali oppure utilizzando servizi offerti da fornitori esterni.
##### Pattern
I pattern architetturali per i sistemi distribuiti sono:
- [[Master-Slaves architecture]]
- [Client-Server](Client-Server.md)
- [[Distributed component architecture]]
- [[Peer-to-peer architecture]]
# Vantaggi
- Condivisione delle risorse
	Il sistema è composto da tutte le risorse che si trovano sul nodo su cui gira un componente del sistema stesso. Queste possono essere memoria, CPU, stampanti e altro.
- Apertura
	Solitamente i sistemi distribuiti lavora su protocolli interni standard e per questo permettono l'interazione tra unità sw e hw di fornitori diversi.
- Simultaneità
	Un sistema distribuito è composta da più processi che girano contemporaneamente su macchine diverse e che comunicano durante la loro esecuzione.
- Scalabilità
	Le capacità di un sistema distribuito possono scalare quanto vogliamo se le richieste al sistema aumentano. In questo [kubernetes](microservizi/kubernetes/kubernetes.md) ci aiuta.
	Si deve poter scalare secondo più fattori:
	- Taglia
		In numero di risorse. Si parla di [scaling up](scaling.md#Vertical) quando vengono potenziate le risorse; si parla di [scaling out](scaling.md#Horizontal) quando viene aumentato il numero di risorse.
	- Distribuzione
		I luoghi dove sono i nodi possono essere molti e lontani.
	- Gestibilità
		La gestione del sistema deve rimanere fattibile quando aumentano le sue dimensioni. È solitamente la parte più complessa.
- Fault tollerance
	Se un sistema gira su una singola macchina molto potente e questa si rompe allora il costo per acquistarla nuova è elevato, ma se i vari nodi sono server economici allora questo non è un grosso problema.
# Svantaggi
- Un sistema distribuito solitamente è più complesso.
- La trasparenza verso l'utente è molto difficile da implementare. In teoria si vuole realizzare quella che è la multi-tenancy: un tenant (i.e., cliente) deve percepire di avere l'interno sistema per se, quando in realtà non è così. Nella pratica questo non è fattibile proprio a causa della distribuzione del sistema e gli utenti sono consci che il sistema non gira sulla propria macchina.
- È difficile specificare la QoS: potrebbe essere troppo costo mantenere quanto specificato; i parametri di QoS potrebbero andare in contrasto (e.g., affidabilità e throughput).
# Comunicazione
La comunicazione tra i componenti di un sistema distribuito può essere di due tipi:
- Remote Procedure Call (RPC)
	È il classimo modello sincrono basato su request e responce.
- Message System (MS)
	È il modello asincrono basato su messaggi.
##### Clock
I nodi della rete non si basano su un global clock e per questo motivo si possono avere problemi di coordinazione e sincronizzazione.
- Siccome i nodi possono essere molto distanti tra loro, un offset di 400ms è concesso. All'interno di una rete più piccola si parla di 40ms.
- Se il clock di un nodo ha un offset più elevato di quello appena indicato allora non è autorizzato a fare operazioni all'interno del sistema per problemi di sicurezza.
##### Middleware
I dispositivi software e hardware all'interno del sistema distribuito sono diversi (e.g., scritti in linguaggi diversi, usare processori diversi) e collaborano grazie a degli standard internet comuni. Serve però un componente che permetta di interfacciarsi tra loro e rappresenti le informazioni in modo comune.
Il middleware è un uguale per tutti i componenti, acquistato off-the-shelpf e che si trova tra l'OS e il livello applicativo. Il suo compito è:
- Serve come supporto alle interazioni: rende trasparente la posizione di ogni nodo, permette la condivisione di parametri e ad esempio gestisce gli eventi.
- Implementa i servizi comuni, come ad esempio un servizio di sicurezza. Questi sono implementati tramite libreire comuni installate su ogni componente del sistema distribuito.