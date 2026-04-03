Il processo di arrivo di Poisson è un processo contatore che modella il numero di arrivi (i.e., eventi) in un intervallo di tempo $[0,t]$, dove questi  sono IID (indipendenti e identicamente distribuiti secondo una [distribuzione esponenziale](distribuzione%20esponenziale.md).
Si definisce come $\mathbb{P}=\{N(t), t\in\mathbb{R}\}$, dove $N(t):=$numero di arrivi entro $t$.
# Esempio
Nonostante nella realtà sia difficile da riscontrare, esistono delle situazioni in cui questo si verifica: se abbiamo una popolazione infinita e ogni utente esegue la stessa azione in modo indipendente (secondo una distribuzione indipendente l'una dall'altra) allora il tempo di arrivo dell'azione segue il processo di Poisson.
# Proprietà
- Ha una forma chiusa: $Prob\{N(t)==n\}=\tfrac{(\lambda t)^n}{n!}e^{-\lambda t}$.
- La somma di processi di Poisson è un processo di Poisson. Questo è importante perché se abbiamo più arrivi che seguono un processo di Poisson allora possiamo definire un solo processo di arrivo sommando i loro tassi.
# Esempio
![processo di arrivo di poisson](MM1.jpeg)
Questo modello è molto semplice ma rappresenta nella teoria delle code (usando la notazione di Kernall) un modello M/M/1: il processo di rilascio e il servizio sono Markoviani (perché i loro tempi di rilascio e di consumo seguono un'esponenziale) e abbiamo un solo server. Il singolo server gestisce un job che si trova in Queue alla volta.