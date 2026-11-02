Le misure che si tirano fuori sono basate sulla distribuzione dei dati e non su informazioni che provengono dall'esterno (e.g., le etichette).
Solitamente queste misure si applicano al [clustering](Clustering.md).
# Coesione
Indica quanto sono coesi gli elementi di un cluster. Si può indicare con $cohesion(C_i)=\displaystyle\sum_{x,y\in C_i}proximity(x,y)$.
# Separazione
Indica quanto due cluster sono diversi e separati. Si può indicare con $separation(C_i, C_j)=\displaystyle\sum_{x\in C_i,y\in C_j}proximity(x,y)$.
# Silhouette Coeﬃcient
Combina insieme la coesione e la separazione.
Se vogliamo calcolare il coefficiente di siluette per un cluster $C_i$ dobbiamo fare la media di ogni $s_i$, dove $s_i$ è il silhouette coeﬃcient del punto $i$ ed è definito come $s_i=\tfrac{b_i-a_i}{\max(a_i,b_i)}$, dove:
- $a_i$ è la distanza media del punto $i$ con i punti del cluster. Questa esiste anche in spazi non euclidei perché non calcolo la media dei punti (che potrebbe non appartenere allo spazio), ma la media delle distanze, che è sempre definibile.
- Il termine $b_i$ è la minima distanza media del punto $i$ con i punti di ogni altro cluster $C_k$.
Osserviamo che $s_i\in[-1,1]$ e più il valore è alto melgio è perché vuol dire che la distanza tra punti del cluster è minore della distanza tra cluster diversi.