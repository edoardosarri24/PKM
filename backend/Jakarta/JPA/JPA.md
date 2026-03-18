Java Persistance Api (JPA) è la speficica [Jakarta](Jakarta.md) per l'[ORM](ORM.md). Si occupa quindi di definire come quali regole devono seguire un provider di ORM per lavorare con applicazioni Jakarta.
La sua implementazione più diffusa è Hibernate.
##### Master
Per fare le cose bene e come si deve si devono gestire anche aspetti più complessi:
- [Granularity mapping](Granularity%20mapping.md)
- [Inheritance mapping](Inheritance%20mapping.md)
- [Association mapping](Association%20mapping.md)
- [Collection mapping](Collection%20mapping.md)
- [Prestazioni](Prestazioni.md)
# Annotazioni
JPA si basa sulle annotazioni (oppure con file XML, ma è più complesso e inutile).
Le annotazioni JPA sono statiche: definiscono i metadati delle classi Java e specificano come le tabelle nel database devono essere create, cioè come eseguire il mapping tra oggetti Java e DBMS.
##### Specializzazioni
Alcune annotazioni sono delle specializzazioni di altre. In questo caso vengono scritte una dopo l'altra (stacked) ma devono essere interpretate come se fossero nested.
##### Tipi
Le annotazioni possono essere su tre livelli: classi, metodi e campi.
Ci possono essere inoltre altri due tipi di annotazioni:
- Le logiche descrivono le entità dal punto di vista del modello OO (es: $@Id$ o $@Entity$).
- Le fisiche descrivono le entità dal punto di vista del modello relazionale (es: $@Table$, $@Column$).
##### Accesso
Le annotazioni relative ai campi possono essere messe sopra i campi stessi (field access) o sopra i getter (property access). La scelta è personale, ma quello che è importante è mantenere la consistanza: o si mettono tutte sopra i getter o sopra i campi.
- Field access
	L'ORM accede direttamente agli attributi e i getter/setter non sono necessari. I campi devono essere $private$, $protected$ o $packege$.
- Property access
	L'ORM accede ai campi tramite getter e setter e questi sono quindi necessari e devono inoltre essere $pubblic$ o $protected$. Nonostante la specifica di JPA sia questa Hibernate permette di mantenere i getter e setter ancora $private$; se si pensa di usare solo Hibernate allora conviene porli come $pirvate$, se nel futuro si pensa di poter cambiare allora metterli $public$.
	In questo caso i getter/setter devono essere semplici e banali, cioè non devono fare nulla; siccome saranno usati ogni volta che si accede ai campi (dal mapper) anche le operazioni al loro interno saranno eseguite. Ad esempio se un setter cripta anche la password) che intendiamo essere un campo), allora questa sarà criptata ogni volta che l'ORM crea un oggetto, perchè il valore dell'oggetto sarà inizializzato tramite setter.
# Entity
È un'annotrazione logica. $@Entity$ specifica che la classe dovrà essere una tabella del DBMS ed è messa sopra la definizione della classe.
# Id
È un'annotazione logica. $@Id$ specifica che il campo rappresenta la primary key nella tabella. Il tipo del campo solitamente è un $Long$, ma questo non è obbligatorio. In quanto chiave primaria Id deve essere non nullo e immutabile.
La chiave primaria può essere di due tipi:
- Naturale
	È una stringa che ha un significato a livello business. Un esempio è il codice fiscale che è univoco a livello di business, ma potrebbe non esserlo per il sistema.
	Il problema di avere chiavi naturali è che potrebbero necessitare di essere cambiate oppure potrebbero non essere effettivamente uniche.
- Surrogata
	È una chiave sintetica non derivata dal dominio in cui opera il sistema. È generata dal database o dal sistema.
##### GeneratedValue
Quando vogliamo usare una chiave surrogata insieme all'annotazione $@Id$ dobbiamo usare anche $@GeneratedValue$ e il campo $Id$ dovrebbe (non necessario) essere di tipo $Long$.
Possono essere specificati dei parametri che specificano la strategia di generazione: auto, identity, sequence, table. Questi cambiano a seconda dell'implementazione di JPA e quindi si devono controllare all'occorrenza.
Spesso $@Id$ viene usato insieme (stacked) a $@GeneratedValue$ e questo indica che il valore non sarà inizializzato durante l'instanziazione dell'oggetto, ma sarà caricato dal DB; una conseguenza di questo è che ci sarà un momento in cui l'oggetto è creato ma non ha un Id. Per questo motivo, se si devono fare delle operazioni sull'oggetto che includono l'Id si usa un UUID (senza $@GeneratedValue$), che restituisce un Id (probabilmente) univoco a livello globale.
# Table
È un'annotazione fisica e si riferisce alla classe.
By default il nome della tabella è li stesos della classe con l'annotazione $@Entity$. Se vogliamo specificare il nome tramite stringa si usa $@Table(name="strnga")$.
# Column
È un'annotazione fisica e si riferisce ad un attributo.
By default il nome della colonna relativa ad un campo è quello del campo stesso. Se vogliamo specificarlo tramite stringa si usa $@Column(name="strnga")$.
# Basic
L'annotazione $@Basic$ serve per specificare che un campo è persistente, cioè che deve essere memorizzato nel DB.
Ha dei parametri che possiamo segnare: $optional$ se $true$ allora il campo in questione può essere $null$, altrimenti no.
È un'annotazione che introduce ridondanza, perché JPA in automatico considera un campo persistente; fetch se posto a lazy allora il dato è ripristinato solo quando necessario, se eager allora viene ripristinato subito.
# Temporal
I tipi data (es: $Date$, $Calendar$) Java non sono mappati in automatico come tali, ma serve l'annotazione $@Temporal$. Ci sono tre possibili formati:
- $@Temporal(TemporalType.DATE)$, che mappa solo la data (anno, mese, giorno), ignorando l'ora.
- $@Temporal(TemporalType.TIME)$, che mappa solo l'ora (ore, minuti, secondi), ignorando la data.
- $@Temporal(TemporalType.TIMESTAMP)$, che mappa sia la data che l'ora (anno, mese, giorno, ore, minuti, secondi, millisecondi).
# Enumerated
Quando una classe ha un riferimento di un tipo che è un $Enum$, questo viene mappato (cioè, il valore del campo che corrisponde all'enum è...) nel database tramite un intero che segue l'ordine con cui i tipi sono stati definiti all'interno della classe enumerazione. Questo comporta che se l'ordine nella classe cambia, allora dobbiamo fare questi cambiamenti nel DB manualmente.
Per evitare questo, e mappare il valroe come la stringa che definisce l'enumerazione, si deve usare $@Enumerated(EnumType.STRING)$.
# Lob
È un'annotazione che permette di specificare come gestire i dati molto grandi, il cui trasferimento incide sulle prestazioni. In JPA esiste solo $@LOB$ con determinati parametri, mentre in Hybernate invece esistono due annotazioni particolari:
- $@BLOB$ è per i dati binari, come foto, video e file. In pratica si usa se il dato è di tipo $byte[]$.
- $CLOB$ è per testi lunghi.  In pratica si usa se il dato è di tipo $String$.
# Transient
Di default tutti i campi sono persistenti, e quindi mappati nel data base. Alcuni di questi campi però necessita di essere transienti, cioè sono necessari solo per una determinata operazione (es: un nome completo necessario per il presentation layer e non per il backend; tutti i campi delle classi che implementano qualche pattern).
L'annotazione $@Transient$ può essere posta sia sopra i campi che sopra i metodi a seconda della strategia usata in tutto il resto del codice: se l'annotazione $@Id$ fosse sempre sopra i getter allora $@Transient$ andrebbe messo sopra il metodo; discorso inverso invece se nel codice l'annotazione $@Id$ fosse sopra i campi.