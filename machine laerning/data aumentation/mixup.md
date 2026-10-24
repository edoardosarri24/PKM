È una tecnica di data aumentation e [regolarizzazione](regolarizzazione.md) che supera l'assunzione per cui un campione per cui un campione è un dato a se stante.
- [Paper](https://arxiv.org/abs/1710.09412)
# Obiettivo
- Si vuole mitigare il problema dell'over confidence, impedendo che impari a memoria campioni di training.
- Si aumenta la generalizzazione.
- Aumento della robustezza all'OOD.
# Cosa perturbare
Solitamente ha senso perturbare sia l'input che il target.
# Funzionamento
1. Si campiona $\lambda$ da una Beta simmetrica: $\lambda=Beta(\alpha,\alpha)$.
	Una parte fondamentale è la scelta di $\alpha$: per valori di $\lambda$ vicini a 1 e/o 0 il dato non venga praticamente perturbato.
2. Si fa una combinazione convessa tra il campione sotto esame e un altro estratto a caso: $x_{mix}=\lambda x_{1}+(1-\lambda)x_{2}$ e $y_{mix}=\lambda y_{1}+(1-\lambda)y_{2}$.