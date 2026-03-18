Un modello di [IR](information%20retrival.md) definisce:
- Il modello che rappresenta i documenti.
	In generale abbiamo: un insieme di documenti $D$; un vocabolario $V=\{t_1,\cdots,t_{|V|}\}$; a ogni termine $t_i$ presente nel documento è associato un peso $w_i$. Il documento $j$-esimo è quindi rappresentato da $d_j=\{w_{1j},\cdots,w_{|V|j}\}.$
- Il modello che rappresentare la query.
- Come definire la similitudine tra documenti e query, cioè come trovare i documenti che soddisfano la query.
# Boolean model
Il modello booleano è uno dei metodi più semplici.
I documenti sono rappresentati tramite la matrice di bag of words: è una matrice in $\{0,1\}^{|v|\times D}$, cioè le righe sono i termini del vocabolario e le colonne i documenti; è quindi una matrice binaria, cioè i pesi $w_{ij}\in\{0,1\}$, dove nella posizione $ij$ della matrice c'è 1 se il documento $j$-esimo contiene l'$i$-esimo termine, 0 altrimenti.
##### Query
Le query devo essere espressioni booleane.
Il risultato è ottenuto mettendo in $and$ le righe corrispondenti ai termini che compaiono della query; se un termine è preceduto da un $not$ (non deve comparire nel documento) allora prima di mettere la sua riga in $and$ si fa un bitwise not (si invertono i bit).
##### Conseguenze
- Abbiamo l'exact corresponce, cioè un documento o soddisfa la query o non la soddisfa.
- I documenti ritornati non possono essere ordinati (il matching tra query e documento è $true$ o $false$) secondo un qualche peso.
- Di solito vengono restituiti o troppi documenti o troppi pochi.
- Richiede la conoscenza dell'algebra booleana.
- La matrice può essere molto sparsa, soprattutto per documenti o vocabolari molto grandi.
# Vectorial space model
Il modello spaziale rappresenta un documento tramite bag of words in $\mathbb{R}^{|v|\times D}$, cioè con una matrice i cui elementi sono i pesi $w_{ij}\in\mathbb{R}$.
##### Pesi TF-IDF
I pesi sono definiti dal modello TF-IDF come $w_{ij}=tf_{ij}\cdot idf_i$, dove:
- $f_{ij}$ è la frequenza (il numero di occorrenze) del termine $i$-esimo nel documento $j$-esimo.
- $tf_{ij}$ è la normalizzazione di $f_{ij}$ rispetto all'occorrenza massima dei termini in quel documento, cioè $tf_{ij}=\tfrac{f_{ij}}{max\{f_{1j},\cdots,f_{|V|j}\}}$, dove $V$ è il vocabolario.
- $idf_{i}$ rappresenta quanto il termine $f_i$ è informativo (un termine raro ha un'entropia maggiore) e si definisce come $idf_i=log\tfrac{N}{df_i}$, dove $N$ è il numero di documenti e $df_i$ è il numero di documenti in cui compare il termine $f_i$. Questo valore è 0 se il termine è presente in ogni documento, mentre cresce più un termine è raro.
##### Query
Dalla query si costruisce un vettore di pesi nello stesso modo che abbiamo usato per i documenti, cioè $w_{ij}=tf_{ij}\cdot idf_i$.
I documenti ritornati sono quelli simili alla query; questa similudine è una distanza e si calcola con la [distanza del coseno](Distanza.md#Coseno).
##### Conseguenze
- Rispetto al modello booleano riesce a restituire i documento ordinati secondo similarità.
- I documenti ritornati possono essere filtrati secondo la similarità. Ad esempio possiamo volere solo i documenti che sono più simili di una certa soglia.