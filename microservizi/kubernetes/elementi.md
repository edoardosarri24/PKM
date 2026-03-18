Per capire come funziona [kubernetes](microservizi/kubernetes/kubernetes.md) è necessario capire gli elementi che lo compongono e come vengono gestiti.
# Cluster
Un cluster è l'intero sistema kubernetes ed è composto da uno o più worker node, le macchine dove il cluster è in esecuzione, gestite dal control plane.
# Pod
Un pod è il blocco base, cioè l'unità più piccola che può essere creata, eseguita e gestita da kubernetes.
Si tratta di un insieme di container che condividono memoria, risorse di rete e una qualche specifica per la loro esecuzione. Sono sempre gestiti insieme: i container che formano un pod non saranno mai separati ed eseguiranno sempre su un worker node.
##### Sidecar
All'interno di un pod ci possono essere più container. Solitamente quello che si è mettere un application container, che implementa il codice relativo al servizio, e un sidecar.
Il sidecar è un container ausiliario che svolge alcuni compiti di decoro: logging, sicurezza e monitoring delle metriche ad esempio.
In questo modo si garantisce il [SRP](single%20responsability%20principle.md) tramite la modularità, indipendenza e il riutilizzo.
# Replica set
Un replica set è l'ente che si assicura che il corretto numero di repliche di un pod sia in esecuzione in ogni momento.
Permette in modo automatico di rilasciare una nuova replica se una di quelli esistenti va in crash. Permette inoltre l'horizontal scaling in modo efficiente.
# Service
Se Kubernetes non esistesse, ogni container (ogni componente distribuito dell'applicazione) dovrebbe avere un proprio IP per comunicare con altri componenti del clsuter o con il client. Questo con Kubernetes però non avrebbe molto senso perché i pod (i.e., le sue repliche) sono effimeri, cioè possono andare in crush ed essere ricreate e questo comporta un cambiamento dell'IP; ogni pod dovrebbe aggiornare il suo riferimento al nuovo pod.
Un service rappresenta un entry point per tutte le repliche del pod ed è caratterizzato da un IP e una porta che non cambiano mai. I pod o i client esterni che voglio utilizzare il servizio devono contattare tale entry pod definito dal service; è il service che decide quale replica chiamare in modo da gestire il loadbalance sul cluster di repliche del pod.
##### Tipi
Ci sono diversi tipi di service:
- ClusterIP
	Espone il service solo all'interno del cluster e non è possibile chiamare il service fuori dal cluster a meno di utilizzo di $Ingress$ (ma solitamente solo per test).
- NodePort
	Serve per esporre il service in ambienti di test o quando non è necessario un load balancer. Il service risponde sulla stessa porta per ogni nodo del cluster.
- LoadBalancer
	È il modo per esporre service in ambienti cloud. Viene eseguito un load balance sul carico.
# Deployment
Gestisce i pods e i replica sets per garantire aggiornamento e lo scaling.