Sono apporcci per limitare la [fault tollerance](fault%20tollerance.md)
Si vuole fornire un metodo per il rilevamento e la gestione dell'errore specifico per il caso in questione. Impone un design particolare tramite l'utilizzo di pattern.
# METODI
## Rilevamento dell'errore - [Checker](Rilevamento%20errori%20checker.png)
L'errore è rilevato da un checker dopo la computazione. Il checker può:
- Eseguire il calcolo attraverso un'altra implementazione (Solitamente meno complessa) in modo indipendente dalla prima. Il problema di questa soluazione sono i costi.
- Controllare che la forma del risultato sia corretta. Il problema di questa soluzione è che la copertura degli errori non è molto elevata.
## Correzione dell'errore
Lo stato non coretto si può correggere in più modi:
- Backword
	L'idea è quella di tornare in uno stato precedente in cui il sistema non aveva errori. Si fa quindi un rollback e si riesegue la computazione che ha portato al fallimento
- Forward
	Il sistema è consapevole di trovarsi in uno stato non corretto, ma continua a svolgere i suoi compito sperando prima o poi di risolvere il problema.
- Voting - [Voter](Rilevamento%20errori%20voter.png)
	La computazione è eseguita da più unità. Il voter fa passare il risultato che ha la maggioranza.
## Utilizzo della ridondanza
I moduli SW che sono ridondati possono essere eseguiti in parallelo o sequenzialmente, sullo stesso o su computer diversi. Nel caso siano eseguiti sulle stesso computer si perde un po' di diversity: se si ha un fallimento di un componente entrambi i SW ne risentiranno.
La ridondanza può essere utilizzata in due modi:
- Incondizionatamente, solitamente usato per il rilevamento degli errori e quindi sempre attivo.
- Su richiesta, solitamente usato per la correzione degli errori e quindi attivato solo se necessario.
## Granularità delle unità di fallimento
Un'unità di fallimento è una parte di software black box i cui risultati sono controllati per controllarne la correttezza.
La dimensione di tale unità (Quando si fanno i controlli, se alla fine a dopo ogni passaggio) è un aspetto fondamentale. Più è piccola più si riduce la latenza con cui ci si accorge dell'errore, si diminuisce la sua propagazione e si favorisce il recupero; aumentano i costi, in quanto implementare un rilevamento dell'errore su ogni unità è molto costosi.
# PATTERN SW
## Recovery blocks (RB) - [Schema](Fault%20tollerance%20recovery%20blocks.png)
Ci sono due implementazioni indipendenti, ognuna delle quali ha un proprio checker: la prima è sempre attiva, la seconda entra in funziona quando il checker della prima non è soddisfatto del risultato. Può esserci un watchdog timer per evitare di non restituire il risultato nel tempo richiesto.
- Ridondanza on demand.
- Rilevamento dell'errore tramite checker.
- Backward error recovery.
## N-version programmin (NVP) - [Schema](Fault%20tollerance%20N-version%20programming.png)
Ci sono N implementazioni uguali, ognuna delle quali è attiva e produce un risultato e c'è un voter che restituisce il risultato che ha la maggioranza. Il voter deve essere il più semplice possibile (Senza logica) in modo che il tasso di fallimento sia molto basso. Può esserci un watchdog timer per evitare di non restituire il risultato nel tempo richiesto.
- Ridondanza incondizionata.
- Rilevamento dell'errore tramite comparazione.
- Forward error recovery.
## N-serf checking programming - [Schema](Fault%20tollerance%20N-self%20checking%20programming.png)
Ci sono N implementazioni diverse. Una è attiva le altre sono in hot stand-by: non è in esecuzioni ma il suo stato è coerente con quello del componente attivo; in questo modo quando deve intervenire non c'è un delay time. Il servizio viene commutato quando il componente attivo ha un guasto; questo cambio corregge quindi anche l'errore.
- Ridondanza on demand.
- Forward error recovery.
## Scop - [Schema](Fault%20tollerance%20SCOP.png)
È l'unione dell'N-version programming e del recovery block: nella parte primaria ci sono implementazioni e un voter, nella seconda una sola implementazione che viene attivata in caso di guasto.
- Ridondanza on demand.
- Rilevamento dell'errore tramite comparazione.
- Forward error recovery.
# PATTERN HW
La ridondanza che abbiamo nel SW a livello di variante la possiamo trovare anche nel'HW a livello di linea.
## NVP/1/1
Ci sono N varianti SW in esecuzione su N linee HW. Il rilevamento dell'errore HW è fatto dal voter in base alla risposta del SW.
La versione più usata, quella con tre componenti SW in esecuzione su tre HW diversi, si ha:
- Tollera un guasto HW o SW.
- Rileva due guasti HW o SW indipendenti (Che sbagliano in modo diverso).
## RB/1/1
Le 2 varianti del recovery block (Con i rispettivi checker) sono duplicate su 2 linee HW. Possono anche essere triplicate, in modo che se una linea si rompe si rimane comunque con due.
- Tollera guasti diversi sulla stessa variante SW.
- Rileva guasti uguali tra varianti SW diverse.
- Tollera 1 guasto HW randomico; se è sistematico entrambe le linee lo avranno.
- Non rileva e non tollera guasti uguali sulla stessa variante SW.
## RB/2/2
Ci sono 4 varianti e 4 linee.
- Tollera 2 guasti indipendenti.
- Rileva 2 guasti collegati.
- Rileva 2 o 4 guasti indipendenti.