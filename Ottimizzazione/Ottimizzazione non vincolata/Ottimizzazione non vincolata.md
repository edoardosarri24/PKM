L'ottimizzazione non vincolato è un casa particolare di [ottimizzazione](Ottimizzazione.md). Vogliamo trovare un $x^*$ tale che $\nabla f(x^*)=0$.
# Algoritmi iterativi
L'aggiornamento della soluzione avviene tramite $x^{k+1}=x^k+S_k$, dove $S_k$ è lo spostamento.
- [Gradient descent](Gradient%20descent.md)
##### Richieste
In un algoritmo iterativo cerchiamo 3 cose:
- Convergenza
	Vogliamo $\displaystyle\lim_{k\to\infty}f(x^{k+1})=\bar{x}$, cioè che la sequenza di soluzioni converga verso un punto di accumulazione. Questa non è una proprietà difficile da richiedere.
- Stazionarietà
	Vogliamo $\displaystyle\lim_{k\to\infty}f(x^k)=\bar{x}$, con $\nabla f(\bar{x})=0$, cioè vogliamo che tutta la sequenza converga verso un punto stazionario
	Oppure vogliamo che $\displaystyle\lim_{k\to\infty}inf\lVert\nabla f(x^k)\rVert=0$, cioè che esista una sotto sequenza di $\{x^k\}$ che converge verso un punto stazionario.
	Questa proprietà è difficile da richiedere. Solitamente si usa una tolleranza, cioè si richiede che $\lVert \nabla f(x^k)\rVert\le\epsilon$.
##### Tasso di convergenza
È $l=\displaystyle\lim_{k\to\infty}\tfrac{f(x^{k+1})-f^*}{f(x^k)-f^*}$. Si definisce:
- Sublineare se $l=1$.
- Superlineare se $l=0$.
- Lineare se $l\in(0,1)$.
##### Errore e complessità
L'iteration complexity che vorremmo è $\mathcal{O}{(log\tfrac{1}{\epsilon}})$, cioè per ogni cifra decimale in più di precisione abbiamo una sola iterazione.
- Un algoritmo ha iteration error $\mathcal{O}{g(k)}$ se $f(x^k)-f^*\in\mathcal{O}{g(k)}$.
- Un algoritmo ha iteration complexity $\mathcal{O}{g(\epsilon)}$ se $min\{k|f(x^k)-f^*\le\epsilon\}\in\mathcal{O}{g(\epsilon)}$.
### Line search
Il passo di aggiornamento è $x^{k+1}=x^k+\alpha_kd_k$, dove $\alpha_k$ è il passo (di quanto mi muovo) e $d_k$ è la direzione (dove mi muovo).
Si parla di line serach esatta (fattibile in forma chiusa solo $f$ è [quadratica](Definizioni.md#Funzione%20quadratica)) se $\alpha_k=\displaystyle\arg\min_{\alpha>0} f(x^k+\alpha d_k)$; si parla invece di line serach inesatta (quella di cui parleremo) se $\alpha_k\approx\displaystyle\arg\min_{\alpha>0} f(x^k+\alpha d_k)$.
##### Armijo
La Condizione di Armijo ci dice che il passo $\alpha_k$ deve essere tale che, data una direzione $d_k$ di [discesa](Definizioni.md#Direzione%20di%20discesa), $f(x^k+\alpha_kd_k)\le f(x^k)+\gamma\alpha_k\nabla f(x^k)^Td_k$, con $\gamma\in(0,1)$.
Il metodo di Armijo invece definisce $\alpha_k$ dati $\Delta_0>0$, $\gamma,\delta\in(0,1)$, come $\alpha_k=\displaystyle\max_{j=0,1,\cdots}\{\Delta_0\delta^j|f(x^k+\alpha_kd_k)\le f(x^k)+\gamma\Delta_0\delta^j\nabla f(x^k)^Td_k\}$, cioè come il massimo intervallo $\Delta_0\delta^j$ che soddisfa il criterio di Armijo. Si tratta di un metodo di backtrack, cioè a ogni iterazione si diminuisce il passo (di un fattore $\gamma$) che dovrebbe verificare la condizione.
- Il metodo di Armijo termina sempre, con $\alpha_k\le\Delta_0$.
- Se $\{x^k\}$ è prodotto con $x^{k+1}=x^k+\alpha_kd_k$ con $x^0\in L_0=\{x\in\mathcal{R^n}|f(x)\le f(x^0)\}$ [compatto](Definizioni.md#Insieme%20compatto), dove $\{d_k\}$ è [gradient related](Definizioni.md#Gradient%20related) e $\alpha_k$ è scelto con il metodo di Armijo $\Rightarrow$ $\{x^k\}$ ha punti di accumulazione, ognuno dei quali è [stazionario](Definizioni.md#Punto%20stazionario).
- Se $\{x^k\}$ è prodotto con $x^{k+1}=x^k+\alpha_kd_k$, dove $\{d_k\}$ è [gradient related](Definizioni.md#Gradient%20related) e $f$ [L-smooth](Definizioni.md#Funzione%20L-smooth) $\Rightarrow$ la condizione di Armijo è soddisfatta$\forall\alpha\in[0,\Delta_{low}]$, con $\Delta_{low}=\tfrac{2c_2(1-\gamma)}{c_1^2L}$.
- Se $f$ [L-smooth](Definizioni.md#Funzione%20L-smooth), $\{d_k\}$ è [gradient related](Definizioni.md#Gradient%20related) e $\alpha_k$ è scelto con il metodo di Armijo con passo iniziale $\Delta_0$, $\Rightarrow$ il numero di passi di backtrack dopo i quali sicuramente $\alpha_k$ verifica il criterio di Armijo è $j^*=max\{0,log_{1/\delta}\tfrac{\Delta_0}{Delta_{low}}\}$.
- Se $\{x^k\}$ è prodotto con $x^{k+1}=x^k+\alpha_kd_k$, dove $\{d_k\}$ è [gradient related](Definizioni.md#Gradient%20related), $f$ [L-smooth](Definizioni.md#Funzione%20L-smooth) e $\alpha_k$ è scelto con il metodo di Armijo $\Rightarrow$ il numero di iterazioni per raggiungere $\lVert\nabla f(x^k)\rVert\le\epsilon$ è $k^*\le\tfrac{f(x^0)-f^*}{\gamma c_2\delta\Delta_{low}\epsilon^2}$, cioè l'iteration complexity è di $\mathcal{O}{(\tfrac{1}{\epsilon^2})}$.