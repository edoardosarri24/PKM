Un sistema parallelo si distingue da uno [concorrente](parallel%20computing/concorrenza.md) sulla base di quanti thread sono in esecuzione in un istante: supponendo di avere un hardware con più processori (CPU), viene permesso a più thread di eseguire contemporaneamente.
# Motivazioni
Ci sono molte motivazioni per eseguire le cose in parallelo:
- Efficienza
	La maggior parte degli algoritmi sono sequenziali. Nel tempo la loro esecuzione è sempre stata più efficiente perché il clock delle CPU è sempre aumentato. Oggi i produttori hw pensano che siamo arrivati a un upper bound della legge di Moore per quanto riguarda la velocità dei transistor.
	Se non possiamo ottenere performace dall'esecuzione parallela allora possiamo ottenerla parallelizzando.
- Dati
	Rispetto a un algoritmo sequenziale si elabora una quantità di dati maggiore; questi devono essere [progettati](data-oriented%20design.md) nel modo corretto.
	Rispetto alla quantità di dati e alla lore relazione con le performance ci sono delle [leggi](performace.md#Leggi) interessanti.
- Hardware
	Nel tempo il numero di transistor per processore è aumentato. Questo ha permesso di avere più core a disposizione per singola CPU; abbiamo quindi un hw che nasce con la possibilità di eseguire operazioni in parallelo.
# Flusso di lavoro
Quando si vuole risolvere un problema usando il parallelismo è una buona pratica seguire un flusso preciso:
- Definire un algoritmo sequenziale che risolve il problema.
- Calcolare delle [metriche](parallel%20computing/metriche.md) di performance su tale algoritmo sequenziale.
- Capire cosa può essere parallelizzato o se ci sono delle parti che devono rimanere sequenziali (non tutto è parallelizzabile). Questo si può fare con tecniche di [decomposizione](decomposizione.md).
- Implementare il nuovo algoritmo parallelo.
- Calcolare le nuove metriche e capire se e di quanto abbiamo migliorato le performance.
# Consumi
In una CPU i maggiori consumi sono relativi al cambio di stato dei transistor. L'energia consuma è $E\propto f^2$, dove $f$ è la frequenza di aggiornamento dello stato dei transistor.
Questo vuol dire che spesso è conveniente, sia in termini di consumi che di velocità di esecuzione, avere più CPU lente rispetto a meno CPU ma più veloci.
# Flynn's Taxonomy
![flynn's taxonomy](flynn's%20taxonomy.png)
Ogni nome nella tassonomia di Flynn è dato da Single/Multiple e Instruction/Data.
- SISN
- SIMD
	Sono le operazioni perfette da eseguire nelle [GPU](GPU.md).o
- MISD
	Non ci sono esempi reali.
- MIMD
# Framework
I framework che permettono la parallelizzazione si dividono in due categoria:
- Impliciti
	È il framework che si occupa del ciclo di vita dei thread. Sono più semplici da gestire ma anche meno potenti.
	- [openMP](openMP.md)
- Espliciti
	Sono i più complicati ma anche i più potenti. Permettono di avere pieno controllo dei thread e del loro ciclo di vita.
	- Java thread.