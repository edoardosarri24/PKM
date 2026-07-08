Per implementare la [sincronizzazione](sistemi%20operativi/thread/sincronizzazione.md) in [C](C.md) abbiamo diverse tecniche.
# Mutex
La libreria lavora con tipi di dato $pthread\_mutex\_t$.
- $pthread\_mutex\_init$
	Inizializza il mutex.
- $pthread\_mutex\_lock$
	Acquisisce il lock o blocca il thread.
- $pthread\_mutex\_unlock$
	Rilascia e risveglia un thread.
- $pthread\_mutex\_destroy$
	Elimina il mutex.
##### Condition variable
Blocca i mutex su una condizione. Il tipo di dato è $pthread\_cond\_t$.
- $pthread\_cond\_init$
- $pthread\_cond\_wait$
- $pthread\_cond\_signal$
- $pthread\_cond\_broadcast$
	Risveglia tutti i thread.
- $pthread\_cond\_destroy$
# Semafori
La libreria sui [semafori](sistemi%20operativi/thread/sincronizzazione.md#Semaphore) lavora sul tipo di dato $sem\_t$. Le funzioni chiave sono:
- $sem\_init$
	Inizializza il contatore del semaforo.
- $sem\_wait$
	Decrementa il contatore. Il thread va in await se il contatore è 0.
- $sem\_post$
	Incrementa il contatore e risveglia un thread.
- $sem\_destroy$
	Libera la risorsa.