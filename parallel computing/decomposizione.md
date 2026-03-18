Quando si passa da un programma sequenziale a uno [parallelo](parallel%20pragramming.md) si deve ripensare al codice, dividendo le parti che possono essere eseguire in parallelo e quelle che non possono essere parallelizzate.
Questa rielaborazione del problema si divide in task decompositizon e data decomposition.
# Task decomposition
Si tratta di eseguire una decomposizione funzionale, dove ogni funzione è indipendente dell'altra; a questo punto i vari elementi sono eseguiti contemporaneamente in thread diversi.
Si deve far attenzione alla granularità dei task per avere [miglioramenti di performance](performace.md#Upgrade).
##### Assegnazione task
Una volta che il problema è suddiviso in funzioni indipendenti si devono assegnare ai vari thread. Ci sono due strade:
- Statica
	L'assegnazione è decisa prima di iniziare la computazione e non cambia durante di essa.
	- È ottimo se sappiamo esattamente gli execution time dei task e possiamo assegnarli staticamente.
- Dinamica
	Si definisce un'assegnazione iniziale (che può anche non essere completa, cioè ci possono essere alcuni task che non sono assegnati a nessun thread) e si permette che il sistema possa assegnarli in modo dinamico.
	- Lo svantaggio è che si ha un overhead per la gestione di strutture dati condivise, comunicazione tra thread e gestore dei threads.
	- Il vantaggio è che si permette il load balance: se un thread ha finito il suo compito allora può eseguirne un altro e non rimandere idle.
##### Limiti
Uno dei limiti principali della deocmposizione in task è la scalabilità del sistema: il numero di funzioni che possiamo scrivere per risolvere un problema è limitato; ci saranno in questo modo core che sono idle e quindi non sfruttiamo totalmente le risorse.
# Data decomposition
Si dividono i dati in più chunk e si esegue lo stesso algoritmo in più thread a ognuno dei quali è assegnato un chunk di dati.
È importante anche valutare il [layout](data-oriented%20design.md) dei dati in base alle operazioni che si devono affrontare.
##### Shape
La decomposizione dei dati può avvenire in vari modi e avere varie forme: per elemento (utile nelle GPU), per righe, per colonne o per blocchi.
La divisione dei dati determina i vicini di un chunk, quegli elementi che potrebbe essere necessario conoscere per eseguire il calcolo (e.g., convoluzioni). Quando si decompone i dati è bene ridurre i vicini di un nodo in modo che se fosse necessaria una qualche informazione il tempo di accesso potrebbe essere inferiore. Una regola empirica per fare questo è massimizzare il rapporto $\tfrac{volume}{area}$.
##### Asseganzione
Come per la task decomposition i chunk possono essere divisi o staticamente o dinamicamente.
##### Performance
Per avere miglioramenti sulle performance si può:
- Ogni thread deve contenere tutte le informazioni per eseguire sul chunk: se deve comunicare con altri thread si ha un overhead.
- Ogni thread deve elaborare su molti dati e quendi la grana della decomposizione deve essere caolcolata correttamente.