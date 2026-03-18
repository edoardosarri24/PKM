Mantenere la consistenza tra oggetti dove c'è una relazione uno a molti senza che essi siano strettamente accoppiati.
- Comportamentale
- Inheritance e Object composition.
## APPLICABILITÁ
- Si vuole notificare degli oggetti di un cambiamento dello stato di un altro.
## STRUTTURA
![[Observer.png]]
- $Subject$
	È l'astrazione del soggetto (Classe astratta, non deve essere implementata). Ha una collezione di $Observer$. Dichiara i metodi per aggiungere, rimuovere e notificare gli osservatori.
- $ConcreteSubject$
	È l'implementazione di $Subject$. Ha uno stato, dei metodi che lo modificano e un metodo che lo condividono. Quando modifica lo stato deve chiamare il metodo $notify$.
- $Observer$
	Interfaccia degli osservatori. Dichiara il metodo $update$.
- $ConcreteObserver$
	Implementa $Observer$ e si memorizza il soggetto da osservare. Quando viene invocato $update$ accederà allo stato del soggetto e modificherà il suo.
## CONSEGUENZE
- In Java c'è una classe Observable e un'interfaccia Observer; sono mal implementate e deprecate. Non è da usare.
- È la soluzione opposta al polling, dove sono gli osservatori che periodicamente interrogano il soggetto.
- Non ci sono accoppiamenti: si possono riusare i soggetti e gli osservatori senza che essi lo sappiano.
- Pattern sincrono: dopo la modifica dello stato si deve aspettare che ogni osservatore abbiamo svolto le proprie azioni. Per risolvere questo si usa il pattern [[Publish subscribe]].
## VARIANTI
- Notify
	Dovrebbe essere chiamato dal soggetto; se se avessero però molti cambi di stato questo diminuirebbe l'efficenza. Può essere chiamato dal client, ma se ne deve ricordare.
- Più soggetti.
	- La firma di $update$ deve contenere il $ConcreteSubject$ che ha modificato lo stato.
	- Se i $ConcreteSubject$ hanno già una classe comune non si può ereditare anche dalla classe $Subject$. In questo caso si deve dichiarare il meccanismo dei soggetti nella classe comune.