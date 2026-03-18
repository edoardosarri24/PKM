Un ORM (Object-Relational Mapping) è un paradigma di programmazione che mappa oggetti Java in entry di un database relazionale. Un sistema software si definisce come ORM quando mappa automaticamente classi Java in tabelle SQL, gestisce il ciclo di vita degli oggetti persistenti e traduce le operazioni sugli oggetti in query SQL reali.
# Implementazione
Creare un ORM è molto complesso: richiede la conoscenza di molti pattern, la scrittura di molte query SQL diverse, si devono risolvere i problemi di [mismatching](ORM.md#Mismatch), di concorrenza e altri aspetti complicati.
##### JDBC
Java ci mette a disposizione JDBC (Java DataBase Connectivity) come linguaggio di basso livello per interagire con la persistenza. Il suo utilizzo comporta di scrivere le query SQL a mano per ogni operazione e per ogni tipo di oggetto.
Questo è un vantaggio se stiamo ricercando la massima efficienza, ma è uno svantaggio in termini di complessità.
##### JPA
La specifica dell'ORM in Java è [JPA](JPA.md), che ci permette di lavorare con la persistenza in modo dichiarativo tramite annotazioni.
# Mismatch
Il problema del mismatching nasce quando cerchiamo di mettere in relazione il mondo OO con il mondo relazionale: alcuni aspetti del mondo Object Oriented hanno una mappatura nel mondo relazionale e viceversa.
##### Identity
In Java ci sono due modi per esprimere l'uguaglianza ci sono due modi: l'operatore di identità $==$ e l'uguaglianza $equals()$. Nel modello relazione invece questo concetto è dato dalla chiave primarie: due entry sono uguali se hanno la stessa primary key.
Il problema in questa situazione è quando abbiamo in memoria due oggetti diversi (secondo Java) ma che hanno al stessa primary key.
##### Granularity
Quando lavoriamo con un domain model ricco abbiamo molte classi, ma probabilmente queste saranno piccole; nel modello relazione invece la granularità è espressa solamente tramite la dimensione delle tabelle (righe e colonne).
Quando si mappano oggetti nel modello relazione e viceversa si devono tenere in considerazione questi aspetti tramite il [granularity mapping](Granularity%20mapping.md).
##### Inheritance
Nel modello relazione l'inheritance e il polimorfismo non si possono rappresentare in modo nativo, ma ci servirà una soluzione tramite l'[inheritance mapping](Inheritance%20mapping.md).
##### Associazioni
Le associazioni nel modello OO possono essere sia bidirezionali che unidirezionali (es: nel [Class diagram](Class%20diagram.md)); dovremmo eliminare le associazioni bidirezionali perché tendono a poter essere implementate in più modi e non possiamo specificare quale. Nel modello relazione invece le associazioni sono solo bidirezionali.
Per portare le associazioni unidirezionali del modello relazionale ci servono ulteriori informazioni, definite dall'[association mapping](Association%20mapping.md), non necessarie nel modello OO, come se queste sono one2many o many2many.