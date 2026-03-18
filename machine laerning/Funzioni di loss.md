Quando lavoriamo con l'[Apprendimento supervisionato](Apprendimento%20supervisionato.md) si risolve solitamente un problema di Empirical risk minimization: si vuole trovare il vettore dei pesi $w$ tale da minimizzare $J_{emp}(w)=\sum_{i=N}^KL(y_i,\phi(x_i,w))$, dove $L(y_i,\phi(x_i,w))$ è la funzione di loss.
Solitamente la loss complessiva è composta da una [Regolarizzazione](Regolarizzazione.md).
# Sum of Squere Error (SSE)
La $SSE$ è definita come $SSE=\displaystyle\sum_{i=1}^N(y_i-\hat{y}_i)^2$. La soluzione è detta soluzione ai minimi quadrati.
##### Conseguenze
- Tende a penalizzare maggiormente gli esempi che commettono un errore maggiore.
- Rende il vettottore dei pesi più omogeno, cioè dove i suoi elementi sono tutti uguali.
# Mean Squared Erroe (MSE)
La $MSE$ è definita come $MSE=\displaystyle\tfrac{1}{N}\sum_{i=1}^N(y_i-\hat{y}_i)^2$. La soluzione è detta soluzione ai minimi quadrati.
##### Conseguenze
- Tende a penalizzare maggiormente gli esempi che commettono un errore maggiore.
- Rende il vettottore dei pesi più omogeno, cioè dove i suoi elementi sono tutti uguali.
# Mean Absolute Error (MAE)
La $MAE$ è definita come $MAE=\tfrac{1}{N}\sum_{i=1}^N|y_i-\hat{y}_i|$.
##### Conseguenze
- È più resistente agli outliers, visto che li penalizza poco.
- Rende il vettore dei pesi più sparso, cioè più valori vicini a 0. È utile quando ci sono meno fattori che sono realmente importanti per predirre l'output.
- Non è differenziabile in 0 e richiede tecniche speciali per l'ottimizzazione.
# Huber loss
È definita come $L_\delta(y, \hat{y}) = \begin{cases} \frac{1}{2}(y - \hat{y})^2 & \text{se } |y - \hat{y}| \leq \delta \\ \delta \cdot (|y - \hat{y}| - \frac{1}{2} \delta) & \text{altrimenti} \end{cases}$.
##### Conseguenze
Combina la $MSE$ e la $MAE$.
È resistente agli outliers: applica la loss quadratica per gli esempi che sono pochi vicini dal valore corretto e quella lineare per quelli più lontani.
È differenziabile in 0 e più semplice da ottimizzare.
# Cross-entropy loss
È definita come $CE=-\sum_{i=1}^N y_i\log(\hat{y}_i)$.
##### Uso
È usata per la classificazione quando si vuole misurare la differenza tra la distribuzione reale e quella predetta.