In un sistema la probabilità di attivare un [fault](fault%20error%20failure%20chain.md#Fault%20(guasto/difetto)), se il sistema gira all'infinito, è 1. L'idea della [fault tollerance](dependability.md#Tecniche) è permettere al sistema di non deviare dal corretto funzionamento anche quando un guasto si attiva.
Si implementa spesso con sclete architetturali data dalla [fault masking](fault%20masking.md) che si basano sulla ridondanza e in particolare mira alla [design diversity](ridondanza.md#Design%20diversity).
![Screenshot 2026-03-12 alle 09.47.44](fault%20tollerance%20tesps.png)
##### Tassonomia
Non è possibile gestire tutti i fualt e tutti gli errori: c'è una tassonomia che ci permette di scegliere quali classi vogliamo gestire.
##### Ricorsione
La fault tollerance è ricorsiva: se introduciamo un componente che si occupa di validare dei risultati o degli stati allora anche questi devono essere controllati.
L'idea generale è che il componente che controlla deve essere molto più semplice, il che permette di ridurre lo spazio degli fault che possiamo introdurre in esso.
##### Coverage
Dopo aver implementato una tecnica dobbiamo capire se questa è valida. Per farlo si usa la coverage, una misura probabilistica condizionata: dato un fault, vogliamo calcolare la probabilità che esso sia trovato.
Non è possibile avere una coverage di 1 per via delle assunzioni che facciamo: oltre al fatto che questo possonoe essere sbagliate, ci saranno sempre degli scenari che possono accadere (anche se con probabilità molto bassa) e che non sono coperti.
# Error detection
L'obiettivo è trovare all'interno del sistema gli [errori](fault%20error%20failure%20chain.md#Error) che si sono già verificati: se questi sollevano dei messaggi allora è semplice, altrimenti se sono latenti è più complesso.
In teoria se tutti gli errori fossero rilevati non ci sarebbe nessun fallimento, ma aggiungere tecniche di error detection è un costo, sia in termini di ovehead che di denaro per i componenti ridondati.
##### Strategie
Ci sono due strategie che possiamo usare:
- Concorrenza
	Si vogliono trovare gli errori mentre il sistema è in esecuzione.
- Preemptive
	Si trovano gli errori quando il sistema è sospeso.
##### Metriche
Ci sono delle metriche quantitative per valutare il nostro sistema dii error detection:
- Costo
- Latenza
- Coverage
	È definita come $P(det|err)$, cioè la probabilità di trovare un errore condizionata dal fatto che l'errore è presente. In pratica è l'abilità del sistema di trovare gli errori quando ci sono e misura il numero di falsi negativi.
- Accuracy
	È definita come $P(err|det)$, cioè la probabilità che l'errore ci siano quando il mio sistema lo trova. In pratica è l'abilità del sistema di non sbagliare quando trovano un errore e misura il numero di falsi positivi.
##### Checker
Vorremmo implementare un checker perfetto, ma nella realtà ci accontentiamo di uno accettabile.
- Perfetto
	Si tratta di un oracolo posto tra l'output del sistema e l'output effettivamente restituito che controlla la sua validità.
	Deve essere indipendente dal sistema e derivante solo dai requisiti.
	Abbiamo inoltre il problema di "chi controlla i controllori".
- Accettabile
	L'obiettivo diventa quello di identificare la maggior parte degli errori: si fissa una soglia su una qualche [metrica](#Metriche) (e.g., la coverage) in modo che il nostro checker sia accettato se la probabilità di rilevare un guasto è maggiore di tale soglia.
##### Classi di checker
Ci sono diversi tipi di checker:
- Replication
	Abbiamo due identiche copie che eseguono in parallelo e un checker che compara i risultati; se i guasti sono indipendenti e il checker è corretto allora riesco a capire se ci sono errori, ma non dove l'errore è stato generato.
	Posso estendere questo checker: se utilizzo più di due unità allora posso capire quale componente ha generato l'errore; se utilizzo la [design diversity](ridondanza.md#Design%20diversity) posso catturare errori di design, cioè quegli errori che sono derivanti da una progettazione del componente sbagliata.
	Il problema è la differenza di throughput tra i vari componenti (il clock può essere diverso, i.e. può fallire, anche se fanno la stessa identica cosa): il checker è efficiente se confronta bit a bit; dobbiamo aggiungere un delay prima del confronto oppure andare come il più lento.
- Timing
	Sono controlli sui [real time](real%20time.md) system.
- Resersal
	Il codice implementa semplicemente una funziona; se riesco a trova la funzione inversa e questa è più semplice da calcoalre (e.g., radice e elevamento a potenza) allora posso eseguire l'inversa passandogli il risultato e vedere se ottengo l'input.
- Coding
	Si vuole ridondare l'informazione: se abbiamo un array di $n$ bit, allora possiamo replicare 4 volte 25 bits. Dato un insieme di code words, cioè un dizionario di parole (i.e., sequenze di bit) corrette, per valutare se i dati sono corrotti allora possiamo usare la [hamming](Distanza.md#Hamming) distance tra i dati e le parole del dizionario.
	In questo contesto è importante valutare l'efficienza che vogliamo avere: più replichiamo, meno dati possiamo inviare a parità di spazio.
	- se un insieme di code words ha una distanza di hamming minima $d$ allora il sistema è in grado di rilevare al più $t|t<d$ errori in una parola: se ci fossero $t=d$ errori allora la parola corretta che è diventata sbagliata per questi $t$ errori potrebbe sovrapporsi a una corretta.
	- Tecniche
		Ci sono varie tecniche per decidere come ridondare le informazioni: con i codici di parità si aggiungono bit infonda ai dati in modo da rendere pari il numero di bit o (byte) a 1; in $\frac{m}{n}$ per avere una parola valida di $n$ bit esattamente $m$ bit devono valere 1; nel checksum si fa un'operazione di modulo e si concatena infondo al dato.
- Reasonableness
	Hanno l'obiettivo di controllare lo stato dei componenti per capire se esso è allineato semanticamente con i requisiti di quel componenti. Solitamente si verifica tramite difence programming, implementata tramite asserzioni a run-time.
	Per esempio se abbiamo una variabile $angle$ questa dovrà avere un valore in $[0,360]$.
- Structural
	Si applica alle strutture dati per controllare la consistenza semantica delle informazioni e quella strutturale.
	Un esempio è capire se tutti i puntatori in una linked list sono ancora validi.
- Diagnstic
	Sono spesso controlli sull'hw e si implementano conoscendo sia l'input che l'output.
- Control flow
	Non sono controlli sui data ma sul flusso che un programma deve eseguire.
##### Quando
Possiamo utilizzare il checker in due momento, possibilmente insieme:
- Last moment
	Dopo aver fatto tutta la computazione e prima di restituire l'putput. Si tratta di un checker con poco overhead, ma che ci da un'indicazione solo sul modulo che ha generare l'errore e non quali sia il componente all'interno del modulo.
- Early
	Si rileva l'errore durante la computazione. È più costoso ma poi è più facile correggere l'errore perhé sappiamo quale è il componente che lo ha generato.
# System recovery
Si porta nuovamente il sistema in uno stato corretto. Questo è fatto gestendo l'errore e il fault che lo ha causato.
##### Error handling
Si elimina l'errore presente nel sistema, cioè lo stato errato che è presente.
- Rollback
	L'idea è di torna indietro a uno stato che sappiamo essere corretto perché memorizzato in un checkpoint. Dobbiamo trovare un compromesso tra quando fare il checkpoint e quanto vogliamo tornare indietro nel tempo.
- Compensation
	Si ripristina lo stato corretto usando la ridondanza: se abbiamo più copie (almeno 3) della stessa variabile allora possiamo stabilire quale sia il valore corretto.
- Rollforward
	Sappiamo che lo stato è sbagliato ma andiamo avanti con la computazione. Solitamente queste si traduce quando si rileva un errore e si esegue poi un execption handler.
##### Fault handling
Oltre a correggere l'errore dobbiamo identificare il guasto che lo ha provocato in modo che non succeda ancora.
La pipeline è la seguente:
- Diagnosi: Si deve capire quale è il fault che ha portato all'errore.
- Isolation: Si deve rimuovere dal sistema tale componente.
- Riconfigurazione: Si deve sostituire il vecchio componente con uno nuovo.
- Inizializzazione: Si deve aggiornare il sistema in modo da attuare l'aggiornamento.