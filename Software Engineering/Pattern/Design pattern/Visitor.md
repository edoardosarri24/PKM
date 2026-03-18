Realizzare un'operazione che performa sugli elementi di una struttura (Che hanno lo stesso supertipo) in base al tipo concreto senza modificarla (Aggiungere operazioni su una struttura di entità).
- Comportamentale
- Inheritance e Object composition.
## APPLICABILITÁ
- La struttura cambia raramente.
- Sopperisce alla mancanza dell'overloading dinamico dei linguaggi di programmazione tramite il double-dispatch (Se il metodo ha un solo parametro).
## STRUTTURA
![[Visitor.png]]
- $Element$
	È il supertipo astratto e comune della struttura. Dichiara il metodo stratto $accept$ che ha come parametro il tipo $Visitor$.
- $ConcreteElement$
	Classi concrete della struttura. Devono implementare $accept$ chiamato sul $Visitor$ passato come parametro il metodo $visit$ passandogli $this$ come parametro.
- $Visitor$
	Interfaccia che visita la struttura. Deve dichiarare tanti metodi quanti sono i $ConcreteElement$.
- $ConcreteVisitor$
	Ognuno rappresenta un'operazione e deve essere implementata in nodo opportuno.
- $Client$
	Creerà il $ConcreteVisitor$ in base all'operazione che vuole eseguire. Sull'oggetto concreto della struttura chiamerà $accept$ passandogli il $ConcreteVisitor$ opportuno.
## CONSEGUENZE
- Applicazione dell'[[Open-Close principle|OCP]]: si aggiungono nuove operazioni sulla struttura senza modificarla.
- Applicazione del [[single responsability principle|SRP]]: la struttura modella gli oggetti e il Visitor le operazioni.
- Complessità costante usando due volte il binding dinamico.
- Non si hanno eccezioni o problemi a runtime; se qualcosa non torna è il compilatore ad avvertirci.
- Rompe l'incapsulamento della struttura: perchè il Visitor acceda allo stato degli oggetti si devono avere dei getter pubblici.
## VARIANTI
- Overloading
	Nel $Visitor$ i metodi possono essere dichiarati come $visit(xxx\ x)$ oppure come $visitxxx(xxx\ x)$: nel primo caso saranno metodi in overloading perchè staticamente $xxx$ sarà diverso per ogni metodo; nel secondo caso no, ma forse si guadagna in leggibilità (Anche se non credo).
	Nei $ConcreteElement$ il codice sarà sempre lo stesso; non è codice duplicato e non può essere fattorizzato perchè staticamente $this$ sarà diverso per ogni classe concreta.
- Void
	Se l'operazione non restituisce nulla non si ha problemi: $visit$ e $accept$ possono essere dichiarati $void$ senza problemi.
	Se l'operazione deve restituire qualcosa e si vuole mantenere il $void$ allora ogni $ConcreteVisitor$ dovrà avere uno stato dove accumulare il risultato e poi avere un metodo per restituire tale risultato.
- Generico
	Se ogni operazione, cioè ogni $ConcreteVisitor$, restituisce tipi diversi e non si vuole dichiarare $visit$ e $accept$ come $void$ si deve: parametrizzare l'interfaccia $Visitor$, il cui tipo sarà dichiarato da ogni $ConcreteVisitor$; rendere generico il metodo $accept$ dichiarato da $Element$.