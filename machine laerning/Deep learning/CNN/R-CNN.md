Se prendiamo [ResNet](ResNet.md), che ha usato Immagenet per addestrare i pesi, vediamo che abbiamo 1000 classi in output, ma solitamente non abbiamo bisogno di così tante classificazioni. Quello che però ci interessa è utilizzare i pesi addestrati, cosa che non possiamo fare perché non abbiamo l'hardware che ce lo permette.
Adattiamo ora queste reti a compiti di detection e di segmentation.
# R-CNN (Regionbased CNN)
 È una rete usata per compiti di detection che sfrutta reti già adddestrate come [AlexNet](AlexNet.md) o [VGGnet](VGGnet.md).
##### Flusso
 Data un'immagine in input la catena di operazioni che è stata fatta è:
 - Usare offline (fuori dal modello) una tecnica detta selections search. Questa permette di estrarre moltissime regioni candidate (RoI, Region of Interest), circa 2000.
 - Si ridimensionano le regioni in modo che abbiano dimensioni fisse e si passano alla CNN per estrarre le features map (cioè le caratteristiche).
 - Si usa un classificatore per etichettare le varie regioni e una regressione per raffinare le coordinate delle regioni.
##### Problema
- È estremamente lento, perché la rete deve essere utilizzata per ogni possibile regione.
- Non è un modello E2E perché si usa offline l'operazione di selection region.
# Fast R-CNN
È un'evoluzione della R-CNN.
##### Flusso
- Si passa l'immagine alla CNN per estrarre le features map.
- Usa la selection search (offline) per estrarre le RoI e le ridimensiona in una dimensione fissa tramite il RoI polling.
- Si passano le RoI a una rete FC che contemporaneamente etichetta le varie regioni e raffina le coordinate delle regioni.
##### Problema
- È estremamente lento a causa della selection search.
- Non è un modello E2E perché si usa offline l'operazione di selection region.
# Faster R-CNN
È un'evoluzione della fast CNN.
# Mask R-CNN
Ultimo modello finale per della lista che ha aggiuto ulteriore complessità a modelli già esistenti.