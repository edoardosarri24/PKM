È una direttiva [pragma](compilazione.md#Pragma) per [openMP](openMP.md). La direttiva è $\#pragma\ omp\ task$.
È utile per quelle situazioni in cui vogliamo parallelizzare un'operazione che è irregolare: ad esempio do$-while$ o algoritmi ricorsivi.
# Funzionamento
Quando un blocco ha prima questa direttiva quello che succede è che il codice e i dati necessari per la sua esecuzione vengono incapsulati in un oggetto Task. Poi questo oggetto finisce in una coda di task e viene prima o poi eseguito da un qualche thread.
Non abbiamo garanzia di quando verrà eseguito; è il runtime che decide quando prelevare un task dalla coda.
# Sincronizzazione
Siccome non abbiamo garanzie su quando un Task sia eseguito, ci sono due modi per garantire che tutti i task siano eseguiti in un determinato punto:
- Alla fine di una sezione parallela, cioè dopo il join.
- Usando la pragma $\#pragma\ omp\ taskwait$.
# Producer-consumer
L'utilizzo principale e più frequente è quello di implementare il pattern producer-consumer. Si definisce un blocco con la pragma [single](parallel%20computing/openMP/sincronizzazione.md#Single) o [master](parallel%20computing/openMP/sincronizzazione.md#Master); al suo interno si definisce il blocco Task, solitamente con loop in modo da creare più Task.
Quello che succede è che il singolo thread che esegue dentro il single sta produce task che finiscono nella coda; quando uno degli altri thread è libero prende questo task e lo esegue.