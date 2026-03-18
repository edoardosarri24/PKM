La complessità cognitiva ha lo scopo di essere usata con uno scope di metodo, di modulo o addirittura di sistema, a differenza della [cyclomatic complexity](cyclomatic%20complexity.md) che sappiamo avere senso solo con uno scope di metodo.
- [cognitive complexity - Sonar 2023](cognitive%20complexity%20-%20Sonar.pdf)
# Obiettivo
L'obiettivo della cognitive complexity è tiare fuori dal codice una metrica che rifletta l'understandability, cioè che permetta di capire quanto uno sviluppatore fatichi a comprendere, leggere manutere un certo codice.
##### Classi
Visto l'obiettivo la complessità cognitiva può anche essere usata per valutare la manutenibilità di una classe.
Lo score infatti non aumentare per un gran numero di getter e setter, se ad esempio la classe è una classe di un domain model.
# vs cyclomatic complexity
Può succedere che due metodi abbiano la stessa [complessità ciclomatica](cyclomatic%20complexity.md) ma uno ha una complessità cognitiva molto più bassa.
La complessità cognitiva non si basa su modelli matematici, ma cerca di rappresentare quanto è stato contorto il pensiero di un programmatore per gestire il flusso del codice.
# Regole
In generale si vuole favorire regole di clean code.
##### Shorthand
La complessità cognitiva ignora le caratteristiche che rendono il codice più leggibile.
Un esempio è racchiudere più istruzioni in un unico metodo con un nome rilevante per il contesto.
##### Brecks in the linear flow
Quando il codice ha un'interruzione lineare nel suo flusso allora si deve  compiere uno sforzo per capire cosa sta succedendo. La ricorsione viene considerata al punto successivo.
La cognitive complexity viene incrementata con gli operatori:
- $for$, $while$, $if$, $else\ if$
	Aggiungono uno. In questo momento non si considerano annidamenti.
	Il motivo per cui non si danno punti appunti a un $else\ if$ è perché questa è uno struttura che per un programmatore non aggiunge complessità.
- $catch$
	Un catch aggiunge 1. I $try$ e $finally$ non aggiungono nulla. In pratica viene considerato come un singolo incremento strutturale.
- switch
	Uno $switch$ con tutti i suoi $case$ aggiunge uno ed è considerato come un singolo incremento strutturale.
	Questo  considerato 1 perché lo $switch$ da un punto di vista della leggibilità e della manutenibilità è una singola struttura. Invece una catena di $if-else$ deve essere letta e manutenuta un caso alla volta con ogni possibile valore.
- Logical operator
	Su una catena di operatori dello stesos tipo si aggiunge solo 1.
	Ogni volta che abbiamo sequenze di operatori diversi si aggiunge 1, perchè lo sforzo per capire questi è maggiore.
- ricorsione
	Si aggiunge uno per qualunque tipo di ricorsione.
	I motivo sono due: la ricorsione è una specie di loop è la Cognitive complexity penalizza i loop; i programmatori considerano la ricorsione come difficile da capire.
- jump
	Ogni istruzione di salto aggiunge 1: $goto$, $continue$, $break$.
	L'unica eccezione è fatta con $return$., che non aggiunge nulla.
##### Nested
Più una struttura di codice è profonda più è complessa da capire.
Per questo motivo, a prescindere dal tipo di elmento, ogni livello di profondità aggiunge 1 a quanto aggiungerebbe l'elemento stesso.