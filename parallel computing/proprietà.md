Nella programmazione [parallela](parallel%20pragramming.md) ci sono delle proprietà che sono importanti e a cui dovremmo ambire, o che non dovremmo raggiungere, quando realizziamo un sistema.
# Safety
È una proprietà che porta alla correttezza del programma. Indica che nulla di male succederà durante l'elaborazione del nostro programma.
##### Esempio
Se due processi devono usare una risorsa in comune, allora una proprietà di safety è che si deve realizzare la mutua esclusione, cioè i due processi non devono essere contemporaneamente in critical section.
# Liveness
È una proprietà che porta alla correttezza del programma. Indica che qualcosa di buono capitetà durante l'elaborazione del nostro programma.
##### Esempio
Se due processi devono usare una risorsa in comune, allora una proprietà di liveness è che non si deve presentare deadlock, cioè quando due o più precessi sono in attesa di una risorsa che l'altro possiede.
# Starvation freedom
Si verifica quando un task non accede mai a una risorsa perché viene scavalcato da un altro a priorità maggiore.
# Waiting
Si verifica quando un task è in attesa di una risosa che deve essere rilasciata da un altro processo.
##### Fault tollerance
Il problema è capire cosa succede se il task che deve rilasciare la risorsa fallisce. Come permettiamo a quell in attesa di eseguire?