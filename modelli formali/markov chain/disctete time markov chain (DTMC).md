Una DTMC è una [markov chain](markov%20chain.md) dove il tempo è scandito da step discreti; il tempo in questo caso viene definito come il numero di transizioni che sono state eseguite.
# Proprietà
- Periodicità
	Uno stato ha periodo $d>1$ se il ritorno in esso una volta lasciato è possibile solo un numero di passi multiplo di $d$.
	Se $d=1$ allora lo stato è aperiodico.
# Soluzione
Per [risolvere](markov%20chain.md#Risoluzione) una catena di markov a tempo discreto ci servono due ingredienti:
- Vettore delle probabilità iniziali
	È il vettore $\pi^0=\pi(0)$. Il suo elemento $\pi_i(0)$ definisce la probabilità che all'inizio il sistema sia nello stato $i$-esimo.
- Matrice di transizioni
	Definisce la probabilità del prossimo stato dato lo stato attuale. È definita come $p_{ij}=P\{X(n+1)=j|X(n)=i\}$. È una matrice stocastica: la somma sulle righe è 1.
##### Soluzione transiet-probability
Si deve risolvere 
È l'equazione che ci restituisce la probabilità di essere in uno stato al prossimo istante temporale: $\pi(n+1) = \pi(0)P^n$ che si basa sull'iterazione dell'equazione di Chapman-Kolmogorov $\pi(n+1)=\pi(n)P$.
##### Soluzione steady-state
Se il sistema è aperiodico, irriducibile e il numero di stati è finito allora si deve calcola un sistema di equazioni lineari $\pi=\pi P\rightleftarrows P(P-I)=0$.
- Periodica
	Se la catena è periodica allora le probabilità continua a oscillare e non convergono mai.
- Riducibile
	Il sistema si stabilizza localmente: i valori a cui si stabilizza dipendono dallo stato iniziale.