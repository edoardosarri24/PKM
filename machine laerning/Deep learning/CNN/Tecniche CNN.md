Sono tecniche da usare nell'addestramento e sui dati per far funzionare le cose con le [Convolutional neural network](Convolutional%20neural%20network%20(CNN).md).
# Early stopping
Se il nostro dataset iniziale però viene suddiviso in due (trainsete e testset) e usiamo il testset per ricavare l'informazione sull'early stopping, allora non stiamo generalizzando su dati mai visti (portando all'overfitting), anche se il testset non è usato per ottimizzare i pesi. La soluzione è usare un trainset, testset e validation set.
Ci sono due tipo di early stopping.
- Vorremmo fermare l'addestramento quando l'errore sul validation set inizia a peggiorare, cioè quando raggiungiamo l'overfitting.
- Fermarci quando l'accuracy sul validation set smette di migliorare di una data tolleranza per un numero di epoche; il numero di epoche è detto patience.
# Data augmentation
La cosa miglior per regolarizzare i dati è ottenerne di più. Ci sono comunque varie strategie che possiamo adottare: una di qeste è la data augmentation.
Possiamo aumentare in modo subdolo i dati che abbiamo producendo da un immagine più immagini ruotandolo, traslando o facendo una riflessione in modo da limitare la dipendenza da caratteristiche spaziali, oppure modificare la scala RGB per ridurre la dipendenza dalla scala del colore.
# Weight regularization
È detta anche weight decay. Si aggiunge un regolarizzatore alla loss, cioè $L_{reg}(\mathbf{x},y,\mathbf{\theta})=L(\mathbf{x},y,\mathbf{\theta})+\lVert\mathbf{\theta}\rVert_i^i$, dove $i=\{1,2\}$.
##### L2
Stiamo chiedendo che i pesi non abbiano troppa varianza, cioè che non ci siano pesi molto piccoli e pesi molto piccoli.
Questo succede perché, se guardiamo il [grafico](weight%20regularization.png) (blu), se un peso è molto lontano da zero allora il gradiente lo porterà più velocemente vicino a 0 rispetto a uno che è già vicino a 0; in pratica stiamo chiedendo di dare più importanza all'ottimizzazione dei pesi molto diversi da zero.
##### L1
Stiamo chiedendo che il vettore dei pesi sia sparso, cioè che molti di questi siano zero.
Questo succede perché, se guardiamo il [grafico](weight%20regularization.png) (rossa), il gradiente è lo stesso per tutti i pesi. I pesi che inizialmente sono più vicini allo zero ci arrivaranno prima di quelli più lontani; non potendo essere tutti i pesi 0 allora si arrivarà ad una soluzione sparsa.
# Dropout
Un problema di una rete con molti neuroni è il fatto che neuroni vicini tenderanno ad apprendere informazioni molto simili, senza imparare cose molto diverse; in pratica la rete è molto profonda, ci sono molti parametri, ma non sta usando la sua intera capacità.
La tecnica consiste nello spegnere metà neuroni durante l'addestramento, in modo da permettere loro di apprendere senza l'aiuto degli altri. Durante l'infernza (a test time o quando si usa la rete veramente) invece vogliamo che tutti i neuroni siano attivi, in modo da usare tutta la potenza della rete. Questo si può fare rispettivamente con i metodi $\texttt{model.test()}$ o $\texttt{model.eval()}$.
Il problema del dropout è che diminuisce significativamente la velocità di addestramento,a rrivando anche a raddoppiarlo. È quindi buona norma non applicarla subito, ma quando siamo convinti che l'architettura della rete funziona bene e vogliamo aggiungere qualche punto di accuracy.
##### Extreme ensamble
Questa tecniche è detta anche extreme ensamble, perché se abbiamo $n$ neuroni possiamo avere $2^n$ modelli.
Il dropout permette anche di fare cose che senza non si possono fare.
# Normalizzazione
La normalizzazione è fondamentale per aumentare la velocità di addestramento ed è usata solo sui dati in input di un modello.
La normalizzazione avviene sottraendo la media e dividendo per la varianza:
- Ha l'obiettivo di mettere sulla stessa scala tutte le features, in modo che alcuni valori non siano molto grandi e altri molto piccoli.
- Evita che il modello dipenda molto dalla scelta iniziale dei pesi.
##### Regolarizzazione
Se si è usata la [regolarizzazione dei pesi](Tecniche%20CNN.md#Weight%20regularization) allora la normalizzazione dei dati potrebbe essere necessaria. Se i dati non sono normalizzati allora l'addestramento potrebbe assegnare pesi molto diversi alle variabili e questo influenza negativamente la regolarizzazione.
# Normaliation layer
Se tramite la normalizzazione si controlla la scala dell'input, la batch normalizzation mitiga i problemi in cui i dei layer interni sono su scale troppo diverse.
Quando un layer produce delle features si calcola la loro media e varianza, si normalizzano sottraendo tale media e dividendo per tale varianza e si passano al layer successivo: $\hat{x}=\tfrac{x-\mu}{\sigma}$, dove $\mu=\tfrac{1}{d_h}\sum_{i=1}^{d_h}x_i$ e $\sigma=\sqrt{\tfrac{1}{d_h}\sum_{i=1}^{d_h}(x_i-\mu)^2}$, dove $d_h$ è la dimensione del layer.
Se una rete ha più canali alloca questo viene fatto per ogni canale delle features.
Durante la back propagation il gradiente ci dice in quale direzione aggiornare i pesi di un layer con l'assunzione che l'input (cioè i pesi del layer precedente) non cambi; però questo non è quello che succede perché si cambiano prima i pesi del layer $i$ e poi del layer $i-1$. Usare la batch normalizzation permette all'aggiornamento dei pesi del layer $i$ di dipendere meno da quelli (che saranno modificati successivamente) del layer $i-1$.
##### Parametri
Durante l'inerenza i sample in input devono essere normalizzati secondo le statiche che la rete ha memorizzato durante l'addestramento.
Questo porta ad avere più parametri nella rete: se abbiamo $x$ canali allora averemo altri $2x$ (media e varianza) parametri in più.