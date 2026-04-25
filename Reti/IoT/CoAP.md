Il Constrained Application Protocol (CoAP) è progettato da IETF Constrained RESTful Environments (CoRE). È un [application layer](IoT%20application%20Layer.md#Application%20layer) basato sul modello clinet-server usato all'interno delle reti [LoWPANs](LoWPANs.md), usato ad esempio in ambito IoT.
# CARATTERISTICHE
- Comunicazione
	Si basa sul modello client/server con una comunicazione asincrona di tipo request/response: le domande e le risposte devono essere collegate (tramite il campo token dell'header).
	Si basa quindi sul protocollo [REST](REST.md): i [CoAP URI](CoAP%20URI.png) identificano le risorse dell'applicazione e le loro rappresentazioni sono scambiate tra client e server. Le operazioni fornite sono le operazioni CRUD di REST: POST, GET, PUT, DELETE.
- È un sottoinsieme e una semplificazione d HTTP. Per questo motivo è garantita la massima interoperabilità con client che non supportano questo protocollo. A differenza d HTTP, CoAP si basa sull'analisi del formato binario invece che sul testo.
- È costruito su UDP. La parte di cifratura e scambio delle chiavi è fatta dal livello DTLS, Datagram Transport Layer Security.
- Permette lo scambio di messaggi asincroni, l'osservazione delle risorse (dove una request prevede più response; è come mettere un flusso su udp), la comunicazione multicast.
# HEADER
Un messaggio CoAP è formato da un header e un payload opzionale (es: messaggio ACK non serve).
![Header CoAP](Header%20CoAP.png)
##### Version (2 bit)
Indica la versione del protocollo utilizzato. Deve essere per forza 01.
##### Type (2 bit)
Indica il tipo di messaggio. In ordine da 0 a 3 abbiamo: CON, NON, ACK e RST.
##### Token length (4 bit)
Indica la lunghezza del campo token. Il valore 0 indica che il campo non è presente; i valori da 9 a 15 sono riservati.
##### Code (8 bit)
Descrive il messaggio. È composto da 3 bit di classe e 5 di significato, dove i primi 3 bit sono separati da un punto rispetto ai secondi.
- Classe 0 (000) indica una richiesta.
- Classe 2 (010) indica una risposta con successo.
	- 2.01 creazione
	- 2.02 cancellazione
	- 2.04 cambiamento
	- 2.05 contenuto
- Classe 4 (100) indica un risposta con un errore del client, cioè che se la domanda non viene cambiata allora la risposta sarà la stessa.
	- 4.00 bad request
	- 4.02 bad option
	- 4.04 not found
	- 4.05 metodo non permesso.
- Classe 5 (101) indica un risposta con un errore del server, cioè il client può rifare la richiesta, ma una risposta corretta dipende dal motivo dell'errore.
##### MessageID (16 bit)
Identificativo del messaggio per rilevare i duplicati.
##### Token (TKL bit)
È un campo opzionale e la sua lunghezza in byte è indicata nel campo TKL. Serve per la corrispondenza tra richieste e risposte.
##### Option
Sono ulteriori informazioni opzionali che possono essere inserite nell'header.
# AFFIDABILITÁ
Per realizzare l'affidabilità ci sono quattro tipi di messaggi:
- CON (confirmable)
	È il tipo di messaggio che permette di realizzare l'affidabilità.
	Ad una request CON ci si aspetta una response ACK per confermare la corretta ricezione; nel caso non si riceva una response ACK prima della scadenza del timer (fatto partire quando parte il messaggio CON) allora il messaggio viene inviato nuovamente con un back-off esponenziale (il timer viene raddoppiato a ogni nuovo invio).
	Le [risposte CON](CON%20response%20in%20CoAP.png) possono essere:
	- Piggy-backed
		I dati della risposta sono all'interno del messaggio ACK di conferma.
	- Separate
		I dati della risposta e la conferma arrivano in due momenti diversi: prima il messaggio ACK; quando i dati sono disponibili allora vengono inviati in un pacchetto CON (al quale si deve rispondere con un ACK).
- NON (non-confirmable)
	Non ci si aspetta una response ACK.
- ACK (acknowledgment)
	Per confermare la ricezione di un messaggio.
- RST (reset)
	Per cancellare una trasmissione affidabile.
# RESOURCE OBSERVATION
Lo stato delle risorse in un server può variare in ogni momento. Per evitare il polling (client che ogni lasso di tempo richiede lo stato della risorsa) e implementare una specie di pattern observer, si usa l'osservazione delle risorse.
Non tutte le risorse sono osservabili. Il server deve marcare le risorse osservabili tramite l'attributo $obs$.
##### Lista degli observer
Un client può includere in una GET l'informazione che vuole osservare la risorsa richiesta, cioè vuole essere notificato quando e se la risorsa cambia stato.
In questo caso il server deve aggiornare la lista della risorsa (una per ogni risorsa osservabile) che contiene i client che hanno richiesto la sua osservazione; un client nella lista non è identificato singolarmente, ma anche dal token del messaggio, che serve poi per correlare la richiesta con futuri aggiornamenti sulla risorsa.
##### Opzione
L'opzione per l'osservazione è l'opzione numero 6. I suoi valori sono 1 per la registrazione, 0 per la deregistrazione e gli altri valori sono usati per ordinare le notifiche.
# TRASFERIMENTO A BLOCCHI
È stato introdotto per dividere grandi quantità di dati in molti blocchi di una data grandezza, dove grandi dipende dal contesto (rispetto alla dimensione di un pacchetto UDP o grande rispetto alla dimensione di un buffer).
La frammentazione viene fatta a L5 e non a L3.
##### Opzioni
Sono state introdotte due opzioni, con lo scopo di gestire la divisione di un grande payload in blocchi più piccoli (grandezza che può anche essere negoziata dagli endpoint tramite l'opzione $size1$ e $size2$), per gestire il trasferimento di ogni blocco e la successiva ricostruzione del destinatario.
- $block1$
	È l'opzione numero 27. È associata al payload della richiesta e solitamente si usa insieme a una POST o PUT.
- $block2$
	È l'opzione numero 23. È associata al payload della risposta e solitamente si usa insieme a una GET.

Il valore di queste opzioni (block) è variabile in base a tre campi:
- NUM è relativa al numero di blocco nella sequenza.
- M è un bit aggiuntivo che indica se ci sono ancora blocchi che seguono quello in questione.
- SZX è composto da 3 bit e indicano l'esponente della dimensione del blocco. Deve essere $0\le SZX\le6$ e la grandezza del blocco è $2^{4+SZX}$.
# OTTENERE RISORSE
##### Reource discovery
Per ottenere le URI delle risorse che un server CoAP ospita c'è un meccanismo basato sul Web linking and CoRE Link Format: il client può inviare una richiesta con il path */.well-known/core* a un server o a un insieme multicast e riceverà una lista di risorse disponibile formattata in CoRE Link Formate. Ogni link contiene un insieme di attributi che descrivono una risorsa: *obs* indica se la risorsa è osservabile, *rt* il tipo della risorsa, *ct* il tipo del contenuto e *if* definisce l'insieme di metodi che la risorsa accetta (CoRE interface).
##### Resource directory
A volte non è possibile ottenere le risorse tramite */.well-known/core*: i nodi server possono essere in sleeping mode oppure il traffico multicast sarebbe inefficiente.
In queste situazioni si può usare il resource directory: gli endpoint (coloro che ospitano le risorse) usano un registro centralizzato per registrare, mantenere, rimuovere e ricercare (di altri) le risorse.
# PROXY
Tra i [componenti](REST.md#COMPONENTI) dell'architettura REST abbiamo citato gli intermediari.
Il loro compiti all'interno di CoAP sono:
- Adattare client che non supportano CoAP oppure che non sono compatibili con una risorsa presente in una network constrained. Cioè per supportare formati che potrebbero non essere adatti per le applicazioni con vincoli.
- Proteggere la rete vincolata contro l'esterno, ad esempio da attacchi DoS (il cui obiettivo è quello di esaurire le risorse del sistema).
- Integrare la rete constrained che usa CoAP con reti HTTP esistenti.
- Diminuire il carico di rete.
- Garantire la disponibilità e la latenza con cui si ottengono le risorse tramite il caching (memorizzazione nella cahce).
##### HTTP/CoAP proxing
- Effettuare la traslazione del protocollo da HTTP a CoAP.
	È più complesso e si seguono queste regole:
	- URI mapping
		È basato sull'incapsulamento dell'URI CoAP in HTTP, visto che il primo è incluso nel secondo.
	- Request mapping
		Se si usa un metodo HTTP che non esiste in CoAP allora non si permette la traduzione; altrimenti il metodo rimane lo stesso.
	- [Response code mapping](CoAP%20response%20mapping.png)
	- Header mapping
		Si mappa l'header HTTP usando le opzioni di CoAP.
- Effettuare la traslazione del protocollo da CoAP a HTTP.
	È semplice, visto che CoAP è un sottoinsieme di HTTP.
- Può mappare una richiesta CoAP in un'altra dello stesso tipo (CoAP to CoAP proxy).