# PRINCIPIO DI KERCKHOFFS
Ci dice che non si deve richiedere che il design di un sistma sia segreto perché la sua sicurezza sia alta.
Questo perché il design di un sistema non può essere cambiato in maniera sicura, semplice e dinamica. Un'obiezione a questo potrebbero essere i container, ma in questo caso il design del sistema è docker (p il runnable di container usato).
Il design deve essere considerato pubblico e lo si deve proteggere con questa assuzione (che non vuol dire dirlo a tutti).
# SLA
Quando si delega un servizio lo si deve vincolare da un contratto, il Service Level Agreement (SLA). Uno SLA definisce il minimo (non il servizio medio) che ci possiamo aspettare da un servizio.
### Entità
Nel nostro mondo, le tre entità che cono coinvolte sono:
- Service provider
	Vuole vendere un servizio al cliente. Utilizza la rete per raggiungere il cliente; non gli interessa la parte di sicurezza.
- Network operator
	Il suo compito è quello di costruire mantenere e gestire la rete. Non ha il compito di bloccare un attaccante; l'unica cosa che deve fare è garantire la minima QoS dichiarata.
- Customer
	Usa il servizio e non gli interssa come la rete funziona.
### Contratto
![SLA](SLA.png)
Il problema con questa foto è che il customer utilizza il servizio prodotto dal service provider e distribuito tramite il netwrok provider; il contratto è tra customer e service operator, e non c'è tra customer e network operator.
Questo è una conseguenza del modello su cui si basa internet; è una cosa buona per il cliente che può negoziare con più service provider (e quindi ottenere un prezzo più basso per via della concorrenza), ma negativo per la QoS (nel caso ci serva un'alta qualità del servizio ci server che ci sia uno SLA tra service e network provider (come tra DAZN e Telecom)) e per la security.
# ATTACCANTI
Il temine hacker deriva dagli anni '60 per identificare qualcuno appassionato di tecnologia che per capirla bene la smontava; la connotazione negativa viene dalle compagnie telefoniche, alle quali gli hacker smontavano le cabine telefoniche per capire come funzionava la telefonia e poi per fare chiamare gratis.
Oggi con il termine hacker si identifica quelle persone che utilizzano le loro skill per risolvere un problema (molto generico).
### Categorie
Oggi gli attaccanti sono categorizzati più a grana fine in base.
- I primi tre sono i più bravi tecnicamente. Non utilizzano attacchi noti in letteratura, ma ne creano di nuovi.
##### White hat
Aderiscono al concetto di hetical hacking (migliorare le capacità di un sistema): trovano problemi nei sistemi e poi dicono al proprietario come migliorarli. Sono freelace che devono essere pagari (solitamente molto).
##### Black hat
Stessi concetti del white hat, ma il loro obiettivo è quello di fare danni. Sono pagati molto e da qualcuno che vuole rompere un sistema.
##### Grey hat
Stanno in mezzo tra white e black.
##### Scrpt kiddie
Non hanno le conoscenze tecniche per creare nuovi vettori di attacco, ma utilizzano quelli presenti in letteratura.
Il problema è che attaccano senza sapere cosa fanno: gli attaccanti delle prime 3 categorie seguono un percorso logico (che possiamo seguire o che può depistarci); loro fanno danni senza una logica.
Siccome non utilizzano vettori di attacco nuovi se veniamo atttaccati da questi allora non abbiamo fatto bene il nostro lavoro, perchè costro i vettori noti dovremmo prottegrci.
##### Blue hat
Fanno parte del security team di un'azienda. Si dividono in read team e blue team: i primi sono attaccanti, i secondi devono difendere.
##### Hacktivist
Sono mossi da obiettivi nobili (es: giustizia, religione, politica) e per questo attaccano coloro che vanno contro questi obiettivi.
Rispetto alle conoscenze possono essere white/black oppure script kiddie.
Spesso non pensano alle conseguenze delle loro azioni: Anonimous ah cancellato tutti i materia pedo pornografici (e questo è buon), ma anche anni di prove della polizia che non ha potuto mettere in carcere i colpevoli.
##### Nation state
Sono nazioni o qualcuno che è stato convenzionato da queste.
Sono i peggiori, perché possono mettere sul campo risorse (es: capacità computazionali, anni/uomo di sviluppo) out of the shelf con cui nessuno può competere.
# VULNERABILITÁ
Gli attaccanti entrano in un sistema grazie alle vulnerabilità.
In un sistema ci sono sempre vulnerabilità per un motivo di costo: realizzare un sistema sicuro al 100% sarebbe troppo costoso in termini di tempo e soldi. Si dice che un programma ha almeno un bug e un'istruzione ridondante; per induzione allora un programma può essere ricondotto a una sola istruzione sbagliata.
### By design
Le vulnerabilità sono ben conosciute, documentate (quando si verifica e con quale vettore di attacco) e accettate.
Sono vulnerabilità che sono state lasciate e non risolte perché si è valutato che senza il sistema non avrebbe rispettato un requisito.
Ci si protegge leggendo il manuale ed evitando le situazione documentate.
- Esempi
	- NAT
	- ARP
		Ad esempio si poteva fare un secure ARP, ma in questo modo un device non può ricevere informazioni dalla rete.
	- Resource discovery protocol
### By bad design
Sono le vulnerabilità inserite senza saperlo, cioè il designer ha fatto un errore in fase di progettazione.
Probabilmente non possono essere corrette tornando indietro, e quindi il miglior modo di risovlere è riniziare da capo (o quasi).
- Esempi
	- SMB, cioè il file sharing di windows.
	- Speculative execution attack
		Nelle CPU multicore vengono eseguiti tutti i rami di un brunch prima di sapere quale percorso prendere. Non è rimuovibile se ci serve la parallelizzazione; è rimovibile negli altri casi ma con una perdita del 30/40% di delle prestazioni.
	- Valutazione delle eccezioni
		Si intende la gestione di comportamenti che non sono stati predetti: un pacchetto arriva, ma non è quello che si aspettava, o è duplicato, o in ordine invertito.
### By bad implementation
Sono i bug inseriti nel sistema a livello di codice.
Delle volte alcuni errori non sono gestiti volontariamente. Ad esempio se un sistema va in crush, potrebbe essere più costoso capire cosa è successo e ripristinare il sistema rispetto a far si che vada in crush e riaprire l'applicazione.
- Esempi
	- Annidamenti sbagliato di if/else/then.
	- Copia di una parte di memoria sbagliata (20 bit messi in 10 bit).
	- Non validare un errore.
##### Trovare el vulnerabilità
Un bug è una vulnerabilità; dobbiamo quindi usare tool per trovare bug nel nostro codice.
- I bug sono pagati
	I black hat, trovano bug per soldi per conto di qualuno che vuole fare dei danni; ii white hat trovano bug per conto nostro e sono pagati da noi.
- Ci sono dei database con i bug più comuni:
	- [CVE](Common Vulnerabilities and Exposures) (Common Vulnerabilities and Exposures)
		Elenca i singoli bug rilevati.
		Se osserviamo un aggiornamento sw questo indica sempre il CVE (sotto forma di CVE-xxx, dove xxx è l'anno e il numero); questo permette di capire quali vulnerabilità sono state aggiornate e se nel caso non aggiorni il sistema a che tipo di attacchi mi espongo.
		Questo è importante perché non sempre è obbligatorio aggiornare un sistema: un vecchio sistema avrà sicuramente delle vulnerabilità, ma queste saranno note e ben mitigate; un sistema aggiornato avrà sicuramente delle vulnerabilità, ma saranno non note; a volte è meglio mitigare bene le vulnerabilità note che non considerare proprio quelle sconosciute.
		Quando si osserva una vulnerabilità dall'elenco delle CVE ci sono due cose importanti: il base score indica quanto la vulnerabilità è dannosa in generale; la stringa vector identifica l'impostazione dei parametri per ottenere il base score indicato. È importante notare che i parametri di questa metrica sono impostati da chi la scopre e quindi spesso soggettivi; tramite il calcolatore sul sito di [first](https://www.first.org/cvss/) si può adattare i parametri sulla base della nostra situazione. Inoltre si possono definire dei parametri che descrivono più approfonditamente il bug di tipo temporale, che definiscono quanto è fondamentale risovere il bug adesso.
	- [CWE](Common Weakness Enumeration) (Common Weakness Enumeration)
		Sono i bug in base alla loro categoria. Quando viene scoperto un bug viene categorizzato (es: memory free, input validation) e il contatore della sua categoria viene incrementato. Questo è un ottimo modo per capire quali sono i test che sicuramente dobbiamo fare nel nostro codice: i bug più comuni nel codice non dovrebbero esserci.
### By bad deployment
In questi casi il sistema funzionava benissimo in ambiente di test, ma una volta sul mercato non fa quello che deve fare.
Possibili motivi sono che abbiamo installato il sistema nel modo sbagliato oppure in un ambiente per cui non era stato predisposto.
### By update
Sono le situazioni in cui il problema non è direttamente nel nostro sistema, ma nelle parti che abbiamo prese dall'esterno (librerie, pacchetti o parti dell'OS).
Qudno si utilizzano pezzi di codice o di sistema che sono presi da altri allora si deve fare la validazione, cioè assicurarsi che siano corretti.
Per risovere questo problema se si usa python si devono usare ambieti virtuali; se non si usa python allora si devono usare i contanier. Questo vuol dire mettere nel nostro sistema librerie statiche, cioè scaricate e che non si possono aggiornare da sole: quando aggirono una libreria voglio farlo io e voglio controllare che il tutto sia ancora sicuro.
- Esempi
	- Funzioni o membri deprecati e comunque usati.
	- Nuovi bug in librerie esterne usate.