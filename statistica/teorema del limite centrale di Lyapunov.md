È un teorema statistico che spiega come si comporta la somma di $n$ [random variable](random%20variable.md) indipendenti.
# Teorema
Se abbiamo $n$ (con $n\to\infty)$ variabili casuali indipendenti (non necessariamente identicamente distribuite, come invece è richiesto per il [teorema del limite centrale](teorema%20del%20limite%20centrale.md)) con valore atteso $\mu_{i}$ e varianza $\sigma_{i}^2$ finiti e per cui vale la condizione di Lyapunov, allora la loro somma $S_{n}=\displaystyle \sum_{i}X_{i}$, se normalizzata (i.e., sommate, stratta la media e diviso tutto per la varianza), converge verso la [distribuzione gaussiana](distribuzione%20gaussiana.md) $\mathcal{N}(0,1)$.
##### Condizioni di Lyapunov
La condizioni di Lyapunov è tale per cui $\displaystyle \lim_{ n \to \infty }\tfrac{1}{v_{n}^{2+\delta}}\sum_{i=1}^n\mathbb{E}(|X_{i-\mu_{i}}|^{2+\delta})=0$, dove:
- $v_{n}=\mathbb{V}[S_{n}]$ è la varianza della somma di tutte le variabili.
- $\gamma>0$ è un qualche parametro per cui deve valere la condizione.
##### Esempio
Si vede come questo funzioni anche nel caso di variabili casuali molto diverse dalla normale. Nell'[esempio](limite%20centrale%20di%20Lyapunov.png) abbiamo una distribuzione che è opposta alla normale, ma somma di $N$ sua istante indipendenti tende comunque alla normale.
# Significato
Questo teorema è utile in questi contesti dove le $n$ variabili casuali modellano un qualcosa e ci interessa trovare un qualcosa che è dato dalla somma di queste variabili.
La somma usando la classica [convoluzione](statistica/convoluzione.md) in questo contesto sarebbe intrattabile per problemi di tempo e spazio, ma con questa tecnica analitica stiamo dicendo che non dobbiamo sommare nulla, visto che la somma tende alla normale. Inoltre la sua accuratezza migliora tanto più quante sono le variabili da sommare, cioè quanto più il compito sarebbe intrattabile con i classici metodi non analitici.