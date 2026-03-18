È una direttiva [pragma](compilazione.md#Pragma) per [openMP](openMP.md) che permette di implementare la [coerenza](memory%20consistency.md#Coerenza) sulle variabili in un sistema [parallelo](parallel%20pragramming.md) e impedire troppe [ottimizzazioni](scrittura.md) sulla memoria.
# Funzionamento
Il significato della direttiva è: non restituire l'istruzione finché l'ultimo valore della variabile specificata non è scritto o letto dalla memoria e inoltre impedisci che le istruzioni relative alla pragma vengano riodinate.
Questo vuol dire che:
- Se un thread ha usato quella variabile ed essa è ancora nei registri della CPU del core, allora viene riscritto nella cache e aggiornato per tutti i thread che vogliono usare tale variabile.
- Se un thread sta leggendo quella variabile allora essa viene caricata dalla memoria in modo da prendere l'ultima versione disponibile.
##### Sincronizzazione
Un flush non vuol dire che i thread si debbano sincronizzare sull'accesso alla variabile, ma solo che se qualcuno ci accede in quel momento allora il valore acceduto sarà l'ultimo aggiornamento.
Se ad esempio dopo un flush il thread va avanti, modifica la variabile e la tiene nei registri, allora il thread che ci accede non vedrà l'ulitma versione.
# Direttiva
La direttiva è $\#pragma\ omp\ flush(var)$.
- Se vogliamo specificare una lista di variabili possiamo usare $\#pragma\ omp\ flush(list)$.
	In questo caso l'operazione non ritorna finché tutte le variabili non sono state flushate. Se abbiamo più variabili da flushare e non usiamo la lista ma più direttive, allora queste pragma possono essere riordinate e questo può provocare una [race condition](race%20condition.md).
- Se vogliamo specificare tutte le variabili possiamo usare $\#pragma\ omp\ flush$.
# Flush implicito
Un flush implicito è fatto ogni volta che:
- Si entra o si esce da una sezione [parallela](parallel%20region.md), [critica](parallel%20computing/openMP/sincronizzazione.md#Critical) e [atomica](parallel%20computing/openMP/sincronizzazione.md#Atomic). Se in questi casi si usa $nowait$, allora il flush non viene eseguito.
- Dopo una [barrier](barrier.md) che sia esplicita o implicita.
# Casi d'uso
Solitamente il [flush implicito](flush.md#Flush%20implicito) è sufficiente in tutte le situazioni e non serve gestire a mano la coerenza di una variabile tra thread, cosa comunque abbastanza complessa.
##### Problema distributo
Un possibile caso d'uso è quando abbiamo un compito da svolgere in parallelo che deve terminare quando un thread lo ha risolto. In questo caso i vari thread possono continuare finché il valore di un flag non è settato a 1. Per gestire che il valore del flag non resti nel registro del thraed che lo aggiorna allora dobbiamo usare il flush.