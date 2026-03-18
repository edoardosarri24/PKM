Si incapsula un'insieme di algoritmi (Classi) in un tipo comune e si rendono intercambiabili senza che il client se ne accorga.
- Comportamentale
- Inheritance e object composition
## STRUTTURA
![[Strategy.png]]
- $Strategy$
	Interfaccia che dichiara l'operazione comune a tutta la famiglia.
- $ConcreteStrategy$
	Sottotipi di $Strategy$ che implementano l'operazione da essa definita.
- $Context$
	Client che ha un riferimento statico a $Strategy$. A runtime, tramite un setter o il costruttore, questo sarà un $ConcreteStrategy$.
## CONSEGUENZE
- SI perde un po' di efficienza a causa dell'overhead di avere più chiamate di funzioni.
- Si realizza il [[Liskov sobstituition principle|LSP]] perchè il client funziona con qualunque implementazione di $Strategy$.
- Si realizza il [[single responsability principle|SRP]] perchè ogni classe viene modificata solo per un motivo.
- Si realizza l'[[Open-Close principle|OCP]] perchè si possono realizzare nuove implementazioni senza cambiare il codice esistente.
## VARIANTI
Si può implementare la stessa cosa con soluzioni più moderne:
- Invece di avere più classi concrete $ConcreteStrategy$ si ha una singola classe di utilità; questa ha tanti metodi statici quante sono le $ConcreateStrategy$ e ognuno di essi restituisce un oggetto di tipo $Strategy$ implementato con una classe anonima.
  Il $Context$ avrà sempre un riferimento a $STrategy$ ma chi lo usa dovrà chiamare un metodo statico della classe di utilità.
- Se $Strategy$ ha un solo metodo astratto il modo precedente può essere anche implementato facendo restituire una lambda invece di una classe anonima.