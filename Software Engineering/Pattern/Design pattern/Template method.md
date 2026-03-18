Definisce la struttura di un algoritmo in una superclasse e lascia alle sottoclassi solo l'implementazione di alcuni suoi passi.
- Comportamentale
- Inheritance
## STRUTTURA
![[Template method.jpg]]
- $AbstractClass$
	È una classe astratta che definisce la struttura del metodo, dichiarata $final$ per impedire la sua modifica da parte delle sottoclassi, chiamando al suo interno metodi astratti (Hooks).
- $ConcreteClass$
	Sottoclassi di $AbstractClass$ che devono implementare solo gli hooks.
## CONSEGUENZE
- Si evita duplicazione di codice fattorizzando il comportamento comune a più sottoclassi.
- Si ha il controllo su come le sottoclassi implementano il metodo.
- Non si può cambiare l'implementazione di un'operazione in una sottoclasse se non cambiando il suo codice o creandone una nuova.
- Si realizza l'[[Open-Close principle|OCP]] perchè si può creare una nuova operazione senza mettere mano al codice esistente.