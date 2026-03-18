È un algoritmo di ottimizzazione non vincolata iterativo basato su line search.
Il passo di aggiornamento è $x^{k+1}=x^k+\alpha_kd_k$, dove $\alpha_k$ è scelto con il [metodo di Armijo](Ottimizzazione%20non%20vincolata.md#Armijo) e $d_k=-\nabla f(x^k)$. Osserviamo che nell'addestramento del MLP (e delle reti neurali) $\alpha_k$ è il learning rate.
##### Direzione
- L'antigradiente $d_k=-\nabla f(x^k)$ è una [direzione di discesa](Definizioni.md#Direzione%20di%20discesa) in $x^k$.
- La sequenza $\{d_k\}$ è [gradent related](Definizioni.md#Gradient%20related) per $c_1=c_2=1$. Allora abbiamo che se $\alpha_k\in[0,\Delta_{low}=\tfrac{2(1-\gamma)}{L}]$ soddisfa il [criterio di Armijo](Ottimizzazione%20non%20vincolata.md#Armijo) (se ci sono le proprietà richieste).
##### Proprietà
- Se $\{x^k\}$ è prodotto dal metodo della discesa del gradiente con $\alpha_k=\tfrac{1}{L}$ ($\gamma=\tfrac{1}{2}$ e $\{d_k\}$ gradient related) e $f$ [L-smooth](Definizioni.md#Funzione%20L-smooth)  e [convessa](Definizioni.md#Funzione%20convessa) $\Rightarrow$ l'[iteration error](Ottimizzazione%20non%20vincolata.md#Errore%20e%20complessità) è $f(x^k)-f^*=\tfrac{L\lVert x^0-x^*\rVert}{2k}\in\mathcal{O}{(\tfrac{1}{k})}$, cioè ha un'iteration complexity di $\mathcal{O}{(\tfrac{1}{\epsilon})}$.
- Se $f$ è [fortemente convessa](Definizioni.md#Funzione%20fortemente%20convessa) allora l'iteration complexity è di $\mathcal{O}{(log\tfrac{1}{\epsilon})}$.
# Stocastic Gradient Descent (SGD)
È applicabile per problemi di somme finite, cioè per problemi del tipo $\displaystyle\min_{x\in\mathcal{R^n}}f(x)=\min_{x\in\mathcal{R^n}}\sum_{i=1}^nf_i(x)$.
##### Direzione e passo
- In questo caso $d_k$ è preso come l'antigradiente di una componente della sommatoria, cioè $d_k=-\nabla f_i(x^k)$. In questo modo $d_k$ è sicuramente di discesa per $f_i(x^k)$, ma non possiamo dire che lo sia per $f(x^k)$. Però se osserviamo il suo valore atteso (con probabilità uniforme) abbiamo $E[d_k]=\displaystyle\sum_{i=1}^n-\tfrac{1}{n}\nabla f_i(x^k)=-\nabla f(x^k)$, cioè con molte iterazione è esattamente l'antigradiente.
- Avendo che $d_k$ non è necessariamente di discesa, non possiamo usare il [metodo di Armijo](Ottimizzazione%20non%20vincolata.md#Armijo). Allora prendiamo $\alpha_k$ costante o costruiamo una sequenza che decresce ma non troppo.
##### Proprietà
Se $f(x)=\sum_{i=1}^nf_i(x)$ è [L-smooth](Definizioni.md#Funzione%20L-smooth), $\{x^k\}$ prodotta con SGD dove $\{\alpha_k\}$ soddisfa $\displaystyle\sum_{i=1}^{\infty}\alpha_i=+\infty$ e $\displaystyle\sum_{i=1}^{\infty}\alpha_i<+\infty$ (la sequenza ma non troppo velocemente) $\Rightarrow$ vale $\displaystyle\lim_{k\to\infty}E[\lVert\nabla f(cx^k)\rVert]=0$.
##### Complessità
- Nel caso non convesso l'iteration compleity è $\mathcal{O}{(\tfrac{1}{\epsilon^4})}$.
- Nel caso [convesso](Definizioni.md#Funzione%20convessa) l'iteration compleity è $\mathcal{O}{(\tfrac{1}{\epsilon^2})}$.
- Nel caso [fortemente convesso](Definizioni.md#Funzione%20fortemente%20convessa) l'iteration compleity è $\mathcal{O}{(\tfrac{1}{\epsilon})}$.
### Minibatch
Invece di prendere un solo termine della sommatoria, prendiamo un batch di componenti. In questo modo riduciamo la varianza di $d_k$.
Allora prendiamo $d_k=\tfrac{1}{M}\displaystyle\sum_{i\in B_k}-\nabla f_i(x^k)$, con $B_k\subset\{1,\cdots,N\}$ e $|B_k|=M$.