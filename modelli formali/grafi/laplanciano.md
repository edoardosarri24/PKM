Il laplanciano $L$ di un grafo è definito sulla [matrice dei gradi](grafi%20indiretti.md#Matrice%20dei%20gradi) $D$ e sulla [matrice di adiacenza](grafi%20indiretti.md#Matrice%20di%20adiacenza) $A$ come $L=D-A$, dove $l_{ij}=\begin{cases}d_i&\text{ if }j=i\\-1&\text{ if }j\in N_i\\0&\text{ otherwise }\end{cases}$. C'è una corrispondenza biunivoca tra un grafo e il relativo laplanciano.
# Proprietà
- La somma degli elementi su una riga (colonna) è 0: $\displaystyle\sum_{j=1}^NL_{ij}=0$.
	Vedi la dimostrazione a slide 3 pagina 22.
- In un [grafo indiretto](grafi%20indiretti.md) è una matrice simmetrica: $L=L^T$.
- È una matrice a diagonale dominante: $|l_{ij}|=\sum_{i\ne j}|l_{ij}|$.
- È una matrice semidefinita positiva: $x^TLx=\displaystyle\sum_{i=1}^N\sum_{j=1}^Nx_iL_{ij}x_j\ge0, \forall x$.
- La forma quadratica $v(x)=x^TLx$ del laplanciano soddisfa $v(x)=\displaystyle\sum_{\{i,j\}\in E}(x_i-x_j)^2$.
	Questo vuol dire che, dato un vettore stato (dove $x_i$ è lo stato dell'$i$-esimo nodo) $x\in\mathbb{R}^N$ con $N$ nodi, la forma quadratica mi fornisce una misura di disaccordo (i.e., $x_i-x_j$) tra i nodi del grafo.
	Vedi dimostrazione slides 3 pagina 26-27.
# Spettro
Definiamo lo spettro del laplanciano come $\{\lambda_1,\cdots,\lambda_N\}$, con $\lambda_i$ $i$-esimo [autovalore](algebra%20lineare.md#Autovalori).
##### Proprietà
- Dalle proprietà degli [autovalori](algebra%20lineare.md#Autovalori), siccome il laplanciano è simmetrico e semidefinito positivo, abbiamo che $0\le\lambda_1\le\cdots\lambda_N$ con $\lambda_i\in\mathbb{R}$.
- Il più piccolo autovalore dello spettro del laplanciano è $\lambda_1=0$ e il relativo autovettore è $v_1=\tfrac{1}{\sqrt{N}}\mathbb{1}$. Si sceglie questo autovettore perché è prassi scegliere gli autovalori di norma unitaria; si può fare perche in generale se $v$ è un autovalore allora anche $\alpha v$ è un autovalore.
	Vedi dimostrazione slides 3 pagina 33.
- Il più grande autovalore dello spettro del laplanciano è $\lambda_N\le2d_{max}$, dove $d_{max}$ è il massimo grado di un nodo.
# Connettività algebrica
Se ordiniamo lo spettro del laplanciano in modo che $\lambda_1\le\lambda_2\le\cdots\le\lambda_N$, allora abbiamo che $\lambda_2$ è detto connettività algebrica.
Questa mi fornisce una misura quantitativa della connettività di un grafo: se $\lambda_2=0$ allora il grafo non è connesso; più $\lambda_2$ si allontana da 0 più il grafo è connesso ed è robusto [rispetto alla disconnessione](connettività.md#Robustezza%20rispetto%20alla%20disconnessione). Questo perché nei grafi che non sono completi vale $\lambda_2\le v_n\le v_e$.
##### Teorema
Se $L$ è il laplanciano di un grafo indiretto, allora $L$ è connesso $\Leftrightarrow$ $\lambda_2>0$.
- Osservazione
	- Se il grafo è connesso allora la molteplicità degli autovalori in 0 è 1 ed è data da $\lambda_1$. Questo vuol dire che la forma quadratica $v(x)$ si annulla in una sola direzione.
	- Se il grafo non è connesso allora il numero di autovalori in 0 è pari al numero di componenti connesse. Questo vuol dire che la forma quadratica si annulla in tante direzioni quante sono le componenti connesse.
- Dimostrazione
	- $L$ connesso $\Rightarrow$ $\lambda_2>0$
		La misura del disaccordo è $v(x)=\displaystyle\sum_{\{i,j\}\in E}(x_i-x_j)^2$. Siccome il grafo è connesso, osserviamo (se si fa il disegno è evidente) che questa si annulla solo se ogni termine della sommatoria è 0, cioè se $x_i=x_j=\alpha, \forall i,j\in N$, cioè se $x=\alpha\mathbb{1}$; questo vuol dire anche che la forma quadratica $v(x)=x^TLx$ si annulla solo in una direzione, quella in cui tutte le componenti sono uguali.
		Siccome $L$ è semidefinita positiva allora il kernel di $L$ corrisponde a uno spazio vettoriale di dimensione 1 e questo vuol dire che esiste un solo autovalore in 0 (questo viene dall'algebra, prendilo così come è). Allora, se gli autovalori sono ordinati si ha $\lambda_k>0,\forall k>1$ e quindi $\lambda_2>0$.
	- $L$ non connesso $\Rightarrow \lambda_2=0$
		Dimostrare $\lambda_2>0\Rightarrow L$ connessa, equivale a dimostrare $L$ non connesso $\Rightarrow \lambda_2=0$ se facciamo la contro nominale.
		Il disaccordo è ancora $v(x)=\displaystyle\sum_{\{i,j\}\in E}(x_i-x_j)^2$, ma se il grafo non è connesso si annulla anche quando $x_i\ne x_j$: in particolare è sufficiente assegnare lo stesso valore ai nodi che appartengono alla stessa componente connessa. Questo implica che la forma quadratica si annulla in più direzioni e allora che ci sono più autovalori in 0.
# Grafo pesato
Se consideriamo un grafo pesato, cioè un grafo in cui a ogni arco è assegnato un peso, allora il discorso del laplanciano, supponendo che il grafo sia indiretto e che i pesi siano positivi, valgono comunque:
- Il laplanciano è definito come $L=D_w-A$ con $D_w$ matrice dei gradi pesata, dove $l_{ij}=\begin{cases}\sum_{k\in N_i}w_{ik}&\text{ if }i=j\\-w_{ij}&\text{ if }j\in N_i\\0&\text{ otherwise}\end{cases}$: sulla diagonale abbiamo la somma dei pesi degli archi incidenti sul nodo; se c'è una connessione allora abbiamo l'opposto del peso di tale connessione.
- $L$ è simmetrica.
- La somma sulle righe è 0.
- $L$ è semidefinita positiva.
- $\lambda_1=0$.
- $\lambda_2>0$ $\Leftrightarrow$ il grafo è connesso.
- La misura totale sul disaccordo è dalla forma quadratica $v(x)=\displaystyle\sum_{\{i,j\}\in E}(x_i-x_j)^2$.