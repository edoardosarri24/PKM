Converte l'interfaccia di una classe in modo che possa essere accettata da un client.
- Strutturale
- Inheritance e Object composition.
## APPLICABILITÁ
- Si vuole usare una classe già esistente per un client, ma la sua interfaccia è diversa da quella richiesta.
## STRUTTURA
![[Adapter.png]]
- $Client$
	Usa specializzazioni di $Target$, cioè usa $Adapter$, senza sapere di $Adaptee$.
- $Adaptee$
	È la classe che si vuole utilizzare e di cui si deve convertire l'interfaccia in $Target$.
- $Target$
	È l'interfaccia a cui $Adaptee$ si deve uniformare.
- $Adapter$
	Deve implementare o estendere sia $Adaptee$ che $Target$; se sono entrambe classi e il linguaggio non lo permette si deve estendere $Target$ e usare l'object composition con $Adaptee$, delegandogli le operazioni.
## CONSEGUENZE
- $Adapter$ è sia di tipo $Adaptee$ che $Target$. Se questo è un problema si deve usare l'object composition.
- Nella versione object composition si possono aggiungere funzionalità prima e dopo quelle che svolgerà $Adaptee$.
- Nella versione object composition si può avere delle sottoclassi di $Adaptee$ e passarle a $Adapter$ tramite costruttore.
- Nella versione con l'inheritance se $Adaptee$ ha una sottoclasse che specializza il metodo allora questo non può essere usato (L'inheritance è un meccanismo statico).
## VARIANTI
Se si ha un'interfaccia e si ha bisogno solo di alcuni suoi metodi se ne può creare un'implementazione che per ogni metodo non fa nulla o fa quello che la maggior parte fa e poi implementare questa.