Sono problemi del tipo $\displaystyle\min_{x\in S} f(x)$, con $S\subset\mathcal{R^n}$.
# Direzione ammissibile
Rispetto all'[Ottimizzazione non vincolata](Ottimizzazione%20non%20vincolata.md) la direzione deve essere ammissibile, cioè dopo lo spostamento da $x^k$, il nuovo punto di minimo $x^{k+1}$ deve essere contenuto in $S$.
> Una direzione $d\in\mathcal{R}^n$ è ammissibile per $\bar{x}$ in S se vale $\bar{x}+td\in S$, con $t\in(0,E)$ per un qualche $E>0$.
- Se $x^*$ è minimo locale per $f$ su $S$, allora $\nexists d\in\mathcal{R}^n$ ammissibile e di discesa.
- Sia $S\subset\mathcal{R}^n$ convesso. Dato $\bar{x}\in S$, la direzione $d=(x-\bar{x})$ è ammissibile $\forall x\in S$.
# Ottimalità
- Sia $f\in C^1(S)$ con $S$ convesso. Se $x^*$ è minimo locale, allora $\nabla f(x^*)^T(x-x^*)\ge0$, $\forall x\in S$.
- Sia $f\in C^1(S)$ con $S$ convesso e $f$ convessa. $x^*$ è minimo locale se e solo se $\nabla f(x^*)^T(x-x^*)\ge0$, $\forall x\in S$.
# Vincoli
Vediamo alcuni tipi di vincoli.
- [[Vincoli poliedrali]]
- [[Vincoli in forma analitica]]
# Algoritmi iterativi
Vediamo degli algoritmi iterativi che si basano sulla [line search](Line%20search.md) ( da cambiare link), cioè per cui $x^{k+1}=x^k+\alpha_kd_k$.
##### Proiezione ortogonale
##### Algoritmi
- [[Gradiente proiettato]]
- [[Frack-Wolfe]]