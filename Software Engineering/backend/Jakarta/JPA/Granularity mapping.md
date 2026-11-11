Spesso quando costruiamo un [domain model](domain%20model.md) lo facciamo introducendo molte classi, alcune delle quali non esistono perché rappresentano effettivamente qualcosa, ma perché sono utili sono nel modello OO Java.
Vogliamo risolvere la questione della granularity, cioè trovare un modo per non dover generare le tabelle relative a queste classi, ma mapparle tramite [JPA](JPA.md) in altre tabelle già esistenti.
##### Esempio
Prendiamo [esempio di granularity](esempio%20di%20granularity.png) esempio.
Address è un attributo complesso e quindi vogliamo gestirlo tramite una classe a parte.
Potremmo però non voler che Address sia tabella a parte, magari perché ogni User ha esattamente un Adress; vogliamo quindi aggiungere i campi all'interno di Address nella tabella relativa a User.
# Embedding
[JPA](JPA.md) supporta il controllo della granularity tramite il meccanismo dell'embedding.
La classe di cui non voliamo una tabella (nell'esempio Address) viene annotata con $@Embeddable$; l'attributo che la riferisce è dichiarato come $@Embedded$. Un esempio di utilizzo è [questo](esemio%20Embedded.png).
##### Osservazioni
- Le classi che sono annotate con $@Embeddable$ dovrebbero essere prive di uno stato, cioè dovrebbero essere solo valori (value type). Inoltre solitamente il loro ciclo di vita è limitato da quello dell'entità che la riferisce.
- Questo schema è possibile se la classe che è composta (quella che dichiara il campo) ha un riferimento singolo all'altra. Se il rifeirmento è multiplo (cioè una collezione) allora is deve usare un [altro](Granularity%20mapping.md#Collezioni) meccanismo
# Collezioni
Quando una classe si riferisce a una collezione di un altro tipo il riferimento deve essere annotato con $@ElementCollection$, mentre al classe riferita deve essere ancora $@Embeddable$.
Possiamo aggiungere l'annotazione $@CollectionTable$ per specificare il nome della tabella della classe riferita e il nome della foreign key.
Quello che succede nel database è che ([esempio](embedding%20collection.png)):
- Nella tabella della classe entità (quella che ha il campo che si riferisce all'altra) non abbiamo nessuna informazione sulla collezione.
- Nella tabella relativa al tipo della collezione, oltre ai campi dei valori, compare una foreign key che si riferisce alla chiave primaria dell'altra tabella.
##### Osservazioni
- Anche in questo caso (come nell'[embedding](Granularity%20mapping.md#Embedding)) la claase riferita non ha solitamente uno stato, cioè non è un'identità, ma un value type. Il loro ciclo di vita è limitato da quello dell'entità che la riferisce.