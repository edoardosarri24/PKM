Lo scaling in [kubernetes](microservizi/kubernetes/kubernetes.md) può essere fatto in due modi: con lo scaling verticale si varia il numero di risorse allocate a un'applicazione; con lo scaling orizzontale si varia il numero di repliche di un'applicazione.
Lo scaling può essere fatto sia manualmente che automaticamente.
# Vertical
Per modificare manualmente la quantità di risorse che un [pod](elementi.md#Pod) può utilizzare si deve modificare i campi $request$ e $limit$ nel file di configurazione del pod.
È importante osservare che questa modifica sarà eseguita solo sulle nuove repliche e che non andrà a influire su quelle esistenti già schedulate.
# Horizontal
Per modificare manualmente la quantità di repliche di un [pod](elementi.md#Pod) si deve modificare il campo $replicas$ nella definizione di replicaset o di deployment.
# Scaling automatico
Lo scaling manuale non è ottimale quando si hanno carichi molto variabili.
Kubernetes permette di fare di fare vertical pod autoscaling, horizontal pod autoscaling e node autoscaling; al momento lo scaling che funziona meglio è quello orizzontale sui pod.
Di default lo scaling viene fatto osservando l'utilizzo della CPU, ma è possibile definire anche se eseguirlo su altre metriche.
##### HPA
L'horizontal pod autoscaling varia automaticamente il numero di pod di un [replica set](elementi.md#Replica%20set).
Funziona tramite un controllo periodico (di default ogni 15 sec):
- Calcola la media di una metrica su ogni replica del pod. Di default questa metrica è la quantità di CPU utilizzata.
- Calcola il numero di metriche ottimale per soddisfare un valore target configurabile è $desideredReplicas=\lceil currentReplicas\times\tfrac{currentMetricValue}{targetMetricValue}\rceil$.
- Se il rapporto $\tfrac{currentMetricValue}{targetMetricValue}$ è vicino a 1 (di deafult la tolleranza è 0.1) allora non si varia nulla; altrimenti viene cambiato in modo automatico il campo $replicas$ per eseguire lo scaling.
- Nel caso in cui siano definite più metriche il calcolo è fatto per ogni metrica ed è considerato il numero di $desiredReplicas$ maggiore.
##### Flapping
Il flapping è quel fenomeno per cui il numero di $resideredMetricas$ varia frequentemente a causa della dinamicità della metrica stessa.
Per risolvere questo problema e non attivare e distriggere repliche in continuazione quello che si è prendere come $desiredReplicas$ attuale il suo valore massimo in una finistra di 5 minuti.
Questo permette che lo scaling up sia eseguito immediatamente, mentre lo scaling down sia diminuito solo se la riduzione di carico è stata confermata negli ultimi 5 minuti.
Ci sono anche delle altre contromisure al flapping che sono in beta: usare una finestra sia per lo scale up che lo scale down; rimuovere un numero massimo configurabile di repliche a ogni controllo; configurare la soglia per non eseguire lo scaling.