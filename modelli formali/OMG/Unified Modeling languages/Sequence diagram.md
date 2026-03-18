Il sequence diagram vuole modellare l'interazione tra oggetti (A questo livello un partecipante può anche non essere implementato nel nostro codice). È un diagramma comportamentale che:
- Specifica i partecipanti a chiamate e risposte.
- Descrive la sequenza delle comunicazioni all'interno di un sistema.
- Descrive le comunicazioni parallele consentite.
## RAPPRESENTAZIONE
- Il tempo va dall'alto verso il basso. In orizzontale sono elencate le invocazioni di metodi.
- Ogni partecipante è identificato all'interno di un rettangolo con $Ruolo:Classe$: il ruolo è qualcosa di più astratto rispetto una classe (Un oggetto può avere più ruoli durante il suo ciclo di vita); la classe non è fondamentale e si può omettere.
- Il ciclo di vita di un partecipante si indica con una linea tratteggiata, chiamata lifeline. Se un partecipante termina il suo ciclo di vita prima rispetto all'interno sistema si usa una X alla fine della sua lifeline.
- L'invio di un messaggio è rappresentato tramite una freccia continua (Piena messaggi sincroni e vuota messaggi asincroni); si può indicare un tempo o un orario in cui eseguire inviare tale messaggio (Di default è $now$). L'invio di una risposta è rappresentato tramite una linea tratteggiata; la sua sintassi, usata sopra la freccia è $att=msg(par1,...):val$, con: $att$ è la variabile in cui è salvato il valore di ritorno; $msg$ è il nome del messaggio; $par$ sono i parametri; $val$ è il tipo di ritorno. [Esempio](Sequence%20diagram%20-%20Messaggi.png).
- Per indicare il tempo di esecuzione di uno scambio di messaggi si possono racchiudere le interazioni in una barra continua sulla lifeline.
- Durante il ciclo di vita un partecipante può creare oggetti: si fa partire una freccia tratteggiata con la keyword $new$.
- Se il mittente o il ricevente di un messaggio non è chiaro allora si usano i found message o lost message: il primo è rappresentato da una freccia che parte da un pallino pieno ed entra nella lifeline; il secondo è rappresentato da una freccia che parte dalla lifeline ed entra in un pallino pieno.
## FRAMMENTI COMBINA
### Branch and loop
Ogni frammento è inserito in un blocco. In alto a sinistra è indicato l'operatore.
- $alt$
	Rappresenta uno $switch$, cioè una sequenza di $if-else$. Sulla sinistra dell'operatore c'è la guardia (Condizione). I vari $case$ sono suddivisi da una linea tratteggiata orizzontale. [Esempio](Sequence%20diagram%20-%20alt%20e%20opt.png).
- $opt$
	Rappresenta un singolo $if$, cioè una sequenza di messaggi opzionale: si eseguono solo se la guardia risulta vera. [Esempio](Sequence%20diagram%20-%20alt%20e%20opt.png).
- $loop$
	Rappresenta un ciclo. Accanto alla keyword $loop$ è indicato il numero di iterazioni tramite qualcosa simile alla cardinalità (Ad esempio (1,\*) o (1,4)). Ci può essere una guardia.
- $break$
	È una sorta di gestore delle eccezioni: se la guardia risulta vera si fanno le operazioni al suo interno e poi si esce dal blocco che contiene il blocco $break$, saltano le operazioni rimanenti.
### Concurrency
- $seq$
	Rappresenta l'ordine di default per l'esecuzione: si hanno tante azioni parallele senza avere un'imposizione tra quelle non correlate. Se due partecipanti sono scorrelati non si può dire quale azione viene fatta prima; la precedenza è chiara solo se i partecipanti comunicano.
- $strict$
	Si vuole forzare il flusso di esecuzione secondo un ordine: si separa il blocco orizzontalmente con una linea tratteggiata; prima si esegue la parte sopra e poi la parte sotto; entrambe sono con l'ordine di default $seq$.
- $par$
	Si vuole parallelizzare più flussi di esecuzioni: si separa il blocco orizzontalmente con una linea tratteggiata; i messaggi all'interno di ogni blocco seguiranno l'ordine di default (seq), ma i due blocchi possono essere eseguiti in parallelo e ci può essere dell'interleaving tra le azioni di blocchi separati.
- $critical$
	Rappresenta un blocco atomico: le istruzioni al suo interno sono sempre eseguite secondo l'ordine di default ($seq$) ma tutte insieme.