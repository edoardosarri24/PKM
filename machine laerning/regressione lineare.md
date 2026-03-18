La regressione (o anche la classificazione) lineare è uno dei modelli più semplici di [Machine learning](Machine%20learning.md).
La funzione che il modello approssima e che deve imparare dipende solo dalla dimensione dei dati, cioè dal numero di features $H$: $\phi(x,w)=w^T\phi(x)=\displaystyle\sum_{h=1}^Hw_h\phi_h(x)$, dove $\phi(x_i)=\phi_i$ sono le basis function.
# Basis function
Le basis function servono per arricchire i dati e permette di rappresentare i dati in modo non lineare pur restando il modello lineare.
Le più usate sono:
- Lineare
	Si ha $\phi(x,w)=\alpha^Tx+\beta$, ovvero abbiamo $w=\begin{bmatrix}\alpha\\\beta\end{bmatrix}$  e $\phi(x)=\begin{bmatrix}x\\1\end{bmatrix}$.
- Polinomiale
- Sinusoide
- Radia basis function
# Linear least-square
La regressione lineare ai minimi quadrati si può formalizzare come $\phi(x,w)=w'\phi(x)$ dove la [funzione di loss](Funzioni%20di%20loss.md) è definita come $L(y,\phi(x,w))=(y,\phi(x,w))^2=(y-w'\phi(x))^2$.
##### Rischio emprico
Il problema di [empirical risk minimization](Funzioni%20di%20loss.md) sta nel minimizzare $J_{EMP}(w)=\displaystyle\sum_{k=1}^K(y_k-w'\phi(x_k))^2$. I pesi ottimali $w^o$ sono definiti come $w^o=\displaystyle\arg\min_wJ_{EMP}(w)=\arg\min_w\displaystyle(y_k-w'\phi(x_k))^2$ e si trovano ponendo a 0 il gradiente:$\displaystyle\tfrac{\partial}{\partial w}J_{EMP}(w)=\sum_{k=1}^K\tfrac{\partial}{\partial w}(y_k-\phi(x_k)'w)^2=-\sum_{k=1}^K2(y_k-\phi(x_k)'w)^2$; allora abbiamo che $\displaystyle\tfrac{\partial}{\partial w}J_{EMP}(w)=0\Leftrightarrow\sum_{k=1}^K\phi(x_k)\phi(x_k)'w=\sum_{k=1}^K\phi(x_k)y_k$.
Osserviamo che, definendo $\displaystyle\sum_{k=1}^K\phi(x_k)\phi(x_k)'w=\Omega w$ e $\sum_{k=1}^K\phi(x_k)y_k=q$, i pesi possono essere trovati in forma chiusa come $w=\Omega^{-1}q$.