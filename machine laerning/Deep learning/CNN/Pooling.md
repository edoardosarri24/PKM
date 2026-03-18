Il polling è l'operazione usata insieme alla [convoluzione](machine%20laerning/Deep%20learning/CNN/Convoluzione.md) all'interno delle [CNN](Convolutional%20neural%20network%20(CNN).md).
Ha l'obiettivo di forzare la rete a catturare le rappresentazioni più significatiche (sematicamente, cioè quelle che caratterizzano quello che stiamo osservando); in pratica stiamo eliminando i dettagli superflui.
# Operazione
Osserviamo che il polling è un'operazione fortemente non lineare, a differenza della [convoluzione](machine%20laerning/Deep%20learning/CNN/Convoluzione.md) che è non lineare solo nell'activation function.
Il polling viene eseguito suddividendo l'input in finestre non sovrapporte o sovrapposte e poi eseguendo una delle seguenti [operazioni](polling.png).
- Sub polling
	Si prende un solo input, solitamente quello centrale.
- Max polling
	Prendiamo l'input con il valore massimo.
	Conserva meglio i dettagli più rilevanti (es: bordi) all'interno di una regione.
- Mean polling
	Prendiamo la media dei valori dell'input.
	Riduce il rumore e quindi è più adatto alle situazioni in cui si vuole l'informazione globale (es: riconscimento di scene) e non quella specifica.
##### Obiettivo
Si vuole costringere una [CNN](Convolutional%20neural%20network%20(CNN).md) a costruire una rappresentazione dell'immagine di più alto livello, cioè concentrarsi di più sui dettagli semantici dell'input.
##### Dimensionalità
Per eliminare i dettagli non importanti si riduce la dimensionalità dell'input, senza però modificare la profondità, cioè il numero di canali. Questo solitamente viene fatto considerando uno [stride](machine%20laerning/Deep%20learning/CNN/Convoluzione.md#Variazioni) maggiore di 1. Se lo stride è $n$ allora la dimensionalità viene ridotta di un fattore $n$.
Se ci sono più canali allora si ripete l'operazione per ogni canale.
##### Complessità
In questo modo si riduce la complessità computazione. Fare un polling che dimezza la dimensionalità dell'input, porta ad avere una convoluzione successiva che viene eseguita sulla metà degli input rispetto alla precedente.
Questo equivale anche a fare una convoluzione dove la finestra ha dimensione doppia, perché è come se venissero calcolati anche questi elementi che sono stati eliminati dal polling perché poco rilevanti.