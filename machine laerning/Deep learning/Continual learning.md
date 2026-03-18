Il problema del deep learning è che di norma si basa sulla staticità: si suppone che i dati e i task non cambino nel tempo e che (i dati) siano sempre disponibili.
Nella realtà però non è così: i dati vanno e vengono (es: per la privacy magari l'utente li cancella e non possono più essere usati) e i task vengono aggiunti e rimossi.
##### Possibili soluzioni
Una possibile soluzione è addestrare nuovamente ogni volta che un batch di dati o un task viene aggiunto o rimosso.
Questa soluzione ovviamente non è scalabile né rispetto alla disponibilità di dati vecchi né rispetto al costo del training.
# Tipi
Ci sono due tipi di continual learning.
- Task-IL
	Durante l'inferenza al modello viene fornito sia l'input che il task da risolvere.
- Class-IL
	Al modello viene fornito solo l'input e deve cercare di capire a quale task è relativo.
# Catastrophic forgetting
Supponiamo che un nuovo task si aggiunga per cui la rete è addestrata. Uno dei problemi principali di modificare i pesi di una rete già addestrata è quello che i pesi erano ottimali per il task precedente; i nuovi pesi saranno ottimali per il nuovo task, ma saranno variati rispetto ai task precedenti. Capita spesso che i pesi si modifichino così tanto che i vecchi task non hanno più una correttezza sufficiente.
# Tecniche
Ci sono varie tecniche per arrivare al continual learning eventando il catastrophic forgetting.
##### Weight regularizzation
Supponiamo di avere un modello già addestrato per un task $A$ e di voler aggiungere un task $B$.
Quello che vogliamo è variare i pesi raggiunti per l'ottimalità di $A$ senza allontanarci troppo da quella soluzione; in [questa](Catastrophic%20forgetting.png) situazione infatti i pesi finali sono ottimi sia per $A$ che per $B$.
Questo può essere raggiunto tramite un regolarizzatore detto Elastic Weight Consolidation (EWC). Nella nuova loss $L(\theta; D_A, D_B) = L_B (\theta) + \sum _iF_i (\theta_i - \theta_{A,i})^2$, oltre a ottimizzare la classica loss per il task $B$ si richiede di restare in una soluzione ottima per $A$.
##### Knowledge distillation
Nel Learning without Forgetting (LwF) si vuole che le predizioni del nuovo modello non si distacchino troppo da quelle vecchie.
Dato un nuovo task $B$ con i suoi input, si genera le probabilità per ogni classe di output con il vecchio modello. A questo punto di addestra il vecchio modello per il nuovo task combinando due loss: la prima è quella che ottimizza il nuovo task e sarà una classica cross entropy; la seconda, a cui aggiungiamo un regolarizzatore $\lambda$ che esplicita l'importanza del nuovo task rispetto a quello vecchio, impone che i risultati del nuovo modello per il nuovo task non si allontanino troppo da quelli del vecchio modello per il uovo task.