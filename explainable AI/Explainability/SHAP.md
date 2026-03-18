SHapley Additive exPlanation (SHAP) è una tecnica del 2017 per l'[outcam explaination](Metodi.md#Outcam%20explaination) (ma anche per la [model inspection](Metodi.md#Model%20inspection)) nel campo dell'[Explainability](Explainability.md) e deriva dalla [teoria dei giochi cooperativa](Cooperative%20game%20theroy.md).
# Funzionamento
SHAP usa la Shepley value per spiegare un test in modo locale tramite l'importanza delle features. In realtà si può usare anche per dare un'importanza globale.
Mettiamo in correlazione la cooperative game theroy con SHAP:
- I giocatori $D$ diventano le features.
- I profitti per ogni sotto insieme delle features diventano il comportamento del modello (es: loss, accuracy).
- La Shappley value diventa la quantificazione dell'impatto delle features.
##### Algoritmo
SHAP lavora in modo additivo e la spiegazione è $f(\hat{x})=\phi_0+\sum_{j=1}^d\phi_j\hat{x}_j'$, dove: $f$ è la black box, $\hat{x}$ è l'istanza da spiegare, $\phi_0=E[f(x)]$ cioè è il valore atteso (la media) su tutto il train set, $\hat{x}'$ è la rappresentazione interpretabile di $\hat{x}$ (è lo stesso concetto di [LIME](LIME.md) che aveva due rappresentazioni per ogni dato. In questo caso è un vettore binario che rappresenta la colazione, cioè le features).
La spiegazione deve essere interpretata in modo [additivo rispetto alla media](SHAP%20interpretazione.png): nella media succede $\phi_0$; ognuna delle tue features da un contributo in positivo o negativo.
# Shapley value
Nell'algoritmo le $\phi_i$ sono proprio le [Shapley value](Cooperative%20game%20theroy.md#Shapley%20value).
Per il calcolo esatto si dovrebbero enumerare tutte le possibili coalizioni (cioè combinazioni di features). Questo però ha complessità $2^d$, cioè esponenziale nel numero di features e diventa unfeasible per circa $d\ge20$.
Per questo motivo si usano procedure di sampling per approssimare l'enumerazione delle coalizioni; si ottiene così delle approssimazioni di shampley value.
Ci sono più tecniche di approssimazione: kernel SHAP che è model agnostic, o treeSHAP e deepSHAP che sono model specific.
##### Kernel SHAP
È una tecnica per stimare i contributi delle features per una predizione $x$, cioè per stimare la Shapley value.
Dato un esempio $\hat{x}$ da spiegare allora l'algoritmo si comporta come:
- Si campiona un insieme di $N<2^d$ (risolve il problema del numero di coalizione esponenziali) coalizioni, ognuna definita da un vettore $z'_k\in\{0,1\}^d$, con $k\in\{1,\cdots, N\}$. Una coalizzatone rappresenta quali features saranno usate (in pratica indicano come perturbare l'esempio da spiegare) ed è quindi un vettore binario.
- Si usa la trasformazione $z_k=h_x(z'_k):\{0,1\}^d\to\mathbb{R}^p$, dove $d$ è il numero di elementi all'interno di una coalizioni e $p$ è il numero di features usate dalla black box $f$: per dati tabulari $d=p$; per immagini $p$ è il numero di pixel e $d$ il numero di superpixel. La trasformazione avviene in questo modo: se $z'_i=1$ allora nel nuovo vettore il valore della posizione $i$-esima è $\hat{x_i}$; se invece $z'_i=0$ il valore della posizione $i$-esima viene campionato dal dataset. Se facciamo questo procedimento per ogni coalizione campionata otteniamo un dataset di perturbazioni.
- Si calcola $f$ per ogni elemento di questo dataset di perturbazioni.
- Si usa [LIME](LIME.md) per addestrare un modello lineare: si usa come loss $L(f,g,\pi_x)=\displaystyle\sum_{z,z'\in Z}\pi_x(z')(f(z)-g(z'))^2$, dove $\pi_x(z')=\tfrac{d-1}{\binom{d}{|z'|}|z'|(d-|z'|)}$; è come se si ponesse $\Omega=0$ e questo vuol dire non porre limiti al numero di features usate nella spiegazione.
- Si restituiscono i pesi del modello appreso come approssimazione dello shapley value $\phi$.
# Globalità
Tramite SHAP si può ottenere una misura globale dell'importanza delle features.
Questo si fa tramite la media del valore assoluto su tutto il dataset perturbato: $I_j=\tfrac{1}{N}\sum_{i=1}^N|\phi_j^{(i)}|$, cioè l'importanza della feature $j$-esima è data dalla media del $j$-esimo elemento dello shapley value su tutti elementi.