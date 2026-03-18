Nella programmazione [parallela](parallel%20pragramming.md), anche in problemi molto paralleli, a un certo punto i thread dovranno scambiare informazioni o dati; un esempio semplice può essere anche solo per sincronizzare i thread alla partenza e all'arrivo.
Per far comunicare thread dobbiamo guardare come è gestita la memoria.
# Shared memory
Nel meccanismo [shared memory](tipi%20di%20parallelismo.md#Shared%20memory) ogni thread può accedere alla memoria globale e trattare le variabili globali come se fossero proprie, oppure alle proprie variabili definite nel proprio spazio.
Quando si usano le variabili globali dobbiamo implementare qualche meccanismo di sincronizzazione.
##### Problemi
- Race condition
	Si verificando quando il risultato della computazione dipende dall'ordine in cui i thread eseguono. Siccome quando si eseguono operazioni in parallelo l'ordine non è deterministico 
# Distributed memory
Nel meccanismo [distributed memory](tipi%20di%20parallelismo.md#Distributed%20memory) i dati vengono scambiati tramite una qualche rete.
Ci possono essere comunicazione point2point o glbal bus.