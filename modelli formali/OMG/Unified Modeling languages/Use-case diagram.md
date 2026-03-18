È un digramma comportamentale che descrive le funzionalità del sistema e i suoi utilizzatori.
È usato nella definizione dei [requisiti](requirement%20engineering.md#REQUIREMENT%20ENGINEERING%20PROCESS) per esprimere le richieste del cliente.
# Partecipanti
### Sistema
Si rappresenta come un grosso rettangolo. Contiene gli use case ma non gli attori.
### Use case
Ogni use case rappresenta una singola funzionalità del sistema.
- Sono rappresentati come degli ovali con delle scritte dentro. Si possono trovare anche con le scritte fuori o come rettangoli con le scritte e un ovale all'interno. [Esempio](Use%20case%20diagram%20-%20Use%20case.png).
- Possono essere astratte e avere delle specializzazioni (Come nelle [generalizzazioni](Class%20diagram.md#GENERALIZZAZIONE) del class diagram).
### Attori
- Sono coloro che interagiscono col sistema attraverso gli use case; è bene specificare che non fanno parte del sistema e quindi devono essere rappresentati al'esterno del rettangolo.
- Possono essere astratti e avere quindi delle specializzazioni (Come nelle [generalizzazioni](Class%20diagram.md#GENERALIZZAZIONE) del class diagram); in questo modo si evita che due attori abbiamo una relazione con lo stesso use case. [Esempio](Use%20case%20diagram%20-%20Generalizzazione%20attori.png).

Possono essere ([Esempio](Use%20case%20diagram%20-%20Attori.png)):
- Umani o non umani.
- Primari se hanno benefici diretti dall'uso degli use case; secondari se non hanno benefici diretti.
- Attivi se usano gli use case (Solitamente primari); passivi se sono usati dagli use case (Solitamente secondari). Per specificare questo concetto si può: scriverlo accanto; posizionare gli attori attivi alla sinistra del sistema e gli attori passiva alla destra del sistema.
# Relazioni
### Attori-Use case
- Gli attori e gli use case sono connessi tramite linee, chiamate associazioni.
- Ogni attore deve essere associato ad almeno uno use case.
- Tra attori e use case si può avere una cardinalità.
- Un attore non deve avere un'associazione con due use case di cui uno estende l'altro o lo implementa, ma solo con il base use case.
### Use case-Use case
- Include
	La relazione di $include$ è rappresenta da una freccia aperta tratteggiata (Con scritto include) che va da $A$ (Base use case) a $B$ (Included use case): l'esistenza di $B$ è necessaria perché esista $A$; non specifica la sequenza delle azioni da eseguire.
- Extend
	La relazione di $extend$ è rappresentata da una freccia aperta tratteggiata (Con scritto extend) che va da $A$ (Extending use case) a $B$ (Base use case). Significa che $A$ è una specializzazione di $B$. Possono essere aggiunte delle note che indicano la condizione da verificare perché $A$ estenda $B$.