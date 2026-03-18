Il problema dell'OOD consiste nell'ottenere una predizione sbagliata con alta confidenza di un input che non si è mai visto, cioè che non appartiene a nessuna classe del train set.
# Close word
Il problema principale è legato al concetto che la softmax tende a [massimizzare la confidenza](Calibrazione.md) rispetto a una delle $k$ classi definite e minimizzare tutte le alte. Anche quando l'esempio non dovrebbe appartenere a una delle classi definite la softmax tende a sceglierne ed ad essere molto sicura; non esiste quindi uno spazio aperto.
##### Soluzione
La soluzione a questo è introdurre la classe $k+1$-esima che rappresneta tutto il mondo su cui non abbiamo dati da addestramento.
Per prendere i dati di questa classe ci sono due soluzioni:
- Known data
	Si aggiungono campioni di training che consociamo. Solitamente il problema è che non bastano, cioè non rappresnetano bene cosa non conosciamo (perché appunto li abbiamo).
- Out-of-distribution synthetic data
	Si generano dati artificiali campionandoli da uno spazio latente che è lontano dalle distribuzioni note. Questo è fattibile con le GAN o le VAE.
# ODIN
È una prima baseline per risolvere il problema dell'OOD.
Si considera un esempio come out of distribution se la confidence con cui è stata predetta la sua classe non raggiunge una certa soglia.
Questa tecnica è molto poco robusta per la natura della softmax, che tende a essere molto confidence.
# Energy based model
Non producono direttamente probabilità per ogni classe, ma attribuiscono a ogni classe un'energia: se l'energia è bassa allora il campione proviene da una distribuzione normale, altrimenti proviene da un'OOD.
##### Funzionamento
L'energia viene attribuita in modo inverso rispetto alla somma totale dei logits: più alta è più l'energia del campione è bassa.
Usiamo poi l'energia come criterio di rifiuto: se supera una data soglia allora il campione viene etichettato come out of distribution.
Alcuni studi propongono di addetsrare la rete con la loss definita come $L = L_{CE} + \lambda\cdot L{energy}$, dove $CE$ è la cross-entropy.
##### Conseguenze
- È praticabile senza modificare la rete.
- Non si deve definire una $k+1$-esima classe.