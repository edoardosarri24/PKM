Si vuole risolvere il problema di trovare insiemi simili, cioè la cui distanza sia minore di una data soglia. Rispetto al [clustering](Clustering.md), in questa situazione consideriamo moltissimi oggetti (miliardi) multidimensionali con molte dimensioni.
L'esempio che faremo è quello di trovare documenti simili, ma si può applicare a qualunque tipo di oggetti. Il processo segue [tre step](step.png).
##### Bag of words
I documenti possono essere rappresentati in molti modi.
Un modo è il bag of words: si ha una matrice dove le colonne rappresentano i documenti e le righe tutte le possibili parole; nella posizione $ij$ abbiamo un 1 se nel documento $j$-esimo compare l'$i$-esima parola del vocabolario.
Il problema sta nella dimensione della matrice e nella sua sparsità; si potrebbe ridurre la dimensione memorizzando solo le parole importanti ma ancora sarebbe troppo grande.
Un altro problema è che se vogliamo trovare documenti simili l'ordine delle parole è importante, ma con bag of words non si considera.
# Shingling
In questa fase si vogliono convertire documenti in insiemi.
Un $k$-shingle (o $k$-gram) è una sequenza di $k$ token all'interno del documento; i token possono essere caratteri, sequenza di più caratteri o stringhe. Le varie sequenze sono considerate sovrapposte: abbiamo una finestra di dimensione $k$ che scorre con uno stride di 1.
È importante scegliere bene $k$. Se consideriamo $k$ il numero di caratteri all'interno della finestra, allora:
- Per $k\to1$ allora tutti i documenti saranno uguali, perché tutti i documenti conterranno tutte le lettere del dizionario.
- Per $k\to\infty$ due documenti sono simili se sono esattamente uguali.
##### Bag of Words
Costruiamo la tabella degli shingles a partire dalla rappresentazione bag of words. Nella nuova tabella avremmo sulle colonne i documenti e sulle righe i possibili shingles; con un bit indichiamo se un dato shingle è presente in un documento.
Questa tabella, come quella delle bag of words è ancora troppo grande e sparsa.
##### Hashing
Il metodo ([esempio](esempio%20rappresentazione%20shingles.png)) usato per la rappresentazione degli shingles è usare una matrice mettendo nelle colonne i documenti e in ogni riga una posizione di un vettore binario ottenuto come segue:
- Per ogni documento consideriamo l'insieme degli shingles di cui è composto.
- Per ogni shingle calcoliamo una funzione hash che restituisca un intero, che sarà l'indice del vettore.
- Poniamo a 1 le posizione del vettore che sono state restituite dalla funzione hash.
##### Similarità
Per valutare la similarità di due documenti, che adesso sono insiemi, si usa la distanza di [Jaccard](Distanza.md#Jaccard), in cui $Sim(c_1,c_2)=\tfrac{|c_1\cap c_2|}{|c_1\cup c_2|}$. L'idea è che duo documenti sono simili se hanno molti sequenze di dimensione $k$ in comune, anche se non nell'o stesso ordine.
A seconda del mondo in cui si rappresentano i documenti si hanno due implementazioni:
- Shingles
	Se abbiamo due documenti rappresentati come gli insiemi degli shingles, allora l'intersezione e l'unione sono quelle classiche.
- Binario
	Se abbiamo i documenti rappresentati come vettori binari ottenuti dopo l'hash, allora l'unione corrisponde al bitwise OR e l'intersezione al bitwise AND.
# MinHashing
Il problema di quanto ottenuto al passo sopra è che se confrontiamo solo 2 documenti la similitudine di Jaccard è veloce, ma per molti documenti il tempo è troppo grande e ci serve fare altre cose.
Vogliamo convertire grandi insiemi (un documento definito da shingles) in piccole firme, cioè una loro rappresentazione molto compatta, in modo che mantengano la similarità: se $c_1$ e $c_2$ sono documenti simili anche $h(c_1)$ e $h(c_2)$ devono essere simili. È un contesto in cui vogliamo fare una [dimensionality reduction](dimensionality%20reduction.md).
Siamo arrivati ad avere un insieme di documenti rappresentato in una matrice dove sulle colonne abbiamo i documenti dove ogni colonna è un vettore di bit ottenuti dopo l'hashing degli shingles.
##### Permutazione
Definiamo una permutazione $\pi$ che permuta le righe della matrice. Indichiamo con $h_\pi(c)$ l'indice della riga del primo 1 che si incontra lungo la colonna $c$ nella matrice permutata con $\pi$; in pratica questo è il primo. 1 che si trova percorrendo la colonna $c$ dall'altro verso il basso.
Si crea un vettore riga che contiene nella posizione $i$-esima i valore $h_\pi(i)$; cioè questo vettore riga contiene per ogni documento l'indice del primo 1 che si trova percorrendo la colonna del documento in questione. Questo vettore sarà lungo quanto il numero di documenti.
Si fanno 100 permutazioni diverse e per ognuna si crea una riga. Otteniamo così una matrice piccola e non sparsa con tante colonne quanti i documenti e ogni riga è una firma di quel documento.
A questo punto si calcola la similarità di due documenti come il numero di volte in cui la firma di quei due documenti è uguale sul numero totale di permutazioni (cioè 100). In questo modo la similarità di Jaccard (che era troppo complessa da trovare) sarà il valore vero, e il valore che troviamo è la sua approssimazione.
##### Proprietà
La probabilità che la funzione minhash per una permutazione casuale delle righe produca lo stesso valore per due insiemi è uguale alla similarità di Jaccard per questi insiemi.
Se $\pi$ è una permutazione allora $Pr\{h_\pi(c_1)=h_\pi(c_2)\}=Sim(c_1,c_2)$.
Per la dimostrazione vedi slides 483.
##### Implementazione
Permutare più volte le righe di una matrice è computazionalmente costoso; per realizzarlo in modo efficiente si usa l'hash.
Definiamo una funzione hash che permuta come $h^p:(0,\cdots,n-1)\to(0,\cdots,n-1)$, dove $n$ è il numero di righe della matrice; ci saranno delle collisioni ma se il numero di shingles è alto non ci interessa.
Possiamo ora usare l'hash generale per trovare le nostre 100 permutazioni dello stesso tipo con $h_{a,b}(x)=((a\cdot x+b)mod\ p')mod\ n$, variando $a$ e $b$, che sono sono interi casuali, e mettendo $p'$ primo con $p′>n$.
##### Miglioramento
Fino a ora si è fatta la permutazione della matrice; in teoria con la permutazione, in pratica con la funzione hash.
In realtà non mi serve memorizzare tutta la matrice permutata: quello che effettivamente voglio è mettere nella matrice finale che contiene le firme l'indice della prima riga in cui dopo la permutazione compare un 1.
L'input dell'[algoritmo](alg%20minhashing.png) è la matrice che contiene i documenti sulle colonne e il vettore di shingles hashati. Si inizializza tutto a infinito perché se questo valore non cambia vuol dire che non entro mai nell'$if$ e quindi tutta la riga o tutta la colonna è 0, cioè quel documento è vuoto o quello shingles non è mai presente.
# Locality Sensitive Hashing
Adesso abbiamo una matrice $M$ in cui per ogni documento (le colonne), abbiamo un certo numero di righe, ognuna delle quali è una firma del documento; confrontare le firme di ogni coppia di documenti per trovare documenti simili entro una certa soglia $s$ avrebbe un costo quadratico, che è troppo grande.
Il Locality Sensitive Hashing (LSH) è un metodo che usa l'hash per valutare l'uguaglianza delle righe in modo lineare sulla dimensionalità della matrice.
##### Funzionamento
Si divide la matrice $M$ in $b$ bande, ognuna formata da $r$ righe.
Per ogni banda si applica una funzione hash in modo che ogni colonna sia mappata in una cella di un vettore lungo $k$; la chiave $k$ deve essere la più grande possibile; ogni banda ha la sua tabella perché si vogliono valutare le bande in modo indipendente l'una dall'altra; si può usare la stessa funzione hash per bande diverse.
Due colonne sono candidate a essere uguali se il loro hash è lo stesso, cioè se finiscono nella stessa posizione della tabella hash relativa alla banda in considerazione.
##### Trade off
Quello che vogliamo fare è limitare il numero di falsi negativi e di falsi positivi: non vogliamo che documenti uguali siano considerati diversi (priorità); non vogliamo che documenti diversi siano cosiderati come uguali (meno importante perché si possono scartare in una fase di analisi successiva).
Questo trade off viene bilanciato tramite la scelta di $b$ e $r$.
Supponiamo che vogliamo etichettare due documenti come uguali se $Sim(C_1,C_2)>t$; possiamo definire un grafico dove sulle ascisse abbiamo la similarity di $C_1$ e $C_2$ e sulle ordinate la probabilità che due bande siano uguali data da $1-(1-t^r)^b$. Quello che otteniamo è [questo grafico](similarity%20vs%20prob%20equal.png): tramite il trade off, cioè tramite la scelta di $b$ e $r$ vogliamo limitare al massimo l'area arancione, cioè quella dei falsi negativi; la specie di asintoto verticale rappresenta la soglia con cui definiamo se due documenti sono uguali.