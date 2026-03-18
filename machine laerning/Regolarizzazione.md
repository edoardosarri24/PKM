Il regolarizzatore è un termine che aggiungiamo alla [loss](Funzioni%20di%20loss.md) per migliorare l'apprendimento. In generare la loss totale si definisce come $L(w)=E_w(w)+\lambda E_r(w)$, dove $\lambda$ è il peso della regolarizzazione.
# Obiettivi
- Mitiga il problema dell'overfitting: abbassa la varianza della funzione impara, rendendola più smooth e più semplice.
- Se la funzione del rischio empirico contiene la lakelihoow, cioè l'errore sui dati, la regolarizzazione puà contenere la priori, cioè quanto già abbiamo imparato sui dati. Questo è utile in problemi di [Continual learning](Continual%20learning.md).
# Lasso
È il regolarizzato dove i pesi sono in valore assoluto.
Stiamo chiedendo di avere soluzioni sparse.
# Ridge
È un regolarizzatore di tipo di Tikhonov, cioè i pesi sono in norma al quadrato.
Stiamo chiedendo di avere una soluzione dove i pesi sono simili tra loro.