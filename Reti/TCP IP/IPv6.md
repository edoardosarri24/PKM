L’IPv6 è un protocollo per l'implementazione del livello [internet](TCP%20IP.md#INTERNET%20(IP)%20-%20[IPv4](IPv4.md)%20-%20[IPv6](IPv6.md)) della pila protocollare [TCP/IP](TCP%20IP.md).
È stato introdotto principalmente per risolvere il problema della scarsità di indirizzi IP allocabili. [IPv4](IPv4.md) ha $2^{32}$ indirizzi possibili contro i $2^128$ di IPv6. È come se, paragonato un indirizzo a un granello di sappia, IPv4 fosse 4 secchielli e mezzo, mentre IPv6 6 pianeti Terra e mezzo.
I suoi vantaggi sono: più indirizzi, header semplificato e quindi si ha un algoritmo leggermente più veloce (maggiore overhead, ma i pacchetti sono processati più volecemente) e l'auto configurazione (cosa che lato sicurezza non ci piace).
# STORIA
![IPv6 timeline](IPv6%20timeline.png)
- 1993
	Nasce il CIDR, che permette di aggregare gli indirizzi nelle tabelle di routing. Questo ha causato un ritardo nella necessità di aggiungere indirizzi IP.
- 1995
	IPv6 è ufficialmente rilasciato e descritto in RFC1752 (un documento redatto dall'Internet Engineering Task Force (IETF)).
- 1997
	Una rete olandase totalmente in IPv6 va online.
- 2000
	SixXS inzia a operare. Ti facevano provare una serie di indirizzi IPv6 tramite tunneling.
- 2003
	La Cina e il South Korea dichiarano di voler diventare learder in IPv6. Questo perché sono molto popolate e non gli bastano gli indirizzi IP.
- 2004
	IPv6 è supportato dalla maggior parte dei nodi.
- 2005
	- Il governo degli USa decide che entro il 2008 tutte le agenzie federali devono migrare all'IPv6.
	- In india un provider, Sify, connette i suoi utenti tramite IPv6 e tutto funziona bene.
- 2008
	- IPv6 copre le olimpiadi della Cina.
	- L'europa dichiara di voler raggiungere entro il 2010 un fattore di utilizzo del 25% per gli utente. Questo non è andato a buon fine a causa dei costi che gli internet provider dovevano sostenere senza avere dei guadagni da questa operazione.
- 2012
	Gli indirizzi IPv4 sono finiti.
- 2020
	- IPv6 continua la sua espansione in modo lineare. Si stima che circa il 50% del traffico sia sotto questo protocollo (è comunque un dato difficile da stimare).
	- Per ottenere indirizzi IPv4 si deve entrare in una lista d'attesa. Questo perché chi li ha non li rilascia ma li vende
##### Ascesa IPv6
IPv6 ha avuto dalla sua sia svantaggi (costi) che vantaggi:
- I costi da sostenere erano/sono:
	- L'insegnamento di IPv6 a chi lavoro con esso.
	- La rottura di alcune subnet, delle tabelle di routing e de i firewall durante il passaggio.
	- Per gli internet provider c'è il costo del cambio router: si dovevano sia far spedire i vecchi router ai clienti, ma soprattutto andavano smaltiti quelli vecchi. Inoltre non ci possono essere guadagni da questa operazione: non si può caricare sul clinete una unznionalità tecnica e IPv6 non aggiungeva alcuna feature lato cliente.
- I ricavi sono:
	- SI eliminano i router NAT che per gli internet provider sono molto costisi.
	- Il 5G funziona con IPv6. Gli internet provider dovevano cambiare se volevano aderire a questo standard.
## HEADER
L’[header IPv6](IPv4-6%20header.png) ha una dimensione fissa di 40 byte; considerando questa dimensione fissa e avendo meno campi rispetto all'header IPv4 ha una maggiore velocità di interpretazione da parte dei router. I campi sono:
- Version
	Indica la versione del protocollo IP usata.
- Traffic class
	Ha una funzione simile al campo "type of service" dell'header IPv4 e serve per indicare il tipo di dati trasportato.
- Flow label
	Il flusso in rete è una serie di pacchetti che va da una sorgente a una destinazione; è identificato dalla quintupla TCP (L4 deve essere implementato come TCP), sorgente, destinazione, porta sorgente e porta destinazione (le porte sono un'informazione del livello superiore che non dovrebbe essere letta).
	Viene usato dal mittente per etichettati una serie di pacchetti che dovrebbero essere trattati come se appartenenti allo stesso flusso.
- Payload length
	Indica la grandezza in byte del payload.
- Next header
	Contiene il protocollo implementato al livello superiore oppure ci dice che dopo l'header ci sono i campi opzionali (che in IPv6 non sono nell'header ma nel payload).
- Hop limit
	Ha la stessa funzione del campo "time to live" dell'header IPv4: indica il numero massimo di salti che il pacchetto può eseguire da un router all’altro.
- Source/Destination address
	Sono gli indirizzi IP del mittente e del destinatario.
### Confronto IPv4
##### Checksum
- Non dovendo essere calcolato l'elaborazione di un pacchetto IPv6 è più veloce di uno IPv4.
- Se c'è un errore nell'header, e il pacchetto non viene consegnato a chi di dovere, il protocollo se ne frega. Si fa affidamento sulla qualità della rete, che ha un bassissimo bit error rate (alla creazione di IPv4 le reti non era così affidabili).
	- Si parla di bit error rate nel momento in cui i dati arrivano al livello MAC.
	- Il livello MAC ha un frame control sequence (nel trailer) che elimina i pacchetti con errori. I pacchetti che passano fanno parte del bit error rate residual.
	- Con package erorr rate si intendono invece i pacchetti errati che passano il controllo del checksum.
- Lato sicurezza il checksum non aiuta a non far passare pacchetti corrotti, cioè l'attaccante non incontra difficoltà nel aggirare il checksum.
##### Frammentazione
Mancano tutti i campi per gestire la frammentazione e quindi per garantire l’interoperabilità tra rete non omogenee, requisito che è fondamentale per il livello IP.
Nell’IPv6 la frammentazione viene gestita su base E2E e non L2L: questo garantisce che i pacchetti arrivino come sono stati inviati (dal punto di vista della dimensione, non dell'ordine). Se il mitente non ha informazione sul MTU per un determinato nodo allora inivia il pacchetto per intero; nel caso a un certo punto ci sia un segmento di rete che non supporta quelle dimensioni allora il mittente riceve un ICMPv6 Packet Too Big (ICMP è un protocollo di livello 3 che si occupa di gestire imessaggi di errore) contente l'MTU di tale link; frammenta il pacchetto in modo da poterlo inviare e riprova; segue questa idea finchè il destinatario non riceve l'informazione. Siccome il percorso migliore potrebbe cambiare ogni tanto si deve verifcare l'MTU mandando un pacchetto più grande; in caso non fosse cambiato si riceverà nuovamente un ICMPv6 Packet Too Big.
Ricordiamo che IPv6 richiede che la minimum MTU per ogni nodo sia 1280 byte; quindi ogni nodo ha un MTU di almeno 1280 byte.
##### Optional
# INDIRIZZI
Gli indirizzi sono assegnati in maniera gerarchica: dal global internet register gli indirizzo vengono assegnati ai vari regional internet registry, che a sua volta li assegna ai local internet registry e quest'ultimo li distribuisce a chi ne fa richiesta in un pool. In questa scala si parla di macroaree (non della regione toscana).
### Schema
Lo spazio degli indirizzi IPv6 è talmente grande che serve uno schema (RFC4291). Abbiamo 3 tipi di indirizzi:
- Unicast
	- Global
		È un indirizzo che identifica un host in modo univoco a livello globale.
	- Link-local
		Hanno validità sul link, cioè dall'host al primo router (se si passa da uno switch il link-local rimane valido). Le uniche eccezioni di quest validità sono le reti ad-hoc.
	- Site local
		Deprecati.
	- Unique Local (ULA)
		Indirizzi privati, ma non usabili da nessuno.
	- IPv4 compatibili
		Non interessanti.
	- IPv6 mapped
		Non interessanti.
- Multicast (one-to-many)
	Il pacchetto viene mandato a più host: il mittente ne invia uno a un multicast router, che avrà il compito di inviarne una copia a ogni destinatario.
- Anycast (one-to-nearest)
	È un indirizzo IP che corrisponde a tutti gli host di una rete o di una subnet. Si sta incaricando la rete di portare il pacchetto al prossimo router funzionante; in IPv4 quando si voleva ridondare un server gli si associava più indirizzi IP ed era l'host che doveva scegliere quale usare; adesso questa decisione viene presa dalla rete.
##### Prefissi
![prefix IPv6 address](prefix%20IPv6%20address.png)
- 2001:DB8::/32
	Prefisso usato nella sola documentazione. Per ogni esempio presente in letteratura, e solo in questo caso, si dovrebbe usare questo prefisso.
### Indirizzi dispositivo
Un indirizzo non ha un solo indirizzo IPv6, ma ne ha tanti:
- Loopback
	Anche se non ha una scheda di rete risponde sempre a questo indirizzo,
- Link local
	Solitamente un indirizzo link local.
- Global unicast
	Sono tanti, non uno solo.
- All-Nodes Multicast (FF02:1)
	È l'indirizzo multicast a cui devono rispondere tutti gli host della rete.
- All- Router Multicast
	È l'indirizzo a cui devono rispondere tutti i router della rete. È considerato un indirizzo semantico, cioè che evidenzia quale servizio è acceso su una macchina. Ci sono infatti anche indirizzi relativi ai router (FF02::2), e anche ad altri tipi di macchina (se vuoi vedi [registro](https://www.iana.org/assignments/ipv6-multicast-addresses/ipv6-multicast-addresses.xhtml)). È molto potente rispetto ad IPv4: questo doveva far passare il pacchetto fino al livello 4 e poi scartarlo nel caso non gli interessasse; in questo modo si può filtrare i pacchetti a livello 3 (direttamente nella scheda di rete).
- Solicitied-Node Multicast
	È l'indirizzo utilizzato per il DAD message, cioè nel momento dell'autoconfigurazione.
### Rappresentazione
Un indirizzo è composto da 128 bit. Si usa la notazione esadecimale, dove ogni 4 caratteri (16 bit, visto che un carattere in esadecimale rappresenta 4 bit) si ha ":".
Si può raggiungere una forma compatta ([esempio](compact%20form%20IPv6%20address.png)): all'inizio di ogni sequenza di 4 caratteri si possono eliminare gli 0 iniziali; si può eliminare la più lunga sequenza di 0.
### Creazione
Un indirizzo IP è composto da il suo prefisso, una prima parte e l'interface ID. Il metodo è più o meno sempre lo stesso: si utilizza un prefisso, si riempi in qualche meno i restanti primi 64 bit e si crea un Interface ID di 64 bit. Quello che può variare è la dimensione del prefisso: se è minore di 64 bit allora per l'interfaceID si può usare tecniche come il completamento EUI-64, altrimenti useremo tecnoche simili che restituiscono meno bit.
Ad esempio nel caso degli indirizzi link-loncal il prefisso è FE80 e i successivi bit, prima dell'interfaceID, per consuetudine (si può anche non fare) sono messi tutti a 0.
##### Interface ID
Per creare l'interface ID (64 bit) di un indirizzo IPv6 non ci sono regole stringenti, ma lo standard ci dice solo che deve essere ragionevolmente univoco. Può essere composto in molti modi:
- Serialmente
	Non è una buona idea perché molto probabilmente i primi che proveremo saranno già occupati.
- MAC
	È il metodo più comune perché solitamente l'indirizzo MAC è univoco.
	- A partire dall'indirizzo MAC a 64 bit.
	- A partire dall'indirizzo MAC a 46 bit (ethernet). In questo caso il metodo è detto EUI-64: è un metodo algoritmico che è spiegato nella documentazione.
- Usare l'[DHCP](IPv4.md#Dinamico) (per l'esame non c'è da capire come funziona).
	È una buona idea. La configurazione nel caso di DHCPv6 è più complicata di DHCPv4.
- Configurarlo manualmente.
- Generatore casuale
	Questo può essere un problema nel caso il generatore non sia molto buono.
- CGA (Cryptographically Generated Addres)
	Con questo modo si associa una parte della chiave pubblica della macchina. L'elaborazione nel router di questa chiave rende questa soluzione più lenta. Il prof non ha mai visto nessuna implementazione.
##### Auto configurazione
È il meccanismo con cui un nodo può ottenere il suo indirizzo link local e global unicast in autonomia.
- Si costruisce il proprio link-local address a partire dal MAC address della scheda di rete.
- Si fa il join all'indirizzo Solicited-Node Muticast (che viene fatto in modo automatico), costruito prendendo gli ultimi 24 bit dell'indirizzo link local ch sta cercando di acquisire. Si trasmette il messaggio DAD (Duplicate address detection) e si rileva se tale indirizzo costruito è unico.
	- DAD è un particolare pacchetto con cui si chiede a chiunque lo riceva di rispodnere solo nel caso in cui l'indirizzo scelto sia il proprio. Siccome ancora non abbiamo un indirizzo global, si usa come indirizzo mittente l'unspecified; non potendo rispondere direttamente al mittente, chi ha l'indirizzo uguale a quello proposto dal nodo risponde in modo multicast (chi riceve riceve al link local multicast address, FF02::1:3). Nel caso si riceva una risposta allora si riprova con un indirizzo diverso.
- Si usa il link local address ottenuto per chiede al router il Router Advertisement (RA). Questo contiene il PIO (Prefix Information Object) che indica il prefisso da usare nella costruzione del global address.
- Si costruisce il global address e si riesegue il meccanismo DAD.
	Per costruire il global address in modo privacy non si può usare l'EUI-64: in questo modo ogni volta che ci agganciamo a un router diverso si vedrebbe cambiare solo il prefix (primi 64 bit dell'indirizzo) e non l'intefaceID (che dipendendo dal MAC address sarebbe sempre lo stesso), e così facendo si potrebbe capire gli spostamenti del dispositivo. Quello che succede è comporttare l'interfaceID in modo: ogni lasso di tempo vengono generati una serie di interfaceID e quando si deve comporre l'indirizzo si sceglie uno tra questi.
- Si setta il defaultt router e si inizia la navigazione.

In questo meccanismo ci sono dei problemi nativi:
- Se un attaccante sulla link local risponde sempre al DAD allora non si arriverà mai ad avere un indirizzo link local e quindi non si otterrà mai un indirizzo global. Questo è un tipo di attacco link-local.
- Ci possono essere errori di trasmissione e quindi il messaggio di risposta in multicast al DAD potrebbe non essere ricevuto, arrivando così ad avere degli indirizzi non univoci. Questo è un problema comune nei casi in cui il messaggio di acknoledgment si mandi solo nelle situaizoni favorevoli e non sempre (in questo caso vuol dire che tutti i nodi dovrebbero dire "ok questo non è il mio indirizzo").
# ON-LINK
La differenza tra subnet di [IPv4](IPv4.md) e on-link di IPv6 è così importante che c'è il documento RFC5942 che la spiega.
Essere on-link e nella stessa subnet implica che due host possono parlarsi senza passare dal router, cioè possono comunicare a livello 2 (MAC), senza coinvolgere l'indirizzo IP.
### IPv4
- Due host nella stessa subnet sono considerati on-link
- Per identificare la subnet si guarda il prefisso dell'indirizzo IP.
- Se $A$ vuole capire se $B$ è nella stessa subnet allora: $A$ prende il suo IP e subnetmask e li mette in $and$; prende la sua subnetmasl e l'IP di $B$ e li mette in $and$; se i risultati sono uguali allora sono nella stessa subnet, altrimenti no.
### IPv6
- Non esiste la subnetmask.
- Due host che hanno lo stesso prefisso non sono necessariamente nella stessa subnet, cioè non per forza possono parlarsi senza passare dal router.
- Gli indirizzi link-local sono sempre on-link.
- Nello standard non c'è un modo per vedere se un indirizzo destinatario è on-link.
	- Quello che praticamente succede è che, a meno di conoscenze pregresse, tutti gli altri nodi sono considerati non on-link; quando un mittente vuole inviare qualcosa a un destinatario passa dal default router; questo, nel caso il destinatario sia on-link, lo dice al mittente tramite un Root Regret (RR) che aggiorna la propria tabella dei vicini.
	- Questo concetto è valido non solo con indirizzi singoli ma anche con interi prefissi. In questo caso il mittente sa che tutti gli indirizzi che gli sono comunicati tramite il prefisso sono on-link e sono raggiungibili da un determinato indirizzo (che sarà probabilmente un sottorouter o un gateway a cui inviare).
	- In questo scenario un attaccante può fingersi il default router e farmi dirigere tutti i pacchetti ad un indirizzo prefissato. Questo attacco (link-local) può essere usato per tracciare il traffico oppure per negare l'accesso ad un sito.
# THREAT
Gli attacchi ad IPv6 sono IP-leyer e upper-layer.
I secondi non sono di nostro interesse (per il corso di network security), ma dovremmo fare sempre il possibile per evitarli; se ad esempio usiamo le socket e non consideriamo bene $socketaddr$ (controllando la lunghezza dell'indirizzo $addrlen$) differenziandolo tra IPv4 e IPv6 allora potremmo avere dei problemi.
Questo porta all'idea che se abbiamo un applicazione che funziona corretttamente e in modo sicuro usando IPv4, se la portiamo in IPv6 potrebbe essere un disastro.
### Network Admin
Il problema principale dell'IPv6 nasce dal network administrator. Infatti le persone tendono o a usare IPv6 come IPv4 oppure a forzale le funzionalità di IPv6 per realizzare freatures di IPv4: un esempio è la gestione di hosto on-link onsiderando i prefissi (quello che in IPv4 sono le subnet).