Si vuole disaccoppiare chi invia la richiesta da chi la gestisce.
- Comportamentale
- Inheritance e Object composition.
## APPLICABILITÁ
- Ci sono più oggetti che potrebbero gestire una richiesta.
- Si vogliono scegliere gli oggetti che possono soddisfare una richiesta in modo dinamico. 
## STRUTTURA
![[Chain of responsability.png]]
- $Handler$
	Difinisce l'interfaccia per gestire la richiesta.
- $ConcreteHandler$
	gestisce la richiesta per cui è responsabile nel modo opportuno.
- $Client$
	Invia la richiesta al primo membro della catena ignorandone il tipo concreto.
## CONSEGUENZE
- Il client non ha la garanzia che la richiesta venga soddisfatta.
## SOLUZIONE
- $Handler$ deve essere astratta in modo da non essere istanziata. Si mantiene un riferimento ad prossimo $Handler$. Nel metodo se il prossimo gestore esiste (La fine della catena deve essere indicata con $null$) gli delega la richiesta altrimenti ritorna.
- Ogni $ConcreteHandler$ esegue l'override del metodo in modo da gestire la richiesta se ne è in grado, altrimenti richiama il metodo della superclasse. Se un $ConcreteHandler$ sa che impiega molto tempo a gestire la richiesta può prima inoltrare e poi gestire: in questo caso la logica deve essere implementata in $ConcreteHandler$ e non in $Handler$ (Non si può usare il [[Template method]]).
- Il prossimo $Handler$ può essere passato tramite costruttore o tramite setter: nel primo caso una volta creata la catena non è più modificabile; nel secondo la si può modificare a runtime.
- Si può usare un [[Template method]] nella superclasse: il metodo che il prossimo $Handler$ sappia gestire la richiesta e se così è ritorna tale valore; altrimenti se esiste un altro $Handler$ gli inoltra la richiesta; altrimenti ritorna.
## FUNZIONI
Se $Handler$ ha un solo metodo stiamo concatenando funzioni.
In questo caso si crea un'interfaccia statica con un metodo statico; i parametri sono quelli del dominio applicativo e una serie di $Handler$ (Classi che implementano l'interfaccia $Handler$). Il metodo statico applica in sequenza i gestori.