Oggi le applicazioni [distribuite](sistemi%20distribuiti.md) eseguono su server cloud dove si paga le risorse che si utilizzano: se un'applicazione gira per più tempo e occupa più memoria allora costa di più.
Java è noto per essere un linguaggio che impone tempi di start lunghi e occupa più memoria rispetto ad altri. Se dividiamo il processo applicativo nella parte di building e quella di running, quarkus pone l'attenzione sul processo di building, rendendo il processo di running snello e rapido.
# Definizione
Quarkus viene definito come *Kubernetes Native Java stack tailored for OpenJDK
HotSpot and GraalVM, crafted from the best of breed Java libraries and standards*. Vediamo nel seguito ogni parte della definizione e i problemi che risorve.
##### Kubernetes native
Quando si parla di Kubernetes native si intende un framework che ha come target per il deploy [kubernetes](microservizi/kubernetes/kubernetes.md). Fornisce quindi dei tool che permettono il building e il deploy in modo semplice e in un singolo step.
##### OpenJDK HotSpot and GraalVM
OpenJDK HotSpot è la JVM classica, quella che viene utilizzata quando eseguiamo un Java application sul nostro computer e che usa il compilare JIT (Just In Time).
GraalVM è invece un JDK (Java Developer Kit) ad alte prestazioni che accellera l'esecuzione di applicazioni scritte in java o in linguaggi JVM compatibili. Questo permette di eseguire il codice Java (in realtà anche di altri linguaggi) in modo più efficiente, di avviare le applicazioni Java in pochissimo tempo e di risparmiare memoria. Un ulteriore vantaggio è la compilazione nativa, cioè quello scenario in cui l'esecuzione non dipende dalla JVM a tun time; non si deve aspetta che carichi (circa 10sec) e il suo spazio di memoria non viene utilizzato.
Questi sono vantaggi che in un mondo cloud-oriented fanno ottenere a Java molta popolarità.
##### Java libraries and standards
Quarkus permette di integrare praticamente tutti le librerie che sono standard in Java, senza compromettere la facilità di compilazione sia nativa che usando la JVM.