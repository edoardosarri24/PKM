OpenMP ([Open Multi Processing](https://www.openmp.org)) è un [framework implicito](parallel%20pragramming.md#Framework) per la parallelizzazione di codice in C/C++. È progettato per i sistemi dove ogni thread può accedere a qualunque area di memoria: siamo quindi in un ambiente [shared memory](tipi%20di%20parallelismo.md#Shared%20memory).
OpenMP non necessita di ristrutturare il codice sequenziale del nostro programma. L'idea è di aggiungere al codice sequenziale delle direttive che dicano al compilatore come parallelizzare il codice.
# Elementi
Il framework di omp è composto da più parti:
- Direttive
	È il core di omp: circa il 90% delle funzionalità si esprime tramite direttive.
	Si dichairano con le istruzioni [pragma](compilazione.md#Pragma) con la prima keyword che è $omp$.
- Funzioni
	Sono funzioni che hanno senso a runtime e che permettono di ottenere lo stato di un thread, modificare il numero di thread e cose di questo tipo.
	Siccome sono definite per il C non c'è nessu name space (dato dalle classi in OOP); per identificare queste funzioni c'è il prefisso $omp\_$.
	Esempi sono $omp\_get\_thread\_num()$ e $omp\_get\_num\_threads()$.
- Variabili d'ambiente
	Sono variabili globali che permettono di cambiare il comportamento di un programma senza ricompilazione; si può solo cambiare la variabile da terminale ed eseguire il codice.
# Compilatore
OpenMP ha bisogno del supporto del compilatore. Se il compilatore supporta OpenMP allora definisce la variabile $\_OPENMP$.
##### Compilazione
Per la compilazione il compilatore deve sapere che deve usare OpenMP, altrimenti ignora le direttive.
Per fare questo si deve:
- Installare le librerie.
- Usare il flag $-fopenmp$.
##### Versione
OpenMP è in evoluzione. Dobbiamo quindi controllare che la versione che stiamo usando sia supportata dal compilatore.
# Thread model
OpenMP implementa in modo nativo il pattern [fork-join](parallel%20computing/pattern.md#Fork-Join%20parallelism): di deafult c'è un main thread; per eseguire una parallelizzazione si deve eseguire prima un fork e poi un join, con [parallel](parallel%20region.md), per forza.
Per questo motivo non è possibile che ci sia un thread demone che continua a eseguire mentre il programma continua l'elaborazione.