L'insieme ammissibile è dato da $S=\{x\in\mathcal{R}^n|Ax\le b\}$, con $A\in\mathcal{R}^{m\times n}$ e $b\in\mathcal{R}^m$; abbiamo quindi $m$ vincoli del tipo $a_i^Tx\le b_i$.
##### Direzione ammissibile
I vincoli attivi in $\bar{x}\in S$ sono $I(\bar{x})=\{i|a_i^Tx=0\}$. Osserviamo che se un punto $x\in S$ non sta sul bordo, allora $I(x)=\emptyset$.
- Sia $S=\{x\in\mathcal{R}^n|Ax\le b\}$. Una direzione $d\in\mathcal{R}^n$ è ammissibile in $\bar{x}\in S$ $\Leftrightarrow$ $a_i^Td\le0$, $\forall i\in I(\bar{x})$.
# Vincoli di box
Un tipo particolare di vincoli poliedrali sono i vincoli di box, cioè dove $S=\{x\in\mathcal{R}^n|l\le x\le u\}$.
##### Direzione ammissibile
Data l'i-esima componente $x_i$ del punto $x\in S$, si ha che:
- Se $x_i=l_i\Rightarrow d=e_i$ è ammissibile.
- Se $x_i=u_i\Rightarrow d=-e_i$ è ammissibile.
- Se $x_i\in(l_i,u_i)\Rightarrow d=\pm e_i$ è ammissibile.
##### Ottimalità
Sia $f\in C^1(S)$ con $S=\{x\in\mathcal{R}^n|l\le x\le u\}$. Se $x^*$ è minimo locale $\Rightarrow$ vale $\tfrac{\partial}{\partial x_i}f(x^*) =\begin{cases} =0 & \text{ se } x_i\in(l_i,u_i) \\>0 & \text{ se } x_i=l_i\\<0 &\text{ se } x_i=u_i\end{cases}$.