Tramite la normalizzazione si mettono sulla stessa scala tutti i valori, in modo che la rete non sia maggiormente influenzata da valori alti rispetto a quelli bassi. L'obiettivo principale è quello di aumentare la stabilità e velocità di addestramento; inoltre l'addestramento non dipende in questo modo dalla scelta iniziale dei pesi.
# Come
Si esegue la normalizzazione sia di input che del target, per tutti gli split del dataset (i.e., trainign, validation e testing). I dati usati per calcolare le statistiche utili per normalizzare sono però solo quelli del training: si evita in questo modo data leackage.
# Tecniche
Ci sono diverse tecniche.
##### Min-Max
Avviene sottraendo il minimo e divendo per l'intervallo: $x_{norm}=\frac{x-x_{min}}{x_{max}-x_{min}}$ per portare i campioni in $[0,1]$; $x_{norm}=\frac{x-x_{min}}{x_{max}-x_{min}}\times_{2}-1$ per portare i campioni in $[-1,1]$.
- Si usa quando i dati hanno un range noto e preciso.
- Se possono arrivare dati fuori dal range, non è una buona normalizzazione.
##### Z-Score
Avviene sottraendo media e dividendo per la deviazione standard: $z=\frac{x-\mu}{\sigma}$.
- I campioni non hanno un range fisso.
- I campioni avranno media $\mu=0$ e varianza $\sigma^2=\sigma=1$.
##### Robust Scaler
Invece di usare media e deviazione standard per normalizzare si usano i quantili e la mediana: $x_{norm}=\frac{x-Q_{2}(X)}{Q_{3}(X)-Q_{1}(X)}$.
- Si usa quando nei dati ci sono molti outliers.
##### Non linear scaler
Si usano funzioni non lineari (e.g., logaritmo).
- Si usa quando la distribuzione dei dati non è normale.
