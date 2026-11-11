Per mappare il concetto di inheritance del modello a oggi nel modello relazione serve un po' di attenzione, visto che nativamente non sarebbe consentito. È un grande mismatch, ma non il peggiore perché abbiamo varie soluzioni; il lavoro sporco di queste soluzioni è in mano a [JPA](JPA.md) e non al programmatore.
##### Inheritance vs object composition
Nella programmazione moderna non si fa un grande uso dell'inheritance, ma più dell'object composition.
L'inheritance si dovrebbe usaro solo quando le spcializzazione della classe base differiscono effettivamente per il tipo. Da questo deriva il [Liskov sobstituition principle](Liskov%20sobstituition%20principle.md): dobbiamo usare questo meccanismo quando vogliamo poter usare la classe derivata al posto della classe base.
Se invece queste differiscono solo per un valore allora si dovrebbero usare altri metodi, come appunto l'object composition.
# MappedSuperclass
La situazione è quella in cui non si devono istanziare oggetti della classe base oppure questi non devono essere mappati nel database; questo permette di rendere la classe base astratta, che il caso d'uso migliore.
La classe base deve essere annotata con $@MappedSuperclass$ e le sottoclassi come $@Entity$; la classe basa può contenere il campo mappato come $@Id$, e allora questo sarà ereditato dalle specializzazioni.
##### Database
Quello che succede lato database è che non viene creata nessuna tabella per la superclasse; ogni sottoclasse avrà nella propria tabella tutti i campi definiti nella superclasse.
Il problema di questa soluzione sono le query poliformiche, cioè quelle query che re dati da tutte le sottoclassi. In questo caso non abbiamo alternati che scrivere una query per ognuna delle sottoclassi e poi unire i risultati ottenuti.
##### Conseguenze
Gli aspetti negativi sono il fatto che non possiamo fare query polimorfiche in modo automatico, più tabelle condividono campi che semanticamente hanno lo stesso significato.
Il vantaggio si ha quando la classe base è astratta. Il vantaggio è però quando vogliamo avere una mastersuperclass che gestisca gli aspetti comuni a tutte le entità, come ad esempio la gestione dell'id univoco.
# Table per class
Rispetto al caso MappedSuperclass qua la classe base, che sarà annotata con $@Inheritance(strategy=InheritanceType.TABLE\_PER\_CLASS)$ può avere una tabella nel database (in questo caso deve essere annotata anche con $@Entity$) e inoltre sono supportate le query polimorfiche tramite joins e union.
##### Conseguenze
Lo svantaggio che abbiamo ancora che più tabelle condividono campi che semanticamente hanno lo stesso significato. Inoltre possiamo fare query polimorfiche ma queste hanno un costo elevato a causa del prodotto cartesiano fatto dal join.
Il vantaggio è che possiamo fare delle query polimorfiche.
Un aspetto interessante è che non tutti i provider di JPA supportano questo tipo di mappatura; Hibernate lo fa.
# Single table
In questa soluzione la classe base è annotata con $@Inheritance(strategy=InheritanceType.SINGLE\_TABLE)$ e tutte le classi derivate sono unite in una sola tabella. Il risultato è una grande tabella sparsa, cioè dove molti valori sono $null$ con l'aggiuta di un campo in più che specifica il tipo dell'oggetto che è contenuto in quella entry. Se la classe base viene annotata con $@DiscriminatorColumn(name="type")$ allora possiamo scegliere il nome del campo che discrimina il tipo.
##### Conseguenze
Il vantaggio è che le query polimorfiche hanno la massima efficienza.
Lo svantaggio è legato alla sparsità della tabella. Se ci sono dei vincoli di integrità con cui specifichiamo che qualche entry non può contenere valori nulli, allora questo tipo di mapping non può essere usato.
# Joined table
È la soluzione più elegante. Si crea una tabella per ogni classe che contiene campi persistenti; le sottoclassi contengono solo i campi che dichiarano e hanno una foreign key che si riferisce alla tabella della superclasse.
Nella classe base si può aggiungere l'annotazione $@DiscriminatorColumn$.
I vantaggio principale è la normalizzazione del database.
##### Object composition
Uno dei vantaggi dell'object composition deriva proprio da questa situazione. Se invece di usare l'inheritance usiamo l'object compoition dove le classi base hanno un riferimento alla super classe, allora il mapping di joined viene in automatico.
L'unico problema che potremmo avere è quello in cui volgiamo nascondere le sotto classi a un client, permettendogli di usare i supertipo; per risolvere questo problema ci serve un'interfaccia comune che è implmetata dalle sottoclassi.