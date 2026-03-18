Metodo del secondo ordine che sfrutta l'hessiana.
Data la funzione obiettivo $f$ trova l'ottimo della sua approssimazione di Taylor al secondo ordine. Se l'approssimazione è $m_k(x)=f(x^k)+\nabla f(x^k)(x-x^k)+\tfrac{1}{2}\nabla^2 f(x^k)(x-x^k)^2$ allora questa ha ottimo quando $\nabla m_k(x^*)=0$, cioè quando $x^*=x^k-[\nabla^2f(x^k)]^{-1}\nabla f(x^k)$. Allora la soluzione successiva è l'ottimo dell'approssimazione, cioè $x^{k+1}=x^k-[\nabla^2f(x^k)]^{-1}\nabla f(x^k)$.
- Il numero di iterazioni è molto basso, vista la convergenza superlineare; il costo per iterazione è alto. Allora è un ottimo metodo se il costo per iterazione non vanifica fare meno iterazioni.
##### Convergenza
Sia:
- $f\in C^2(\mathcal{R}^n)$ con $\nabla^2f(x^*)$ non singolare (cioè invertibile),
- $x^*\in\mathcal{R}^n:\nabla f(x^*)=0$,
- $x^0\in B(x^*,\epsilon)$.
Allora $\{x^k\}$ prodotta dal metodo di Newton è tale che:
- $\{x^k\}\in B(x^*,\epsilon)$.
- $\displaystyle\lim_{k\to\infty}x^k=x^*$.
- Il tasso di convergenza è super lineare.
##### Hessiana singolare
Se l'hessiana è singolare, allora la si può perturbare per renderla invertibile: in questo modo $x^{k+1}=x^k-1d_k$ con $d_k=-H_k\nabla f(x^k)$, dove $H_k=\nabla^2f(x^k)-(\lambda_{min}(\nabla^2f(x^k))-c_1)I$.
Osserviamo che $c_1$ è uno dei valori della bounded eigenvalues condition, per cui $\nabla^2f(x^k)$ è invertibile se vale $c_1\le\lambda_{min}(\nabla^2f(x^k))\le\lambda_{max}(\nabla^2f(x^k))\le c_2$.