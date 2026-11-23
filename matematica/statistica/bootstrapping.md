
Si tratta di un approccio statistico data-driven per ottenere l'incertezza di una stima (e.g., la media o il parametro di un modello). Invece si usare formule teoriche dove è necessaria una prior (i.e., tecniche parametriche) si utilizzano i dati che si hanno disposizione per studiarne la [confidenza](bande%20di%20confidenza.md).
# Funzionamento
Si basa sul campionamento con reinserimento.
L'idea è di trattare il campione empirico (i.e., i dati a disposizione ottenuti tramite esperimenti) come se fosse la popolazione vera. Si estraggono $N$ samples con reinserimento dalla popolazione (quella che in realtà è la campionaria); si calcola il parametro di interesse (e.g., media) su questo nuovo campione; ripetiamo $B$ volte e avremo $B$ valore del parametro, cioè una sua distribuzione che approssima il parametro. A questo punto possiamo prendere la media, possiamo fare un intervallo di confidenza basando sull'IQR o altre cose.
# Utilizzi
- Quando si dati sono pochi e difficili/costosi da ottenere.
- Quando si devono calcolare misure statistiche complesse. Per valutare l'incorrettezza di una media ci sono metodi in forma chiusi consolidati, ma se l'oggetto della stima è più complesso allora è un problema.
- Quando non abbiamo nessuna prior sulla distribuzioe dei dati reali.