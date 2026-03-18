Le risorse che sono gestite di default da [kubernetes](microservizi/kubernetes/kubernetes.md) sono due:
- CPU
	È espressa in CPU units, dove ogni unità equivale a un CPU core fisico o virtuale.
	Sono permesse anche frazioni dell'unità. In questo caso si parla di milli CPU, cioè 0.1 CPU=100m CPU.
- Memoria
	È espressa in bytes.
# Definizione
Per definire le risorse che un pod deve utilizzare si utilizza un file YAML.
- $request$
	Indica le risorse che sono necessarie al container.
- $limit$
	È la quantità massima di risorse che contianer comsuma. Potrebbe essere anche maggiore del $request$.
# Pod scheduling
Per schedulare la replica di un pod su un nodo kubernetes utilizza l'informazione $request$ e non l'informazione $limit$: solo i nodi che hanno tante risorse non allocate quanto è indicato da $request$ vengono considerati per la schedulazione della replica del pod.
In questo contesto per risorse non allocate non si intende le risorse utilizzate sul nodo in questo istante, ma la somma di quelle richieste dei pod già schedulati su quel nodo. Può succedere infatti che in un istante una replica di un pod utilizzi più risorse di quelle richieste (al più quelle specificate in $limit$).
Può succedere che la somma dei valori $limit$ dei pod scheulati sullo stesso nodo sia maggiore del 100% delle risorse disponibili in quel nodo. In questo caso si parla di overcommitment. Kubernetes permette queste perché scommette che non tutti i pod contemporaneamente utilizzeranno più di quanto specificato in $request$.
# Overcommitment
Quando un pod o un nodo cerca di utilizzare più memoria o CPU ci sono vari contemporaneamente che kubernetes adotta. In ogni caso questa situazione è detta overcommitment.
##### Pod
Può succedere che una replica di un pod cerchi utilizzare più risorse di quelle che è autorizzato a utilizzare (definito in $limit$). In questo caso ci sono due possibilità:
- CPU
	La CPU è una risorsa comprimibile. In questo caso semplicemente si può ridurre la CPU a cui il processo ha accesso.
- Memoria
	La memoria invece è una risorsa non comprimibile. Quando un processo tenta di utilizzare più memoria di quella che potrebbe, il processo viene killato da kubernetes.
##### Nodo
Un nodo può raggiungere un utilizzo di memoria critico, cioè vicino al 100%, quando più nodi utilizzano contemporaneamente più memoria rispetto a quanto specificato in $request$.
In quest situazione si deve killare una replica di un pod e per decidere quale kubernetes permette di definire delle classi di QoS. Se più repliche appartenenti alla stessa classe di QoS devono essere killate allora si utilizza un OutOfMemory (OOM) score: viene killato prima chi spreca più risorse, cioè chi ha la differenza tra utilizzato e $request$ maggiore.
- Best-effort
	Sono quei servizi che vengono eseguiti solo se c'è disponibilità di risorse.
	Sono i primi che saranno killati.
- Guaranteed
	Ogni pod che vuole appartenere a questa classe di servizio deve specificare i valori $request$ e $limit$ con lo stesso valore.
	sono gli ultimi che saranno killati.
- Burstable
	Sono tutti gli alti pod, quelli che non appartengono alla classe best-effort o guaranteed.