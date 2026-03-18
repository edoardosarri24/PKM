Nel [grafo di internet](internet%20graph.png) (che dovrebbe mettere al centro gli autonomous sysem (AS) con più traffico in entrata e in uscita) ogni pallino è un AS. Tra i vari AS non esiste una gerarchia o una centralizzazione e i collegamenti avvengono tramite accordi commerciali; è quindi una mappa auto organizzata.
Siccome la sicurezza di internet è in funzione della sicurezza di ogni AS, non possiamo mai dire che internet è un posto sicuro.
### Concetti base
- Subnet
	È un segmento di rete identificato dalla coppia network address e subnetmask. Tra due subnet diverse ci deve essere un router che le collega.
- AS
	È un concetto amministrativo e identifica chi e come gestisce un segmento di rete; un AS avrà un insieme di subnet, ma i due concetti sono scollegati.
	All'interno di internet un AS è un insieme di prefissi di routing del protocollo internet (IP) sotto il controllo di uno o più operatori che definiscono una policy di routing.
### Interconnessione
- Source host
	- Crea un pacchetto per il destinatario.
	- Controlla se il destinatario è nella stessa subnet:
		- Se si, trasforma l'indirizzo IP in un indirizzo MAC e invia il pacchetto.
		- Altrimenti, invia il pacchetto al router giusto nella propria subnet.
- Router
	- Controlla l'IP del destinatario e ipotizza la sua subnet.
	- Se il destinatario è all'interno di una subnet direttamente collegata al router allora invia il pacchetto
	- Altrimenti invia il pacchetto al prossimo router.
- Subnet
	Invia i pacchetti dalla sorgente alla destinazione secondo i propri meccanismi.
# ISO/OSI
- [ISO/OSI](ISO%20OSI.md)
# TCP/IP
### [TCP/IP](TCP%20IP.md)
### [IPv6](IPv6.md)