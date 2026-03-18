L'interprete CPython impedisce il multithreading, garantendo solo la concorrenza tra [thread](thread.md): più thread potevano essere attivi e competere per la cpu, ma solo uno andava in esecuzione. Questo permette di nascondere la latenza ma non di avere un vero speed-up.
# Operazione I/O
L'unico momento in cui CPython permette il multithreading è quando si eseguono operazioni di I/O.
Il motivo è che queste chiamate eseguono chiamate di sistema che sono scorrelate dal python runtime e quindi non affliggono di sicuro il contatore gestito da GIL.
# Multiporcessing
L'unico modo per avere del parallelismo vero è usare il [multiprocessing](python%20multiprocessing.md), cioè avere più processi in esecuzione e quindi più interpreti e GIL diversi.
In questo modo i thraed di processi diversi non potranno parlare tramite shared memory, ma solo tramite IPC.
# GIL
Il Global Interpreter Lock (GIL) assicura che in ogni processo Python solo un thraed sia in esecuzione.
Questo è dovuto all'approccio safety del garbage collector, unico a livello di processo, cioè di istanza di interprete: ogni volta che un oggetto viene riferito da una nuova variabile o la variabile diventa inutile si incrementa o decrementa un contatore. Questo ovviamente deve essere fatto in modo atomico; per garantire in modo semplice questa atomicità, il GIL assicura che un solo thread sia in esecuzione.
##### Disattivazione
A partire da Python 3.13 è possibile usare interpreti diversi da CPython, quello rilasciato normalmente con conda, che permettono di disattivare il GIL.
È meglio per questi scopi avanzati usare il gestore di pacchetti UV, che permette di usare gli interpreti come IronPython e Jython.
