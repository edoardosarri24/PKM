In un sistema la probabilità di attivare un [fault](fault%20error%20failure%20chain.md#Fault%20(guasto/difetto)), se il sistema gira all'infinito, è 1. L'idea della [fault tollerance](dependability.md#Tecniche) è permettere al sistema di non deviare dal corretto funzionamento anche quando un guasto si attiva.
La fault tollerance utilizza la ridondanza, in particolare mira alla [design diversity](ridondanza.md#Design%20diversity). Essa può essere a livello di tempo, se eseguiamo più volte la stessa cosa, o a livello di componente, se mettiamo più elementi che fanno la stessa cosa in parallelo.
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
Si devono trovare all'interno del sistema gli [errori](fault%20error%20failure%20chain.md#Error) che si sono già verificati: se questi sono originati da messaggi allora è semplice, altrimenti se sono latenti è più complesso.
In teoria se tutti gli errori fossero rilevati non ci sarebbe nessun fallimento, ma aggiungere tecniche di error detercndetection è un costo, sia in termini di ovehead che di denaro per i componenti ridondati.
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
	Si vuole ridondare i dati: se abbiamo un array di $n$ bit, allora possiamo replicare 4 volte 25 bits. Per valutare se i dati sono corrotti allora possiamo usare la [hamming](Distanza.md#Hamming) distance.
	In questo contesto è importante valutare l'efficienza che vogliamo avere: più replichiamo, meno dati possiamo inviare a parità di spazio.
- Reasonableness
- Structural
- Diagnstic
- Control flow
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