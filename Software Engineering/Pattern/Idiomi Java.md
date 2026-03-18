Sono meccanismi con cui fare le cose in un determinato linguaggio di programmazione. Sono strettamente legati al linguaggio di programmazione.
# Classificazione ortogonale
##### Analisi
Durante la fase di analisi per esprimere che una classe può avere più valori per ogni caratteristica in modo ortogonale (per ogni categoria la classe può assumera al più un valore) si può usare più generalizzazioni (inheritance) per ogni caratteristica.
##### Implementazione
Nella fase di implementazione questa cosa non è fattibile. Si usa invece l'inheritance con l'object composition: la caratteristica fondamentale (quella che definisce veramente la classe) è realizzata tramite inheritance; quella ausiliaria, tramite un riferimento a una classe astratta implementata poi da tante classi quanti sono i possibili tipi.
##### Esempio
Da [questa situazione](esempio%20-%20classificazione%20orotogonale.png) situazione, dove un CourseMaterial può essere English/Italian e Master/Undergraduate (in modo esclusivo) si passa ad avere una classe astratta CourseMaterial, con due implementazioni concrete Master e undergraduate e una classe astratta Language con due implementazioni concrete English e Italian; la classe CourseMaterial avrà un riferimento alla classe astratta Language.
# Attributi Enbedded
Siamo nella situazione in cui un attributo, che rappresenta uno stato, è troppo complesso. Allora si crea una classe che lo rappresenta e lo si embedda nella classe in cui appartiene, cioè si usa un'aggregazione forte (perché da solo non ha senso, ma serve l'esistenza dalla classe a cui si riferisce).
Vediamo un [esempio](esempio%20-%20attributi%20embedded.png).
# Mapper
Siamo nella situazione in cui una classe usa al suo interno più classi, cioè dove nell'implementazione avremo in campo riferito ad una classe esterna. La classe che aggrega le altre è detta Mapper.
##### Database
Questa soluzione è ottima per quanto riguarda l'uso del database. Solitamente le classi che sono aggregate contengono informazioni abbastanza persistenti, cioè che non cambiano nel tempo (o cambiano molto poco). Non aggregare tutto nella stessa classe permette di modificare solo le tabelle del database che non godono di questa proprietà (cioè che sono modificate spesso).
Da [questo esempio](esempio%20-%20mapper.png) osserviamo che le tabelle del database relative alle classi Course, Student e Professor saranno persistenti nel tempo, mentre la classe Exam cambierà spesso. In questo modo io riesco a variare lo stato della tabella del database relativa a Exam senza modificare quelle relative alle classe classi.
# Antipattern target
Sappiamo che i metodi dovrebbero essere messi nella classe giusto solo dopo che abbiamo il class diagram completo; lo possiamo fare basandoci sullo use-case.
In ogni caso i metodi non devono essere aggiunti per forza nella classe di chi compie l'azione, ma più eventualmente nella classe di chi subisce un cambio di stato. Un altra soluzione, che è la migliore, è quella di lasciare nelle classi del domain model solo i getter e setter e i metodi che compiono azioni si mettono nella business logic.
[Qua](esempio%20-%20antipattern%20target.png) vediamo un esempio. Un altro potrebbe essere quello in cui l'infermiera fa una somministrazione: il metodo non deve essere messo nella classe Nurse, ma al più in quella Patient perché lo stato cambia in Patient (nuova somministazione fatta con data e armaco); la soluzione migliore è comunque mettere il metodo nella business logic e usare le entità solo per cambiare stato.