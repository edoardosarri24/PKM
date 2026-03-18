L'association mapping è una grande mismatch tra modello OO e modello relazione. Ci servono più informazioni rispetto a quelle che java fornisce.
Siamo nella situazione in cui nel modello relazione le associazioni (rappresentate dalla foreign key) non hanno una navigabilità, mentre nel modelo OO le associazioni possono essere monodirezionali, unidirezionali, one2one, one2many, many2one o many2many. Tutte queste informazioni vanno specificate tramite le annotazioni [JPA](JPA.md).
##### Bidirezionalità
È importante osservare che se nel modello OO la direzione è unidirezionale, basta specificare la corretta annotazione nel campo della classe source; se invece l'associazione è bidirezionale allora l'annotazione va inserita in entrambe le classi.
Nel caso delle associazioni direzionali, viene creata una tabella di associazione, che contiene le foreign key per entrambe le classi.
##### Persistant cascading
Quando si fa gestire un oggetto all'[entity manager](Entity%20manager.md) si deve stare attenti agli aspetti funzionali sulla persistenza degli oggetti composti: se volgiamo che la persistenza sia a cascata allora dobbiamo annotare con il parametro $cascade=CascadeType.PERSIST$.
Nel caso questo parametro non sia impostato e vogliamo la persistenza a cascata allora va fatto manualmente mettendo sotto il controllo dell'entity manager anche l'oggetto riferito.
# Many2One
Un oggetto della classe source si può riferire a un oggetto della classe target, ma un oggetto della classe target può essere riferito da più oggetti della classe source.
In questa situazione ([esempio](association%20many2one.png)) la classe source, che sarà quella che mantiene il riferimento, deve annotarlo con $@ManyToOne$.
Nel databse nella tabella relativa alla classe source verrà mantenuta una foreign key alla tabella della classe target. Se vogliamo specificare il nome della colonna relativa alla fereign key si deve usare anche l'annotazione fisica $@JoinColumn(name="name")$.
# One2Many
Un oggetto della classe source si può riferire a una collezione di oggetti della classe target, ma più oggetti della classe target possono essere riferiti da un solo oggetto della classe source.
In questa situazione ([esempio](association%20one2many.png)) la classe source, che manterrà il riferimento, deve annotarlo on $@OneToMany(mappedBy="field")$, dove il parametro $mappedBy$ si riferisce al nome dell campo nella classe target.
##### Cascade
È un parametro dell'annotazione che permette di gestire la propagazione sui figli delle operazioni; è quindi un aspetto funzionale del sistema, cioè dice ccome le cose sono fatte. In [questo esempio](esempio%20cascading.png) se elimino un User voglio eliminare anche tutti gli Order associati a esso, ma non gli Item del catalogo che appartenevano a quegli ordini.
Il parametro $cascade$, come in [questo esempio](cascade%20esempio.png), è direzionale e deve essere associato al source, cioè alla classe che possiede l'associazione.
Il parametro prende vari valori: Remove, Persist, Merge, All, etc.
# Many2Many
Sia la classe source che la classe target possono riferirsi l'una all'altra con una molteplicità maggiore di 1.
Entrambe le classi devono annotare il campo che mantiene il riferimento con $@ManyToMany$. Una delle due classi deve specificare il nome del paramentro in cui l'altra classe memorizza il riferimento con $@ManyToMany(mappedBy="field")$, la classe opposta eventualmente può specifiare il nome della join table e dei suoi campi con $@JoinTable(name="name_tabel",$ $joinColumns=@JoinColumn(name="own_name"),$ $inverseJoinColumns=@JoinColumn(name="other_name"))$.
# One2One
Sia la classe source che la classe target possono riferirsi l'una all'altra con una molteplicità pari a 1.
Si risolve con l'annotazione $@OneToOne$ sul campo che mantiene il riferimento ambo le parti.