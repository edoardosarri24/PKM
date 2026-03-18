Il problema che vogliamo risolvere è rappresentare i dati in meno dimensioni senza perdere troppe informazioni.
Ci sono diversi motivi per affrontare questo problema: eliminare il rumore introdotto da features non fondamentali per poi scoprire pattern che non si noterebbero quando la dimensionalità è troppo alta; raggiungere un numero di dimensioni che ci permette di visualizzare i grafici (i.e. tre dimensioni); semplificare il processing di dati troppo complessi.
##### La maledizione della dimensionalità
Quando si parla di dimensionalità viene sempre fuori la maledizione della dimensionalità. Il punto è che all'aumentare della dimensionalità la sparsità dei dati aumenta in modo non lineare.
##### Assunzione
Diremo che i dati sono all'interno di una matrice $n\times d$, dove $n$ è il numero di samples e $d$ è la dimensionalità, cioè il numero di features. L'obiettivo è produrre quindi una matrice $n\times k$, con $k<d$, che preserva la maggior parte delle informazioni rispetto alla matrice di partenza.
##### Problemi
Le due tecniche sottostanti (i.g. PCA e SVD) vogliono catturare relazioni lineari. La PCA infatti trasforma i dati tramite una trasformazione lineare e se i dati non hanno dei pattern lineari allora la PCA non riesce a catturarli.

# Principal Component Analysis (PCA)
Questo è un [esempio](PCA%20esempio.pdf).
##### Funzionamento
- Partiamo da un dataset i cui elementi sono memorizzati in una matrice $M\in\mathbb{R}^{n\times d}$. Vogliamo arrivare a $M'\in\mathbb{R}^{n\times k}$.
- Si devono trovare le direzioni lungo le quali i punti sono maggiormente distribuiti, cioè quelle direzioni con più informazione, ovvero quelle con la massima varianza.
	- Si calcola le matrici quadrate $MM^T$ o $M^TM$ a seconda se si hanno più righe o colonne. Se si hanno più dati si usa $M^TM$; se si hanno più features si usa $MM^T$.
	- Si calcolano gli autovettori e gli autovalori di questa matrice trovata. Gli autovettori definiscono le direzioni, gli autovalori quanta varianza c'è lungo quella direzione.
- Si ordinano gli autovalori e si prendono gli autovettori relativi ai $k$ autovalori che hanno valore maggiore.
- Si mettono insieme questi $k$ autovettori e si forma una matrice $E$. Questa matrice avrà $d$ righe perché gli autovalori hanno tanti elementi quante sono le features della matrice su cui sono calcolati; avrà poi $k$ colonne.
- Si ottiene la matrice finale $M'=ME$.
##### Osservazione
Se $E\in\mathbb{R}^{d\times d}$ è la matrice di tutti gli autovettori, allora fare $ME$ equivale a traslare in un nuovo sistema di riferimento i dati originali senza ridurre la dimensionalità. Il nuovo sistema di riferimento è quello definito dagli assi che puntano nelle direzioni di maggiore varianza.
# Singular Value Decomposition (SVD)
È un metodo più potente rispetto a PCA.
L'idea è quella di scomporre $M\in\mathbb{R}^{n\times d}$ (sempre possibile) come $M_{[m\times n]}=U_{[m\times r]}\Sigma_{r\times r}(V)^T_{n\times r}=\sigma_1u_1v_1^T+\cdots+\sigma_ru_rv_r^T=M'$, dove:
- $M$ è la matrice dei dati e ha rango $r$. Ad esempio $M$ contiene $m$ documenti e $n$ termini.
- $U$ contiene i vettori singolari sinistri, cioè gli autovettori di $MM^T$. L'$i$-esima riga di $U$ si può interpretare come l'$i$-esimo elemento di $M$ mappato in uno spazio ridotto che rappresenta i concetti.
- $\Sigma$ è una matrice diagonale con $\sigma_{ii}>\sigma_{jj}$ se $i<j$. Ogni elemento $\sigma_{ii}$ è la radice quadrata dell'autovalore $i$-esimo di $M^TM$ o $MM^T$: rappresenta quindi la varianza che hanno i dati lungo la direzione definita dal relativo autovettore; $\sigma_i$ rappresenta quindi quanto è forte il concetto della colonna $i$-esima di $U$.
- $V$ contiene i vettori singolari destri, cioè gli autovettori di $M^TM$. L'$i$-esima riga di $V$ si può interpretare come l'$i$-esima features di $M$ mappata in uno spazio ridotto che rappresenta i concetti.
##### Funzionamento
Per ridurre la dimensionalità e arrivare a $M'\in\mathbb{R}^{n\times k}$ si devono conservare solo i $k$ concetti più importanti, che sono rappresentanti dai primi \sigma_{ii} per $i\ge k$: costruendo $\tilde{\Sigma}$ in modo che i restanti valori siano 0 si arriva alla nostra matrice $M’ = U\tilde{\Sigma} V^T$.
Per decidere $k$, cioè quante dimensioni eliminare, si può pensare di mantenere circa 80%/90% dell'energia totale; l'energia è definita come $\sum\sigma_i^2$. Solitamente questo richiede di rimuovere più del 10%/20% delle dimensioni.
##### Osservazioni
- Il costo è dato da $\mathcal{O}{(nm^2)}$ o $\mathcal{O}{(n^2m)}$ a seconda di quale è la dimensionalità più piccola.
- SVD minimizza il recostruction error cioè l'errore tra la matrice originale $M$ e la sua approssimazione $M'$. Questo errore può essere calcolato tramite la distanza (o norma) di Frobenius, cioè $||M-M'||_F=\sqrt{\sum_{ij}(M_{ij}-M'_{ij})^2}$.
- Le matrici $U$, $\Sigma$ e $V$ esistono sempre e sono uniche.