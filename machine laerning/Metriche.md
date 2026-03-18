# Supervisionato
Verificare quanto un modello supervisionato funziona bene è abbastanza facile e universale.
##### Accuracy
È definita come il rapporto tra gli elementi correttamente classificati sul totale: $\text{Accuracy} = \frac{\text{True Positives} + \text{True Negatives}}{\text{Total Samples}}$.
- Indica quanto spesso il modello classifica correttamente le istanze.
- Si usa quando un dataset è bilanciato. Non è una buona misura per dataset non bilanciati.
##### Precision
Misura il rapporto tra le istanze correttamente classificate come positive sul numero di istanze classificate positive: $\text{Precision} = \frac{\text{True Positives}}{\text{True Positives} + \text{False Positives}}$.
- Indica la fiducia che possiamo dare al modello quando ci dice che un elemento è positivo:
	- Se abbiamo una precisione alta allora per quelle istanze che il modello classifica come positive possiamo essere sicuri che sia giusto.
	- Magari però ci sono altri elementi che dovevano essere classificati come positivi che il modello ha clclassificato negativi perché non era molto sicuro.
- Ad esempio non vogliamo che una mail buona sia classificata come spam, ma ci può andare bene se una spam è classificata come buona.
##### Recall
Misura il rapporto tra le istanze correttamente classificate come positive sul numero di istanze effettivamente positive: $\text{Recall} = \frac{\text{True Positives}}{\text{True Positives} + \text{False Negatives}}$.
- Indica quanto bene il modello riesce a trovare le istanze positive:
	- Se abbiamo una recall alta allora possiamo essere sicuri che il positivi siano stati classificati correttamente.
	- Però potrebbero essere classificati come positivi anche alcuni negativi per cui il modello non era sicuro che fossero negativi.
- Ad esempio non vogliamo che un paziente malato sia classificato come sano.
##### F-measure
È solitamente una media armonica tra precisione e recall: $\text{F1} = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}$.
- Indica una metrica che è un compromesso tra le due.
- È utile quando abbiamo dataset sbilanciati, dove devono essere considerati sia i false positive che i false negative.
##### ROC curve
Mette in relazione il False Positive Rate (FPR) con True Positive Rate (TPR), cioè la sensibilità.
Si vuole ottenere una AUC (Area Under Curve) grande, cioè una quasi funzione a scalino, dove in 0 è molto alto.
Si usa nella classificazione sbilanciata.
# Non supervisionato
Le misure che si tirano fuori sono basate sulla distribuzione dei dati e non su informazioni che provengono dall'esterno (es: le etichette).
Solitamente queste misure si applicano al [clustering](Clustering.md).
##### Coesione
Indica quanto sono coesi gli elementi di un cluster. Si può indicare con $cohesion(C_i)=\displaystyle\sum_{x,y\in C_i}proximity(x,y)$.
##### Separazione
Indica quanto due cluster sono diversi e separati. Si può indicare con $separation(C_i, C_j)=\displaystyle\sum_{x\in C_i,y\in C_j}proximity(x,y)$.
##### Silhouette Coeﬃcient
Combina insieme la coesione e la separazione.
Se vogliamo calcolare il coefficiente di siluette per un cluster $C_i$ dobbiamo fare la media di ogni $s_i$, dove $s_i$ è il silhouette coeﬃcient del punto $i$ ed è definito come $s_i=\tfrac{b_i-a_i}{\max(a_i,b_i)}$, dove:
- $a_i$ è la distanza media del punto $i$ con i punti del cluster. Questa esiste anche in spazi non euclidei perché non calcolo la media dei punti (che potrebbe non appartenere allo spazio), ma la media delle distanze, che è sempre definibile.
- $b_i$ è la minima distanza media del punto $i$ con i punti di ogni altro cluster $C_k$.
Osserviamo che $s_i\in[-1,1]$ e più il valore è alto melgio è perché vuol dire che la distanza tra punti del cluster è minore della distanza tra cluster diversi.