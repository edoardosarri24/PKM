È chiamato anche Additive White Gaussian Noise (AWGN) ed è una forma di data aumentation e [regolarizzazione](regolarizzazione.md) principalmente usata per dati sequenziali e/o serie temporali, cioè dati dove l'ordine dei dati è rilevante.
# Obiettivo
Aumentare la robustezza del modello su dati ottenuti da sensori non perfetti, rumori dell'ambiente o rumore ad alta frequenza.
# Cosa perturbare
Dipende dal tipo di sensore.
Se l'unico sensore non affidabile è quello dell'input, mentre quello del target è considerato perfetto, allora possiamo valutare di agire solo sull'input in modo che la rete impari a filtrare solo quello.
# Varianti
Ci sono varie formule e ognuna è diversa a seconda dei dati che stiamo elaborando.
Prendiamo come esempio un dato $x\in\mathbb{R}^{T\times C}$, dove $T$ è la lunghezza della sequenza e $C$ è il numero di canali/features.
##### Channel-scaled
$$x_{\text{noisy}}[t, c] = x[t, c] + \epsilon[t, c] \cdot \sigma_c \cdot \eta$$
- $x[t,c]$ rappresenta l'input del canale $c$-esimo al tempo $t$-esimo.
* $\epsilon[t, c] \sim \mathcal{N}(0, 1)$ è un sample di una [normale standard](distribuzione%20gaussiana.md#Normale%20standard) iid: questo vuol dire che non c'è relazione del rumore estratto dalla distribuzione al variare dell'input considerato.
* $\sigma_c$ è la deviazione standard della feature $c$-esima, calcolata su tutti i campioni e istanti del dataset di training.
* $\eta$ è l'iperparametro scalare `noise_level`. Settandolo ad esempio $\eta=0.05 stimao dicendo si sommare un rumore che pesato per il 5% della deviazione standard del canale.