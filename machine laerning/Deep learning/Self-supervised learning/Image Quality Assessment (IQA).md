Il task è, data un'immagine, determinare il ranking assoluto della qualità dell'immagine. Il problema è che non abbiamo dataset con immagini annotate di alta qualità (raw) e annotarle vorrebbe dire interrogare molti esperti e quindi sarebbe troppo costoso.
È quindi un contesto in cui il [Self supervised learning](Self%20supervised%20learning.md) è un'ottima soluzione.
# Proxy task
Il proxy task non vuole dare subito un ranking assoluto, ma più un ranking relativo, cioè ci vuole dire quanto un immagine è più accurata rispetto a un'altra.
##### Generazione samples
Il nostro obiettivo è di derivare, a partire un'immagine, una lista di immagini con qualità non superiore; non sappiamo quanto la nuova immagine è peggiore riserro alla precedente, ma solo che lo è.
Le tecniche per ottenere questo degrado ci sono moltissime tecniche (es: Gaussian blur, JPEG compression, Gaussian).
##### Architettura
L'architettura usata è quella delle [reti siamesi](Siamese%20net.md). Prendiamo una coppia di immagini di qualità diverse: la migliore la passiamo in una sotto rete e l'altra nell'altra sotto rete (ricordiamo che le reti siamesi sono due solo concettualmente).
A questo punto calcoliamo la loss e il modello ci dovrebbe solo dire che la prima immagine è migliore rispetto alla seconda, senza dare importanza alla qualità assoluta delle due immagini.
La loss che è stata usata è $L_{rank}=max\{0,\hat{c}(I_j)-\hat{c}(I_i)+\epsilon\}$, dove $\hat{c}(I)$ indica l'out dell'immagine $I$; in questo modo se la prima immagine ha un un output (cioè un ranking) inferiore quell'immagin non da controbuto alla loss.
# Downstream task
Il downstream task si occupa di fare fine tune dei parametri della rete addestrata con il proxy task tramite le poche immagini annotare. In questo modo si riesce a stabili un ranking assoluto.
La loss usata in questo caso è stata $L_{iqa}=\tfrac{1}{M}\sum_{i=1}^M(c_(I_i)-hat{c}(I_j))^2$.