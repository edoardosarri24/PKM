# Leggi
Nel mondo della [parallel pragramming](parallel%20pragramming.md) ci sono due leggi famose e note che ci danno delle informazioni sulle [metriche](parallel%20computing/metriche.md) di performace che un sistema parallelo può raggiungere.
##### Amdahl
È una legge del 1967 e ci dice che data la dimensione di un problema, cioè fissato il numero di dati da computare, c'è un upper bo sul grado di parallelismo, il quale porta a uno [speedup](parallel%20computing/metriche.md#Speedup) massimo di 20; dopo questo [upper bound](amdahl's%20law.jpg) incrementare il numero di core non porta nessun beneficio.
- Limiti
	Non considera tutte le varaibili che possiamo parallizzari, ma solo l'aumento del numero di processori.
	A suo tempo aveva senso perché i dati erano pochi, ma oggi i problemi diventano sempre più grandi in dimensione.
##### Gustafson
Questa legge affronta quello che nel mondo del parallelismo di chiama **scaling parallelism**: un sistema parallelo è scalabile se mantiene le stesse prestazioni all'aumentare della dimensione del problema aumentando il numero di unità di elaborazione. Ci mostra come la legge di [Amdahl](performace.md#Amdahl) non sia sempre valida che lo [speedup](parallel%20computing/metriche.md#Speedup) può crescere in [modo lineare](Gustafson's%20law.png) all'aumentare dei processori.
In pratica ci dice che anche se i dati aumentano, possiamo mantendere il tempo di computazione costante aumentando il numero di processori. Questo è molto più contemporaneo perché i la size di problemi oggi aumenta sempre di più.
# Downgrade
Quando eseguiamo la parallelizzazione dobbiamo cercare di non far decare le prestazione del programma.
##### Non parallelizable code
Nel nostro codice ci saranno (quasi) sempre parti che non possono essere parallelizzate. Questo è in accordo, come si vede [qua](non-parallelizale%20code%20performance%20limt.png), con la [legge di Amdahl](performace.md#Amdahl).
Data $f$ la parte che non può essere parallelizzata, anche se assegniamo la restante $1-f$ parte a infiniti processori, $f$ ci porta ad avere sempre dei limiti.
Si risolve parzialmente migliorando il codice non parallelizzabile.
##### Overhead
È un costo che viene introdotto nei sistemi paralleli che non c'è nei sistemi sequenziali. Sono esempi:
- Comunicazione
	La comunicazione tra task deve essere ridotta al minimo. È una buona norma eseguire una [decomposizione](decomposizione.md) in modo che essi siano il più indipendenti possibile.
	La comunicazione può avvenire in modo shared memory, come tra i core di una CPU, o distributed memoria, cioè tramite rete.
- Sincronizzazione
	La sincronizzazione è necessaria almeno quando più thread vengono creati e quando si devono sincronizzare in un punto.
- Decomposizione
	Quando si esegue la [decomposizione](decomposizione.md), i dati o thread devono essere suddivisi e gli deve essere assegnato il proprio compito.
- Località della memoria
	Quando abbiamo poca località spaziale o temporale, la gerarchia della memoria è un collo di bottiglia: se i dati non si trovano nella cache di primo livello, cosa che accade se abbiamo poca località spaziale o temporale, allora si deve andare nella cache di secondo livello e così via.
##### Contention
È la contesa dei processi per risorse condivise. Possono portare a ritardi anche unbounded.
##### Idle time
Se un processo finisce prima il suo compito rispetto ad altri allora quel processore rimane non usato.
# Upgrade
- Si deve ridurre le [dipendenze](dipendenze.md) dei processi (o [thread](thread.md)) che eseguono in parallelo. L'efficienza aumenta se i processi non hanno bisogno di essere controllati, coordinati o sincronizzati.
- Si devono individuare gli hotspot, cioè quei punti che beneficiano maggiormente dalla parallelizzazione. Infatti parallelizzare una parte di codice che lavora su pochi dati potrebbe non valerne la pena, mentre lavorare su una parte che esegue su moltissimi data potrebbe essere una buona idea.
- Identifica i bottleneck, come I/O, in modo da valutare la ristrutturazione del programma per migliorare queste parti.
- Granularity task
	Ogni volta che un core cambia task da eseguire abbiamo un overhead. I task devono fare abbastanza cose da limitare questo overhead.
- Scalabilità
	Usare una [data decomposition](decomposizione.md#Data%20decomposition) porta a un sistema più scalabile. Infatti la task decomposition ha dei [limiti](decomposizione.md#Task%20decomposition#Limiti).
- Complessità algoritmi
	Per risolvere ogni problema ci sono algoritmi che sono più complessi di altri. Siccome il nostro compito è fare le cose più velocemente, non ci interessa la pura complessità, ma più il completition time.
	Ci sono infatti algoritmi più complessi ma molto semplici di da parallelizzare; invece a volte gli algoritmi che hanno una complessità più bassa sono più complessi da parallelizzare.