Per implementare la [sincronizzazione](sistemi%20operativi/thread/sincronizzazione.md) in [C](C.md) abbiamo diverse tecniche.
# Mutex
Sono disponibili da C++11
##### Classi
- Mutex
	Implementano il mutex vero e proprio con i metodi $lock()$ e $unlock()$.
	- $std::mutex$
		È il lock classico. Può essere usato da tutti i gestori.
	- $std::shared\_mutex$
		Può funzionare sia come lock esclusivo che condiviso. È usato da $std::shared\_lock$ o $std::unique\_lock$.
- Gestori
	Gestiscono le chiamate ai mutex.
	- $std::lock\_guard$
		Il mutex è lock in tutto lo scope dell'oggetto e non può essere rilascaito prima.
	- $std::unique\_lock$
		Controllo esclusivo sul mutex ma con tutta la flessibilità che vogliamo: acquisizione e rilascio quando decidiamo noi. È l'unico che si può usare con le [condition variable](#Condition%20variable).
	- $std::shared\_lock$
		Accesso concorrente in sola lettura, la scrittura è bloccante.
##### Condition variable
Blocca i mutex su una condizione. La classe è $std::condition\_variable$. I metodi principali da chiamare su oggetti di questo tipo sono:
- $wait()$
- $notify\_one()$
- $notify\_all()$
# Semafori
I [Semaphore](sistemi%20operativi/thread/sincronizzazione.md#Semaphore) sono stati introdotti da C++20.
##### Classi
- $std::counting\_semaphore$
	Semaforo con un dato numero di risorse.
- $std::binary\_semaphore$
##### Metodi
I principali metodi sono:
- $acquire()$
	Decrementa e blocca.
- $release()$
	Incrementa di una o più unità il contatore del semaforo e rilascia.
- $try\_acquire()$
	Decrementa senza bloccare.