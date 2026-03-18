I patterns i classificano in base a:
- Purpose (Proposito)
	- Creazionali
		Si occupano della creazione di oggetti. Spesso non si hanno costruttore in overloading ma si ha il costruttore con i campi richiesti e setter con campi facoltativi.
		- [[Builder]]
		- [[Singleton]]
		- [[Static factory method]]
	- Strutturali
		Definiscono come classi e oggetti sono composti e strutturati. Lascia allo sviluppatore il compito di decidere il flusso di esecuzione.
		- [[Composite]]
		- [[Facade]]
		- [[Adapter]]
		- [[Decorator]]
	- Comportamentali
		Definiscono come classi e oggetti interagiscono e si distribuiscono le responsabilità. Il flusso di esecuzione è fissato dal pattern.
		- [[Template method]]
		- [[Strategy]]
		- [[Visitor]]
		- [[observer]]
		- [[chain of responsability]]
- Scope (Campo d'azione)
	- Class
		Definiscono le relazioni tra tipi e usano inheritance e override. Servono per avere un controllo statico sugli oggetti.
	- Object
		Definiscono le relazioni tra oggetti e usano object composition e delagation. Servono per sfruttare il polimorfismo.