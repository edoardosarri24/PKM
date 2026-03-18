Solitamente la [mappatura](mapping.md) non viene eseguita da sola, ma è fondamentale anche che l'agente si muova nello spazio; si parla in questo contesto di Simultaneous Localization And Mapping (SLAM): l'agente mappa l'ambiente mentre si muove nel suo spazio. Svolgerle insieme permette di poter definire un trade off tra la massimizzazione dell'informazione acquisita e la minimizzazione del costo di spostamento.
Formalmente, data una sequenza di misure $Z(0),\cdots,Z(t)$ e una sequenza di input all'agente $u(0),\cdots,u(t-1)$, si vuole stimare la sequenza di stati $x(0),\cdots,x(t)$ dell'agente nella mappa $M$.
# Motivazione
![SLAM motivation](SLAM%20motivation.png)
I sensori danno un'informazione locale dello stato dell'agente e inoltre questa informazione può essere rumorosa perché i sensori non sono perfetti; la mappa è in fase di costruzione e quindi non la conosco totalmente; le osservazioni date dai sensori possono essere migliorate se consociamo la mappa.
Per questi motivi queste due operazioni devono essere svolte contemporaneamente.
# Esplorazione
L'esplorazione si basa sul concetto di frontiera: è l'insieme dei punti che il robot può raggiungere e che separano l'ambiente conosciuto da quello non noto; sono quei punti che l'agente deve raggiungere per acquisire nuove informazioni.
##### Single-robot exploration
Se abbiamo un solo agente allora in ogni istante di tempo $t$ questo deve decidere dove andare.
Per ogni punto della frontiera $p\in X$ si definisce un reward che dipende dalla posizione come $r(t,p)=g(t,p)-\alpha c(t,p)$, dove: $g(t,p)$ rappresenta l'informazione acquisita una volta arrivati nel punto $p$; $c(t,p)$ rappresenta il costo del viaggio da $x(t)$ a $p$ (possiamo dire semplicemente che $c(t,p)=\left\|x(t)-p\right\|^2$); $\alpha$ è il peso dell'interesse verso il costo del viaggio (0 se nullo, crescente altrimenti).
Un algoritmo greedy (si massimizza il reward istantaneo) è il seguente: per ogni istante di tempo $t$:
- Determina i punti di frontiera
- Seleziona la frontiera $p^*(t)$ che massimizza il reward $r(t,p)$.
- Vai a $p^*(t)$ usando un algoritmo di [motion planning](robot%20motion%20planning.md).
- Aggiorna la mappa mentre ti sposti.
##### Multi-robot exploration
Se abbiamo più agenti allora questi condivideranno una mappa (richiede uno scambio di informazioni) e in ogni istante di tempo $t$ l'agente $i$-esimo deve decidere la prossima posizione $p_i$ da raggiungere.
Il concetto è lo stesso del singolo robot: per ogni punto di frontiera $p\in X$ si vuole massimizzare il reward collettivo definito come $r(t,p_1,\cdots,p_N)=\displaystyle\sum_{i=i}^Ng(t,p_i)-\alpha\sum_{i=1}^Nc(t,p_i)$; per evitare conflitti solitamente si pone $\left\|p_i-p_j\right\|\ge\delta$. Questa massimizzazione può essere risolta tramite un problema di task assignment (e.g., algoritmo ungarico).
Un'altra tecnica è quella di usare le [celle di Voronoi](coverage.md#Definizione) e trasformare il problema in un problema a singolo robot.