L'energy distance è una metrica statistica per misurare la distanza di due [random variable](random%20variable.md) in modo non parametrico: mentre i metodi parametrici fanno supposizioni sulla distribuzione di provenienza dei dati e poi cercano di stimare i parametri che la definiscono, questo metodo non stima nessuna [densità di probabilità](random%20variable.md#PDF).
# Formulazione
La formula matimatica definisce la distanza tra le famiglie $F$ e $G$ come
$\mathcal{E}(F,G) = 2\,\mathbb{E}\Vert X - Y\Vert - \mathbb{E}\Vert X - X'\Vert - \mathbb{E}\Vert Y - Y'\Vert$.
##### Componenti
- $X$ e $X'$ sono due campioni iid provenienti da $F$.
- $Y$ e $Y'$ sono due campioni iid provenienti da $G$.
- $\Vert\cdot\Vert$ rappresenta una qualunque distanza tra due punti in un qualunque spazio; può essere anche una distanza di uno spazio multidimensionale. Per questo motivo l'energy distance è dimension-free: l'unica fase in cui la dimensione dei campioni ha una qualch erilevanza è questo valore.
##### Significato
Scomponiamo ogni pezzo:
- $2\mathbb{E}\Vert X-Y\Vert$
	È il valore atteso della distanza di due elementi delle due famiglie.
- $2\mathbb{E}\Vert X-X'\Vert$
	È il valore atteso della distanza di due elementi provenienti da $F$.
- $2\mathbb{E}\Vert Y-Y'\Vert$
	È il valore atteso della distanza di due elementi provenienti da $G$.
##### Stima campionaria
Nella realtà abbiamo non conosciamo $F$ e $G$ e ci serve quindi la stima campionaria $$d_{n,m} = \frac{nm}{n+m} \left( \frac{2}{nm}\sum_{i=1}^n \sum_{j=1}^m \Vert X_i - Y_j \Vert - \frac{1}{n^2}\sum_{i=1}^n \sum_{j=1}^n \Vert X_i - X_j \Vert - \frac{1}{m^2}\sum_{i=1}^m \sum_{j=1}^m \Vert Y_i - Y_j \Vert \right),$$
dove:
- Le due famiglie $F$ e $G$ hanno cardinalità rispettivamente di $n$ e $m$.
- Il valore atteso è diventato la media.
- Il $\frac{nm}{n+m}$ rappresenta il fattore di scala. Le due popolazioni campionarie possono avere cardinalità molto diverse. Questo fattore ci permette di non rendere la famiglia con meno campioni (se questi sono molti meno) totalmente irrilevante; infatti $\displaystyle\lim_{n \to \infty} \frac{nm}{n+m} = m$.
# Permutation Test
L'energy distance ci restituisce un valore $d_{n,m}$, ma non ci dice se questo è grande o piccolo. Per sapere come leggere questo risultato si utilizza un permeutation test, che ci permette di arrivare a un $p$-value.
##### Funzionamento
L'ipotesi nulla $H_{0}$ è che le due famiglie siano uguali, cioè che $F=G$.
- Si uniscono i campioni in un unico grande insieme.
- Si estraggono $m$ campioni e si attribuiscono a $F$. Si calcola a questo punto l'energy distance $d^*_{n,m}$ tra questi due sotto insiemi ottenuti a questa iterazioni.
- Si itera $B$ volte ottenendo una distribuzione di $d^*_{n,m}$.
- Si definisce $p\text{-value} = \frac{\# \{d_{n,m}^* \ge d_{n,m}\}}{B}$.
##### Significato
Se abbiamo che $p\text{-value}<\text{theshould}$ (e.g., 0.05) allora si rigetta l'ipotesi nulla e le due famiglie sono diverse.
Questo perché quando mescolo i dati, le due popolazioni $F$ e $G$ diventano uguali e quindi mi aspetto un valore $d^*_{n,m}$ molto basso. Se il valore $d_{n,m}$ è "alto" (non importa sapere quanto) allora il $p$-value sarà basso, perché $d_{n,m}>d^*_{n,m}$ quasi sempre.