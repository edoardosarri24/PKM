L'istante critico di un task è il tempo di arrivo di tale task che massimizza il suo [responce time](Real%20time/task.md#Definizioni).
##### Zona critica
Correlato all'istante critico abbiamo anche la zona critica, cioè l'intervallo temporale più stressante per un task. È definita come l'intervallo di tempo tra l'istante critico e la fine di quel job iniziato nell'istante critico.
È importante perché se il task riesce a completare all'interno della zona critica allora riesce a completare anche in altri intervalli, visto che durante la zona critica il processore è occupato a eseguire il task in esame e task a priorità superiore.
# Hard real-time
In un contesto di hard real-time, il responce time è calcolato usando tempi di esecuzione deterministici, cioè si utilizza il [WCET](timing%20analysis.md#WCET).
C'è un teorema di Liu and Layland che ci dice che l'istante critico del task in esame coincide con il momento in cui tutti i primi job dei task a priorità più alta arrivano contemporaneamente al primo job di quello sotto esame. Questo si può tradurre con: tutti i primi job di tutti i task arrivano nello stesso momento.
# Soft real-time
In un contesto dove il tempo di esecuzione è [probabilistico](timing%20analysis.md#Scenari) l'istante critico non è quello definito dal teorema di Liu and Layland. [Questo articolo](critical%20instant%20for%20Probabilistic%20timing%20uarantees%20refuted%20and%20revisited.pdf.pdf) (sezione $III.B$) mostra un controesempio in cui prendere un altro istante porta a un responce time maggiore di quello ottenuto considerando il classico istante critico.
##### Motivazione
Il problema di considerare il tempo di rilascio sincrono di tutti i primi job dei task sta nel fatto che stiamo ignorando come la variabilità dei tempi di esecuzione, dettata dall'approccio probabilistico, faccia accumulare ritardo rendendo un job futuro molto più a rischio rispetto ai primi. Questo avviene appunto perché il lavoro di un task a priorità più alta che passa da un periodo all'altro non è un valore deterministico ma una distribuzione.
##### Soluzione
La soluzione non è modificare l'istante critico del task ma cambiare la tecnica con cui si calcolare il responce time. Si utilizza sempre il tempo di rilascio simultaneo come istante critico, ma si utilizza la [probabilistic responce time analysis](probabilistic%20responce%20time%20analysis.md).