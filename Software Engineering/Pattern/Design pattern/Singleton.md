Si vuole assicurare che una classe abbia una sola istanza all'interno dell'applicazione e si fornire un punto di accesso a quell'istanza.
## SOLUZIONE
- Il costruttore deve essere privato in modo che la classe sia l'unica a poter creare istanze di se stessa.
- L'istanza da restituire è salvata in un campo privato, statico e dello stesso tipo della classe.
- L'istanza può essere creata o in fase di dichiarazione della classe o alla prima richiesta.
- La richiesta dell'istanza viene fatta con uno [[Static factory method]] convenzionalmente chiamato $getInstance$.
## OSSERVAZIONE
- I campi dell'oggetto non devono essere statici.