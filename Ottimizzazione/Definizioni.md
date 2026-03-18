##### Minimo globale
$x^*\in S$ è minimo globale se $f(x^*)\le f(x), \forall x\in S$.
##### Minimo locale
$x^*\in S$ è minimo locale se $f(x^*)\le f(x), \forall x\in S\cap B(x^*,\epsilon)$.
##### Insieme compatto
$S\subset\mathcal{R}^n$ è compatto $\Leftrightarrow$ $S$ limitato e $S$ chiuso.
- $S$ limitato $\Leftrightarrow$ $\forall\{x^k\}\in S,\exists \bar{x}:\displaystyle\lim_{k\to\infty}\{x^k\}=\bar{x}$, cioè se esiste $\bar{x}$ punto di accumulazione.
- $S$ chiuso $\Leftrightarrow$ $\forall\{x^k\}\in S$, dato $\bar{x}$ punto di accumulazione, si ha $\bar{x}\in S$.
##### Insieme di livello
Un insieme di livello è $L_\alpha(f)=\{x\in\mathcal{R^n}:f(x)\le\alpha\}$.
##### Funzione coerciva
$f:S\subseteq\mathcal{R^n}\to\mathcal{R}$ è coerciva se $\forall\{x^k\}\subseteq S$ tale che $\displaystyle\lim_{k\to\pm\infty}\{x^k\}=+\infty$, allora $\displaystyle\lim_{k\to\pm\infty}f(x^k)=+\infty$.
- Vuol dire che la funzione fa cosa vuole nel mezzo, ma da un certo punto in poi va a $+\infty$ sia a destra che sinistra.
##### Punto stazionario
Un punto $x^*$ è punto stazionario per $f$ se vale $\nabla f(x^*)^Td\ge0$.
##### Insieme convesso
$S\subseteq\mathcal{R^n}$ è convesso se $(1-\lambda)x+\lambda y\in S,\ \forall x,y\in S, \ \forall\lambda\in[0,1]$.
- Vuol dire che data una coppia di punti in $S$, il segmento che li unisce sta tutto in $S$.
##### Funzione convessa
$f:S\subseteq\mathcal{R^n}\to\mathcal{R}$ è convessa se $f((1-\lambda)x+\lambda y)\le(1-\lambda)f(x)+\lambda f(y)$.
##### Funzione strettamente convessa
$f:S\subseteq\mathcal{R^n}\to\mathcal{R}$ è strettamente convessa se $f((1-\lambda)x+\lambda y)<(1-\lambda)f(x)+\lambda f(y)$.
##### Funzione fortemente convessa
$f:S\subseteq\mathcal{R^n}\to\mathcal{R}$ è fortemente convessa se $\exists g:\mathcal{R^n}\to R|f(x)=g(x)+\lambda\lVert x\rVert^2$.
- Vuol dire che $f$ deve essere convessa almeno quanto una parabola.
##### Funzione quadratica
$f:S\subseteq\mathcal{R^n}\to\mathcal{R}$ è quadratica se $f(x)=\tfrac{1}{2}x^TQx+c^Tx$, con $Q\in S_n$.
##### Direzione di discesa
$d\in\mathcal{R^n}$ è una direzione di discesa in $\bar{x}$ se $f(\bar{x}+tf)<f(\bar{x})$.
- Se $\nabla f(\bar{x})^Td<0\Rightarrow d$ è di discesa in $\bar{x}$.
- $f$ [convessa](Definizioni.md#Funzione%20convessa). $\nabla f(\bar{x})^Td<0\Leftrightarrow d$ è di discesa in $\bar{x}$.
##### Direzione di curvatura negativa
$d\in\mathcal{R^n}$ è una direzione di curvatura negativa in $\bar{x}$ se $d^T\nabla^2 f(\bar{x})d<0$.
- Se $\nabla f(\bar{x})^Td=0$ e $d^T\nabla^2 f(\bar{x})d<0\Rightarrow d$ è di discesa in $\bar{x}$.
##### Gradient related
Un insieme di direzioni \{d_k\} si dice gradient related se $\exists c_1,c_2\in\mathcal{R^+}$ tali che:
- $\lVert d_k\rVert\le c_1\lVert\nabla f(x^k)\rVert$.
- $\nabla f(x^k)^Td_k\le-c_2\lvert \nabla f(x^k)\rVert^2$.
##### Funzione L-smooth
$f\in C^1(\mathcal{R^n})$ è L-smooth se $\tfrac{\lVert\nabla f(x)-\nabla f(y)\rVert}{\lVert x-y\rVert}\le L,\forall x,y\in\mathcal{R^n}$.
- Se $f$ è L-smooth $\Rightarrow$ $f(x+d)\le f(x)+\nabla d(x)^Td+\tfrac{L}{2}\lVert d\rVert^2$.