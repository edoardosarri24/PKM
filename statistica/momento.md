Data una [variabile casuale](random%20variable.md) $X$, il suo momento di ordine $n$ è definito come $E[X^n]=\int_{-\infty}^{+\infty}x^nf_X(x)dx$.
Informalmente questo integrale si può vedere anche come l'integrale di $x^n$ pesato per il valore della PDF $f_X(x)$ nel pezzetto $dx$.
# Primo ordine
Il momento di primo ordine è il valore atteso ed è definito come $\mu_X=E[X]=\int_{-\infty}^{+\infty}xf_X(x)dx$.
Indica dove la distribuzione è centrata.
##### Media
La differenza è che la media è un valore misurato, cioè ottenuto da esperimento. Il valore atteso solitamente è la collocazione dei dati in una distribuzione.
# Secondo ordine
Il momento di secondo ordine è definito come è definito come $E[X^2]$, cioè il valore atteso del quadrato.
##### Varianza
La varianza è definita come $\sigma_X^2=E[X^2]-E^2[X]=\int_{-\infty}^{+\infty}(x-\mu_X)^2f_X(x)dx$ e indica la dispersione della distribuzione rispetto alla media, cioè quanto i dati sono distanti da un valore misurato empiricamente; è una misura di dispersione assoluto.
Siccome la varianza è una quantità quadratica, non è paragonabile con il valore atteso. Per questo motivo spesso si considera la deviazione standard, cioè $\sigma_X=\sqrt{\sigma_X^2}$.
# Coefficiente di variazione
Indica la dispersione della distribuzione rispetto al valore atteso, cioè quanto i dati sono distanti da un valore teorico); una misura di dispersione relativa. Un valore basso indica che i dati sono concentrati vicino al valore atteso $\mu_X$.
È definito come il rapporto tra deviazione standard e valore atteso, cioè è $CV_X=\tfrac{\sigma_X}{\mu_X}$.