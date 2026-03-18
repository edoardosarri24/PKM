Aggiunge funzionalità ad un oggetto in modo dinamico e senza influenzare altri oggetti.
- Strutturale
- Inheritance e Object composition.
## APPLICABILITÁ
- Non è possibile aggiungere funzionalità con l'inheritance (Ad esempio la classe è $final$).
- Si vuole aggiungere più volte la stessa funzionalità.
## STRUTTURA
![[Decorator.png]]
- $Component$
	È l'interfaccia dei componenti a cui si vogliono aggiungere funzionalità. Definisce le operazioni comuni a tutti i partecipanti del pattern.
- $ConcreteComponent$
	È l'implementazione concreta di $Component$.
- $Decorator$
	Classe base dei decoratori. Ha un riferimento ad un singolo $Decorator$. Implementa l'operazione delegandola al componente.
- $ConcreteDecorator$
	Rappresenta la funzionalità che si vuole aggiungere. Fa qualcosa prima o dopo la delega, ma mai al suo posto.
## CONSEGUENZE
- Ogni oggetto, sia decoratore che decorato, si occupa dello stretto necessario rispettando l'[[single responsability principle|SRP]].
- Decoratori e decorati hanno la stessa interfaccia e quindi sono trasparenti (Sia nel numero che nel tipo concreto) per i client.
- Non ci possiamo basare sull'identità degli oggetti: si possono avere due oggetti praticamente uguali ma diversi in memoria.
- Non si può decorare più componenti o più volte lo stesso componente (Cioè un $ConcreteComponent$) contemporaneamente.
## SOLUZIONE
- Il $ConcreteComponent$ non piò essere $null$; si deve controllare nel costruttore di $Decorator$.
- $Decorator$ può implementare l'operazione con un [[Template method]].
## FUNZIONI
Se $Component$ ha una sola operazione allora col pattern stiamo simulando la composizione di funzioni. Se il linguaggio la mette a disposizione la programmazione funzionale (Le lambda in Java) allora sono da preferire.
La soluzione è creare un'interfaccia funzionale con un solo metodo astratto. Si aggiungono poi altri due metodi con un'implementazione di default che hanno come parametro e tipo di ritorno lo stesso tipo dell'interfaccia funzionale: uno chiama prima il metodo astratto passandogli il parametro della lambda e poi chiama il metodo astratto sul parametro del metodo di default passandogli il parametro della lambda; l'altro fa viceversa.