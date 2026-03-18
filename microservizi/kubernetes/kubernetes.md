Prima di Kubernetes le applicazioni Java erano eseguite su [virtual machine](virtualizzazione.md#Virtual%20machine), ma questo consumava troppe risorse. Le VM erano gestite dagli amministratori e questo imponeva degli standard agli sviluppatori. Oggi si utilizzano i [container](container.md) per rilasciare applicazioni, i quali consumano molte meno risorse.
Un'applicazione a [microservizi](microservizi.md) può essere composta da molti container; al posto di utilizzare gli amministratori per gestire i micro servizi si utilizzano tool di orchestrazione, come Kubernetes. Esso permette di:
- Astrarre dall'infrastruttura sottostante.
- Automatizzare il deployment, la gestione e lo scaling.
- Aumentare la resilienza dei sistemi rilanciando quei componenti che sono falliti.
# Vantaggi
- L'horizontal scaling è gestito in automatico da kubernet secondo una qualche metrica (di default il carico sulla cpu) o si può specificare se modificare il numero di repliche.
- Semplifica il rilascio di applicazioni senza essere a conoscenza dei server nel cluster.
- Le repliche sono allocate in modo efficiente secondo le risorse di ogni worker node.
# kubernetes-native microservices
Quando si parla di kubernetes-native microservices si intende sviluppare micro servizi con l'idea che la piattaforma di deployment sarà [kubernetes](microservizi/quarkus/kubernetes.md).