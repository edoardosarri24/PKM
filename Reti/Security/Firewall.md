# DEFINIZIONE
La maggior parte delle persone pensano che un firewall sia un dispositivo che non pemette agli attaccanti di entrare nella rete. In questo modo stiamo definisce un firewall sulla base di quello che fa e non di cosa esso rappresenta per la sicurezza.
Un firewall è quindi un dispositivo che server per implementare una misura di sicurezza sulla base di un'analisi del rischio: prima si definiscono gli asset; si separano in base ai loro requirements di sicurezza, ai rischi e alla fiducia (es: utenti guest e utenti autenticati godono di fiducia diversa), creando così un insieme di asset che hanno senso di stare insieme (area di sicurezza); si separa a questo punto le parti di rete che contengono asset diversi; i firewall sono esattamente il punto di connessione di rete che sono composti da asset diversi.
Questo per la rete interna; c'è una parte di rete su cui però non abbiamo controllo, cioè internet. Tramite un firwall possiamo isolare un pezzo di internet (la nostra rete intranet) ed esercitare le policy di sicurezza adeguate.
##### Osservazione
Se il computer dell'azienda viene portato al di fuori del firewall e collegato a una rete che non avrà gli stessi livelli di sicurezza, allora questo dispositivo è da considerarsi violato e periocolo per la nostra rete.
# PRINCIPI DI CONFIGURAZIONE
I firewall vanno configurati secondo due principi:
- Least privilege
	È il principio per cui si dovrebbe permette solo il traffico indispensabile per quella determinata area di sicurezza. Questo però porta a due osservazioni:
	- Nel caso di una coesione M2M (Macchine2Macchine) allora è fattibile e andrebbe fatto esattamente così; non serve che un dispositivo IoT vado su Wikipedia.
	- Nel caso di una connessione con un umano questo non è possibile: ci ritroveremmo o persone che ci vengono a cercare, oppure che si fanno da hotspot con il telefono e quindi bypassano il firewall.
- Separation of duties
	È il principio per cui un'entità deve fare solo quello per cui è stata pensata e non deve fare altro; vuol dire che il firewall deve fare il firewall e non altri servizi.
	Ci sono dei casi in cui questo principio ha senso di essere violato: il NAT (dopo vediamo perché).
### Numero
Se abbiamo un singolo firewall allora questo diventa un single ponit of failure sulla sicurezza della rete; non si parla di single point of failure per il funzionamento perché se la rete è suddivisa in più [livelli](firewall%20idea.png) e si rompe un firewall allora è bene che l'intera rete non sia più accedibile.
In questo caso a un attaccante basta violare un firewall per accedere a più aree di sicurezza della nostra rete.
Quello che vorremmo è avere più firewall e di tipi diversi: così l'attaccante non può penetrare due firewall con la stessa tecnica. Questo comporta due problemi: il costo di avere apparecchiature diverse; la conoscenza che l'amministratore della rete deve avere.
Tranne nel caso in cui abbiamo un profilo di sicurezza molto alto non serve più di un firewall (al massimo 2).
### Demilitarizer Zone (DMZ)
Solitamente la DMZ contiene una serie di servizi che devono essere contattabili dal resto di Internet; ad esempio ci potrebbe essere un centralino VoIP (che deve riceve le chiamate entranti) o il server mail.
Questa zona non deve essere nell'intranet, ma neanche totalmente esposte all'esterno; si posizionano nel mezzo in modo che sia coperta da firewall.
L'intranet può parlare con la DMZ e con internet (poco); internet può parlare poco con la DMZ e non parla con l'intranet.
# TIPI
Ci sono tre tipi di firewall:
- Stateless
	Decide se un pacchetto debba essere fatto passare in base all’analisi del suo contenuto; analizza quindi sia il livello 3 che i livello 4. L'analisi del livello 4 è abbastanza inutile perché, non avendo memoria, non riesce a collegare i pacchetti dello stesso flusso informativo oppure pacchetti IPv4 che hanno subito la frammentazione.
- Statefull
	Esegue l'analisi degli stessi livelli del precedente, ma avendo memoria riesce a fare una valutazione più adeguata dei pacchetti passasti: riesce a collegarli a un flusso o a gestire la frammentazione.
	È il tipo di firewall che si usa. Il più famoso è il "packet filter", che è quello che usa MacOS.
- L7 Firewall
	Analizza livello 3, 4 e 7, cioè analizza anche il contenuto di una richiesta del livello applicazione (più altro in TCP/IP e 7 in ISO/OSI) (es: capisce se una post http contiene un JavaScript malevolo).
	Per implementare questo, il firewall deve fare una packet inspection, cioè un man in the middle in modo legalizzato; per fare questo deve decifrare i messaggi, eseguendo un man in the middle legalizato. Questo può essere fatto se il firewall conosce gli ultii protocolli di crittografia, e questo porta a due problemi: i continui aggiornamenti di un firewall si pagano molto cari (solitamente come abbonamento); l'implementazione è una sorta di reverse engineering, che comporta codice molto elaborato, quindi soggetto a bug e quindi soggetto a vulnerabilità.
# NETFILTER PACKET TRAVERSAL
È il processo che gestisce i pacchetti di rete nel kernel Linux; il firewall che utilizza Linux nel suo insieme è detto iptables. È un firewall di tipo fullstate.
![netfilter](netfilter.png)
Il pacchetto entra nel firewall dalla rete: o viene forwardato a un altra rete o deve entrare nella rete; questa parte viene fatta dal routing.
Se è stato forwardato va fuori a un altro routing.
##### Conntrack
Fa il tracking (pallini grigi) della connessione, cioè ci dice se un pacchetto assomiglia a qualcosa che è già passato, in passto. Per fare questa operazione un firewall deve esser di tipo statefull.
Il [NAT](NAT.md) fa esattamente la stessa cosa, cioè tiene traccia del traffico passato; questo è il motivo per cui firewall e NAT hanno senso di stare insieme nella stessa macchina.
##### Mangle
Tabella (pannini senape) che permette di cambiare il contenuto dei pacchetti (es: time to live); non cambia mai indirizzo e posta sorgente e destinazione.
##### Nat
È la parte di [NAT](NAT.md) (pallini blu), cioè quella che fa il cambio di indirizzi e porte.
##### Filter
L’ispezione (pallini rossi) del pacchetto avviene o nel forward o nell’input o nell’output; non viene fatto in altri posti perché non c’è bisogno.
La scelta di dove farlo non è vincolata: Netfliter fa così ma ci sono altri firewall che fanno in altri modi.
# CLASSIFICAZIONE DEI PACCHETTI
Un pacchetto nell'iptables (firewall di Linux) può avere vari stati:
- NEW
	Un pacchetto è in stato NEW se è il primo di una nuova connessione; vuol dire anche che il pacchetto non appartiene a nessuna connessione esistente e che il traffico al momento è solo mono direzionale.
	Un esempio è il pacchetto di SYN nella connessione TCP.
- ESTABLISHED
	Un pacchetto è in stato ESTABLISHED se appartiene a una connessione già esistente; vuol dire anche che il traffico bidirezionale è già stato avviato.
	Un esempio è un pacchetto di risposta in una connessione TCP (come ACK).
- RELATED
	Sono pacchetti relativi a flussi correlati a una connessione ESTABLISHED; vuol dire che sono pacchetti che sono stati marcati da una regola di Conntrack, cioè che sono stati riconosciuti.
- INVALID
	Sono pacchetti di errore. In qusto caso è bene scartarli.
### Regole
Scrivere regole per gestire i pacchetti che si trovano in questi stati non facile: non si dovrebbero mai scrivere a mano (e ancora peggio usare le regole di altri), cioè senza il supporto di un tool.
Il modo ideale per farlo è scrivere le regole a livello funzionale (indicando quindi come comportarsi a seconda del protocollo) e poi farsi generale le regole vere e proprie da inserire come comportamento del firewall dal tool stesso. Ad esempio non dobbiamo dire blocca la porta 80 (quella dell’http) ma dovremmo dire direttamente blocca l’http.
# RESILIECY
Il firewall é un single point un failure: se il firewall si blocca allora la rete interna deve smettere di comunicare con l'esterno.
Per questo motivo il firewall deve essere ridondando. Ci sono varie tecniche.
##### Cold swap
Abbiamo una macchina identica alla prima, perfettamente configurata. ma spenta.
- Il vantaggio sta nelle parti hw della macchina (es:  ventole, hard disk), che si suppone non abbiamo difetti.
- Lo svantaggio é che dal momento in cui si accende a quando questa è in funzione passa tanto. Per minimizzare i tempi di reazione della rete al cambio di macchina si può pensare di avere gli stessi indirizzi MAC su entrambe le macchine.
##### Hot swap
Abbiamo due macchine identiche contemporaneamente accese, ma solo una è in funzione.
- Il vantaggio sta nei tempi di reazione al campio macchina, che sono molto bassi.
- Lo svantaggio è che abbiamo un consumo di risorse doppie rispetto ad avere una sola macchina. Non è infatti una buona soluzione a meno che non si abbiia la nessità di tempi di swap bassi.
##### Ridondanza fisica
Non solo le due (o più) macchine sono accese ma sono anche in uso. Davanti alle $n$ macchine firewall è presente un'altra macchina detta load balancer; quest'ultima è dingle point of failuer, ma essendo composta da hw molto semplice sostituirlo è banale e veloce.
- Il vantaggio è che ogni macchina ha un carico solo dell'$1/n$; se una si rompe allora le prestazioni solo di $1/n$.
- Lo svantaggio è che se una macchina si rompe allora i flussi che stavano passando da quella sono persi. Per non perdere i flussi si deve fare una sincronizzazione delle contrack e per farlo si può fare una sincronizzazione basata su evento o periodica;  non si può sapere a priori qual'è meno oneroso perché non si può sapere quanti eventi ho avuto in due periodici update consecutivi.