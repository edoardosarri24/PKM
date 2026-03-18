L'[entity manager](Entity%20manager.md#Interfaccia) può prendere gli elementi da database con il metodo $find$ quando la chiave primaria è nota. Per recuperare una chiave prima non nota ci servono delle query che verificano determinate condizioni.
[JPA](JPA.md) supporta due modi per fare le query: direttamente tramite linguaggio SQL o tramite [JPQL](Java%20Persistence%20Query%20Language%20(JPQL).md).
# Creazione
Ci sono due interfacce per creare query sull'[entity manager](Entity%20manager.md).
- $Query$
	Il tipo di ritorno è una squery non tipizzata. Richiede un casting e può essere unsafe.
	Lo statment è $Query\ query=entityManager.createQuery("query")$.
- $TypedQuery<Type>$
	Ritrona una qeury tipizzata e solitamente è il metodo migliore.
	Permette di controllare il tipo a runtime.
	Lo statment è $Query\ query=entityManager.createQuery("query", Type.class)$.
##### Parametri nominali
In una query si possono definire parametri nomicali, cioè il cui valore reale sarà deifnito in un secondo momento tramite il metodo setParameter.
Si definisce un  parametro nominale con i $:$ sibito prima del nome.
- Un esempio è $entityManager.createQuery("from\ Order\ where\ user=:myuser")$ e poi si usa $query.setParameter("myuser", aUser)$. In questo modo $aUser$ è un'istanza vera e propria e quindi si prenderanno gli ordine di quell'utente.
##### Esecuzione
La query è eseguita solo a run e quindi il testing è necessario.
Viene eseguita quando si chiama $getSingleResult()$ oppure $getResultList()$.
Il risultato sono oggetto/i di tipo $Object$ per le query non ti pizzate, o del tipo specificato nel caso delle query tipizzate.
# Altri metodi
L'interfaccia Query permette anche di eseguire la cancellazione o l'aggiornamento delle entità sul database, ma questo è melgio farlo traite l'[entity manager](Entity%20manager.md).