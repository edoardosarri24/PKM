---
Professore: Michele Boreale
Esame: 2023-01-23
Voto: 20
---
## VARIABILI
- Rendendo una variabile $volatile$ essa deve essere sempre letta e scritta dalla RAM e non dalla cache del processore. Questo impedisce il caching dei processori (Ottimizzazione per cui ogni processore ha una propria cache dove tiene i dati che sua prima di metterli nella RAM).
- Usare comandi atomici LCR (Che fanno al più un accesso alla memoria globale, cioè quella condivisa) serve per mantenere l'atomicità a livello assembly. Si fa usando variabili locali.
## THREAD
- Un thread è oggetto di classe $Thread$ che implementa $run$. Si attiva con $start$. Si attende la sua fine con $join$.
- Un thread termina o perchè finisce le istruzioni da eseguire o perchè ha sollevato un'eccezione. Per forzare la conclusione del thread ci sono due modi:
	- Se è il thread stesso a decidere di interrompersi si usa $return$. Non $stop$ perché deprecato.
	- Se un thread vuole interromperne un altro ci possiamo basare sul flag booleano, detto stato di interruzione, che ogni thread possiede. Questo flag è inizialmente posto a false; il metodo d'istanza $interrupt$ lo pone a vero. Se il thread che può essere sospeso da altri controlla il suo flag (O all'inizio di un ciclo o ogni tanto) con $isInterrupt$ e si arresta se questo è vero allora tutto funziona.
## LOCK
- Ottimi per gestire l’accesso alle singole risorse oppure ad una CS.
- Varie implementazioni:
	- Synchronized (In Java)
		- Attesa attiva.
		- Per metodi il lock è $this$. Per statements il lock può essere un oggetto qualunque.
	- Lock
		- Attesa attiva.
		- Si deve gestire il lock in modo esplicito: dichiarare un lock e chiamarci il metodo $lock$ e $unlock$.
		- In Java i lock sono rientrarti: solo il lock che già lo detiene può acquisirlo nuovamente (C'è un contatore che gestisce il rilascio).
	- Semaphore
		- Attesa passiva.
		- Un semaforo binario è più pesante rispetto ad un $Lock$. Da usare se c'è molto contention.
## SEMAFORI
- Attesa passiva.
- Ottimi per la sincronizzazione tra processi o per la gestione di più risorse. Per CS o una risorsa meglio i lock.
- In Java
	- $acquire$ non completata: eseguirla in un ciclo.
	- Ogni semaforo ha un solo waitset, che è non ordinato; se si vuole ordinato si deve inizializzare uno strong semaphore.
	- Un thread sospeso sul waitset può essere svegliato da un thread qualsiasi e non solo dopo una $release$ (Cosa che succede invece con i lock). Utile nel recupero di un deadlock.
## MONITOR
- Si incapsula la CS o le risorse all'interno di un monitor e vi si accede solo tramite metodi dichiarati dal monitor.
- Politiche di resumption
	- S&C
		Il controllo rimane a chi ha eseguito la signal. Ci sono due varianti: nella prima il processo risvegliato ha la priorità sui processi esterni; nella seconda il processo risvegliato e quelli esterni hanno la stessa precedenza.
	- IRR
		Il controllo viene preso dal risvegliato
- Java
	- Synchronized
		- Si usa quando c'è una sola condizione per cui i thread aspettano. I thread si sospendono sull'unico wait set del monitor (Di $this$).
		- Si implementa dichiarando tutti i metodi synchronized e tutti i campi private.
		- $wait$ non completata: eseguirla in un ciclo.
	- Lock
		- L'acquisizione del lock provoca attesa attiva.
		- Interfaccia che mette a disposizione $newCondition$ (Metodo di istanza). Sulla condition si chiama $await$, $signal$ o $signalAll$. Le condition non sono ordinate (Meglio così per l'efficienza) a meno che non si voglia.
		- I metodi devono: acquisire il lock, controllare la condizione ed eventualmente sospendersi, segnalare ad altri thread se una condizione è diventata vera, rilasciare il lock.

## MESSAGE PASSING
- Non si ha memoria condivisa.
- Canali
	- Simmetrici
	- Interfaccia $BlockingQueue$. Classe $SynchronousQueue$ per sincronismo; $ArrayBlockingQueue$ per asincronismo.
- Rendez-Vous
	- Asimmetrico e sincrono
	- Con canali sincroni: il canale su cui il server deve rispondere è inviato insieme al messaggio.
	- Con Socket di $java-net$.