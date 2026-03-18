Il NAT (Network Address Trabslation) è un meccanismo di [IPv4](IPv4.md) e nasce per risolvere il problema della saturazione degli indirizzi: l'idea è quella di associare a più indirizzi interni a una subnet un solo indirizzo esterno pubblico e globale.
Col il NAT non si assume che gli indirizzi interni siano univoci a livello globale ma solo a livello di subnet.
Lo spazio degli [indirizzi interni](NAT%20private%20address.png) del NAT è limitato a tre specifici intervalli in relazione alle classi.
### Problema
Il problema principale del NAT è che si suppone, e quindi ci si comporta, come se faccia cose per cui in realtà non è stato pensato, come la sicurezza. Quando qualcuno cerca di contattare per la prima volta un nodo interno, e quindi non c'è una corrispondenza all'interno della tabella, allora il pacchetto viene scartato. Questo è una comportamento di costruzione del NAT ma non è stato pensato per la security.
# TIPOLOGIE
Il NAT è un meccanismo stateful, deve quindi mantenere uno stato in ogni momento.
### NAT statico
La tabella di mapping tra indirizzi privati e pubblici è 1:1 ed è fatto staticamente.
Ha un utilizzo molto limitato e non aiuta a risolvere la saturazione degli indirizzi.
### NAT dinamico
La tabella di mapping tra indirizzi privati e pubblici è fatta dinamicamente, cioè si aggiunge una entry solo quando un host vuole comunicare con internet e solo finchèè necessaria.
Se si assume che non tutti gli host vogliano comunicare con internet contemporaneamente allora aiuta con il problema della saturazione; nel caso pessimo però si dovremmo avere tanti indirizzi esterni quanti sono gli host interni.
### NAPT
È il Network Address and Port Translation (con NAT ci si riferisce a questo): non è coinvolto solo il livello 3 (indirizzi IP) ma il mapping coinvolge anche il livello 4 tramite il concetto di porta.
Non si considera più la relazione tra indirizzi interni ed esterni, ma tra flusso di dati (es: un tab aperto di chrome, una app aperta che usa internet) e indirizzi esterni. Tramite la coppia {IP, porta} si riesce a mappare molti indirizzi interni con un unico indirizzo esterno.
È un meccanismo complesso perchè, oltre a fare il mapping, si deve riscrivere il checksum sia del livello 3 che del livello 4.
# TRASLAZIONE
La traslazione degli indirizzi si basa su due concetti:
- Binding
	Gestisce la traslazione dei pacchetti tramite la trasforma della tuple {IP, protocol, port} da interno a esterno.
- Filter
	Decide se un pacchetto deve essere traslato, indipendentemente se il binding esista o no (cioè anche se c'è la corrispondenza nella tabella). Questo filtro rende il comportamento del NAT non deterministico, inteso nel senso che non si può sapere quando ha un comportamento oppure un altro.
### Complessità
Il motivo per cui il comportamento del NAT è così complicato è che coinvolge anche il livello 4, e TCP e UDP sono diversi.
- TCP
	È stateful: c'è un flusso di dati tra due endpoint e la coppia {binding, filter} può essere tenuta finché il flusso è aperto. Si sposa bene con il meccanismo del NAT perché abbiamo detto che esso è stataful. Il NAT in TCP funziona sempre in modalità [simmetrica](NAT.md#Simmetric).
- UDP
	È stateless: non c'è un flusso di dati continuo tra due endpoint. È un meccanismo connectionless, e la coppia {binding, filter} non può essere tenuta per l'interno flusso di dati (perchè non c'è). Però per come è stato pensato il NAT deve essere fatto: questo è il motivo per cui oltre al biding c'è il filter.
### UPD NAT
Stiamo elencando le tipologie di tr che si possono fare.
Da qui in poi prendiamo come esempio il referral & handover e vedremo quando funziona e quando no; è il meccanismo UDP per cui più host vogliono comunicare tramite un app di messaggistica UDP (es: discrod, playstation).
##### Symmetric
Funziona esattamente come il NAT per TCP: solamente il destinatario può rispondere se usa il suo indirizzo e la sua porta (quella segnata sulla tabella NAT). [Esempio](simmetric%20NAT.png).
Il referral & handover non funziona, almeno che il destinatario (B porta 90 nell'esempio) non sia un server che prende i nostri messaggi e li inoltra al destinario del messaggio; siamo però in una situazione molto centralizzata.
##### Full Cone
Il filter non fa nessun tipo di lavoro: conoscendo indirizzo e porta esterni del NAT tutti possono rispondere da qualunque porta. [Esempio](Full%20Cone%20NAT.png).
Il referral & handover funziona ma è troppo permissivo. Un attaccante deve solo conoscere l'indirizzo esterno del NAT e provare a rispondere a qualunque porta del NAT senza neanche fingersi qualcun'altro.
##### Restricted Cone
Il filter lavora sulla base dell'indirizzo IP: conoscendo indirizzo e porta esterni del NAT può rispondere un solo host da qualunque porta. [Esempio](Restricted%20Cone%20NAT.png). Il referral & handover non funziona, almeno che il destinatario (B nell'esempio) non sia un server che prende i nostri messaggi e li inoltra al destinario del messaggio; siamo però in una situazione molto centralizzata.
##### Port Restricted Cone
Il filter lavora sulla base della porta: conoscendo indirizzo e porta esterni del NAT tutti gli host possono rispondere però solo da una determinata porta. [Esempio](Restricted%20Cone%20NAT.png).
Il referral & handover funziona, con la sola restrizione che il destinatario risponda dalla porta giusta. Lato security è ok perché l'attaccante dovrebbe conoscere l'indirizzo esterno del NAT e variare sia la porta del NAT che la propria.
# CLASSIFICAZIONE
Avere una classificazione dei NAT è molto difficile e ci interessa il giusto. Quello che dobbiamo sapere è che un host non sarà più identificato con il suo IP pubblico (apparentemente pubblico, cioè quello che lui vede e sente come suo IP) e con la sua porta, perchè il farà un traslazione. Inoltre abbiamo che un host può essere identificato da più IP e più porte.
Un host che non ha ancora comunicato con me può comunicare. Il modo in cui lo può fare è dato dal tipo di NAT e dal tipo della cascata di NAT.
### STUN
Sarebbe fattibile se tra source e dest ci fosse un solo NAT, ma quasi mai questo succede. In questa situazione possiamo usare il metodo STUN (Session Traversal Utility for NAT). È un protocollo che si basa su domanda e risposta e cerca di minimizzare il loro numero. Si basa sul modello clinet/server: un dispositivo, che deve avere un clint STUN, che vuole sapere che NAT lo espone al mondo invia un messaggio al server STUN. [Lavorando](STUN.jpeg) per esclusione sull'indirizzo IP e numero di porta si riesce a capire il tipo di NAT.
##### Esempio
Vediamo un [esempio](STUN%20problem%20example.jpeg) dove non è possibile capire il tipo di NAT.
Se abbiamo un NAT con due indirizzi esterni ci sono dei casi dove non è possibile apparire all'esterno con lo stesso indirizzo; l'esempio che vediamo è comune negli internet service provider. L'indirizzo esterno del nostro NAT sarà interno per un internet service provider (ISP) (che è un autonomous system), il quale di sicuro avrà un indirizzo pubblico a livello globale. Supponiamo che il mio IPS abbia un accordo con altri due IPS (autonomous system); allora la direzione del mio traffico (cioè, da che parte va il mio pacchetto?) Dipende dal routing, cioè della destinazione; ottengo quindi un indirizzo IP diverso a seconda di dove vado a partire dall'esterno dell'ISP. In questo modo i server di destinazione mi vedono in modo diverso.
Il problema in questo caso non è di raggiungibilità ma di indirizzo IP: io sono comunque raggiungibile ma vengo visto con due indirizzi diversi.
### Binding
Per l'endpoint si può classificare il NAT in questo modo.
##### Endpoint independent
Funziona come il Full Cone. Il NAT usa lo stesso binding per tutti i pacchetti con lo stesso IP/porta; l'IP/porta del destinatario non sono considerati.
##### Endpoint address dependent
Funziona come il Restricted Cone. Il NAT usa lo stesso binding per tutti i pacchetti con lo stesso IP/porta del mittente e lo stesso IP del destinatario; la porta del destinatario non è considerata.
##### Endpoint port dependent
Funziona come il Port Restricted. Il NAT usa lo stesso binding per tutti i pacchetti con lo stesso IP/porta del mittente e la stessa porta del destinatario; l'IP del destinatario non è considerata.
##### Endpoint address and port dependent
Funziona come il Symmetrci. Il NAT usa lo stesso binding per IP/porta del mittente e del destinatario.
### Port binding (19:15)
Per il binding della porta si può classificare il NAT in questo modo. Questo è il motivo che rende il NAT non deterministico, cioè che non si può prevedere il suo comportamento, nel senso che il cuo comportamento dipende da aspetti che non possiamo controllare.
##### Port preservation
Il NAT proverà a non cambiare la porta. Se due host usano la stessa porta allora al primo non sarà cambiata e al secondo si; il problema è che se l'applicazione richiede che il numero di porta sia quello specificato allora al secondo host non funzionerà.
##### Port overloading
Il NAT tenderà a mantenere inviolata la porta per l'ultimo arrivato. Se due host che usano la stessa porta arrivano in seguenza quello che succede è: al primo non viene cambiata la porta; quando arriva il secondo viene cambiata la porta al primo e al secondo viene mantenuta la sua. Il problema è che se l'applicazione richiede che il numero di porta sia quello specificato allora solo il secondo sarà in grado di utilizzarla; questo provoca un apri e chiudi dell'applicazione.
##### Port multiplexing
Il NAT cerca di fare multiplexing sulla stessa porta in base all'indirizzo IP/porta del destinatario. Il problema si ha se più host hanno un flusso con lo stesso destinatario, perchè il NAT deve capire quando fare multiplexing e quando non farlo.
Con questa soluzione il NAT utilizza molta CPU e memoria. Solitamente un NAT viene utilizzato in port multiplexing e quando è a fine risorse va in port preservation; questo dipende da quante risorse ha e da quanti flussi arrivano (cose che non possiamo controllare) ed è il motivo per cui il NAT è non deterministico.
# TIME REFRESH
Per decidere quando far scadere il binding e il filter nell'UDP devo guardare se ci sono ancora flussi in entrata e/o in uscita. Questo concetto è un problema per la security.
Vediamo vari tipi di time refreshing e le loro problematiche. Tutte le soluzioni (tranne l'outbound) hanno il problema degli attacchi DoS: se un attaccante si finge uno dei due endpoint e continua a mandare pacchetti, allora il NAT continua a mantenere attivo il binding e si può avere un esaurimento di risorse (se tutti i binding sono aperti e ne arrivano altri).
### Bidirectional
Il timer viene aggiornato quando passano pacchetti sia in ingresso che in uscita.
### Outbound
Gli unici pacchetti che aggiornano il timer sono quelli in uscita. A seconda dell'applicazione, per esempio in caso di applicazioni che richiedono un traffico solo in entrata (es: Netflix), potrebbe essere necessario un keep-alive; lato security questo non ci piace perché stiamo dicendo all'esterno che stiamo effettivamente parlando con un qualche server anche se non ce ne sarebbe bisogno.
### Inbound
Gli unici pacchetti che aggiornano il timer sono quelli in entrata. A seconda dell'applicazione, per esempio in caso di applicazioni che richiedono un traffico solo in uscita, potrebbe essere necessario un keep-alive.
### Transport protocol statre
Teoricamente è il migliore: decodifica l'application layer per cercare di capire quello che sta succedendo e fa il refresh del time nel modo corretto. Ci sono però vari problemi:
- Il NAT sa esattamente cosa l'host sta facendo, cioè conosce il tipo di protocollo utilizzato, il traffico in entrata e quello in uscita.
- Il modo in cui decodificare l'application layer dipende dal sw che sta utilizzando. Questo ha un costo molto elevato perché spesso richiede di fare reversal engineerign e procedere per prove. Inoltre i sw che riescono a fare questo sono molto complessi, e più un sw è complesso più è esposto a vulnearabilità; consiedriamo che questo sw sta sul NAT e quindi lo rende vulnerabile (anche se dovrebbe essere in realtà una macchina molto semplice).
# FRAMMENTAZIONE
Un'altro problema del NAT è la gestione della frammentazione. In teoria non dovrebbe ricostruire i pacchetti, in quanto non è un endpoint; spesso però la stessa macchina che fa da NAT fa anche da firewall e questo ha la necessità di ricostruirli.
In questo contesto se il primo pacchetto, quello che contiene il frammento, non è il primo ad arrivare si potrebbe avere un attacco DoS, mandando tanti pacchetti senza far mai arrivare il primo; il NAT memorizzerà tutti i pacchetti finchè la sua memoria non è piena, e inizierà allora a scartarli.