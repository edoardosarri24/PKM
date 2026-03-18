L'entity manager è un componente software dell'[ORM](ORM.md) che ha il compito di gestire il contesto di persistenza: si occupa di gestire il ciclo di vita degli oggetti.
##### Jakarta
In [Jakarta](Jakarta.md) l'Entitt Manager è un'interfaccia definita da [JPA](JPA.md) e implementata da una qualche sua implementazione. Le annotazioni che JPA ci mette a disposizione per gestire il ciclo di vita degli oggetti in modo dichiarativo sono dette essere dinamiche, perché servono per definire quando i dati devono essere trasferito da e verso il database.
##### Caratteristiche
- Non costoso
	Di default utilizza la politica [lazy](Prestazioni.md) per il caricamento delle informazioni degli oggetti.
	Un entity manager tende a tener bloccato il database per il minor tempo possibile e per fare questo utilizza la cache: quando si ricerca un elemento nel database si ricerca prima nella cache e poi eventualmente si blocca il database.
- Non thredsafe
	Ogni utente dello stesso domain model deve avere il suo entity manager e non possono condividerne uno.
- Può avere un ciclo di vita più o meno lungo: si va dalla singola richiesta dell'utente all'intera durante dell'applicazione (essendo di solito web app questa è infinita).
##### Unit of Work
È un pattern adottato dall'entity manager per garantire che non si facciano operazioni sul database in continuazione.
Le operazioni sono eseguite solo alla fine della UoW, cioè quando si chiama $commit()$ sulla transizione.
# Entity manager factory
L'entity manager è un oggetto creato da metodi statici dell'EntityManagerFactory.
Mentre creare istanza di un entity manager è un'operazione che ha un costo trascurabile, creare una Factory di questo tipo comporta un costo alto perché necessita la connessione con il database. Questo è un problema relativo perché la Factory sarà un singelton il cui ciclo di vita sarà lo stesso dell'applicazione.
Un entity magaer può essere generato in due modi.
##### Standalone
È generato manualmente tramite un EntityManagerFactory. Si usa un metodo statico della classe $Persistance$ per creare la Factory e poi sull'oggetto restituito si chiama un metodo d'istanza per creare l'entity manager. Dopo la creazioni l'entity manager e la Factory devono essere chiuse con il metodo $close()$.
Le transizioni devono essere gestite manualmente: sull'entity manager si chiama $em.getTransaction().begin$ e poi $em.getTransaction().commit$. Nel mezzo si chiama $em.persist(newObjcet)$ poi eventualmente anche altre operazioni.
##### Container based
Il ciclo di vita dell'entity manager e delle transazioni è gestito da un container (un application server).
Si basa sul concetto di Dipendency Injection (DI) basato su EJB3.
In questo caso dovremo avere una classe, il cui campo entityManager sarà annotato con $@PersistenceContext$.
# Life clycle
L'entity manager (in Hibernate chiamato Session) è responsabile di controllare il ciclo di vita degli oggetti di cui è responsabile.
All'applicazione per funzionare basta che gli oggetti su cui fa operazioni siano in memoria principale e non gli interessa se gli oggetti sono persisti o no. Della persistenza se ne occupa l'entity manager.
##### Identity map
L'entity manager implementa quello che è l'identity map pattern: per ogni istanza (oggetto di una classe annotata con $@Entity$) che è sotto il suo controllo garantisce che in ogni momento ci sia una sola copia caricata in memoria principale.
##### Stati
Un oggetto può trovarsi in [quattro stati](lifecycle%20sotto%20l'EM.png), che determinano come l'oggetto è in relazione con il database (es: se e quando viene salvato):
- Transient
	È un oggetto presente in memoria principale, ma non è associato all'entity manager. Una modifica a quell'oggetto non influenzerà il database perché non ha un'id associato alla chiave primaria di un'entiry nel database.
- Persistent
	È un oggetto User che è stato associato all'entity manager tramite $em.persist(User)$. Ogni modifica su questo oggetto sarà caricato in memoria.
- Detached
	L'oggeto era persiste ma ora non è più associato all'entity manager; succede ad esempio quando l'entity manager viene chiuso. Se modifichiamo lo stato da qui in avanti le modifiche non si rifletteranno sul database.
- Removed
	Si tratta di un oggetto User persistente che dopo la chiamata $em.remove(User)$ è ancora in memoria ma sarà rimosso dal database dopo la chiusura della transizione, cioè dopo $em.getTransaction().commit()$.
# Interfaccia
L'entity manager usa metodi definiti nell'interfaccia specificata nel pacchetto $javax.persistence$. Questi metodi specificano come gli oggetti si muovono tra i vari [stati](Entity%20manager.md#Stati).
- $persist()$
	Un oggetto creato con $new$ è in stato Transient. Per metterlo sotto il controllo dell'entity manager si deve chiamare questo metodo e rendere quindi l'oggetto Persistant. È il modo standard per creare oggetti, cioè corrisponde a una $INSERT$ nel linguaggio SQL.
	Deve essere chiamato all'interno di una transazione.
- $getTransaction()$
	Recupera un oggetto transizione, unica per ogni oggetto EntityManager. Solitamente sull'oggetto restituito si chiama poi il metodo $begin()$ e $commit()$.
- $find()$
	Ricerca nel database un oggetto per chiave primaria (e per tipi, implicitamente cioè senza parametri); ritorna l'entità o $null$ se non esiste. Segue la politica lazy, ma si può specificare con le [annotazioni statiche](Prestazioni.md#Annotazione) di JPA.
	Si raccomando di eseguirla in una transazione; in questo caso l'oggetto restituito è nello stato Persistant; altrimenti nello stato Detached.
	Corrisponde a una $SELECT$ in SQL.
- $createQuery()$
	La query passata a $getResultList()$ o $getSingleResult()$ deve essere prima creata tramite $createQuery(String query)$.
	Gli oggetti ritornati da queste operazioni sono in stato Persistent.
- $remove()$
	Rimuoe l'entità dal databse. Deve essere chiamata all'interno di una transazione (transactions scope).
	L'entità rimossa va nello stato Removed e sarà eliminata alla fine dell'Unit of Work.
- $clear()$
	Pulisce lo stato persistenze, cioè tutti gli oggetti nello stato Persistant passano nello stato Detached.
- $close()$
	Chiude l'entity manager e deve essere chiamata alla fine di tutte le operazioni su di esso.
- $refresh()$
	Sovrascrive eventuali modifiche nello stato persistente (nella memoria principale) con lo stato dell'entità nel database.
- $merge()$
	Memorizza lo stato di un'entità nel database se il suo stato non è Persistace; in questo caso non viene fatto nulla e viene ritornato solo l'oggetto stesso.
	Ritorna un oggetto nello stato Persistant se lo stato era Detached.
	Deve essere chiamata all'interno di una transazione.