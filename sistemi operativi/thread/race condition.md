Il termine race condition è usato nel campo della sincronizzazione tra [thread](thread.md) per indicare quella situazione dove il risultato dipende dall'ordine con cui i thread eseguono.
# Motivazioni
Quando lavoriamo con più core nella computazione [parallela](parallel%20pragramming.md), non possiamo dare per scontato nessun ordine di esecuzione.
Le race condition nascono proprio da una supposizione di questo tipo: quando più thread accedono alla stessa area di memoria condivisa (detta critical section), non possiamo in nessun modo dire chi accederà prima.
# Sincronizzazione
Per eliminare la race condition dobbiamo garantire la proprietà [safety](proprietà.md#Safety) di mutua esclusione: solo un thread alla volta deve entrare nella sezione critica.
Questo può essere fatto tramite un meccanismo di [sincronizzazione](sistemi%20operativi/thread/sincronizzazione.md).