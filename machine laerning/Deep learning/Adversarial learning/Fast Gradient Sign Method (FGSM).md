È un tipo di adversial attack che si può fare in contesti di tipo [white box](Adversarial%20learning.md#Conoscenza) e [untargeted](Adversarial%20learning.md#Target).
L'obiettivo è variare l'input di un dato modello e generare un esempio avversario, cioè un esempio che il classificatore classifica non correttamente.
# Funzionamento
Definiamo la loss per addestrare la rete come $L(\theta,x,y)$.
Il rumore $\eta$ per uno specifico input $x$ è definito come $\eta(x)=\epsilon sign(\nabla_xL(\theta,x,y_{true}))$. Quello che stiamo facendo è non aggiornare i pesi $\theta$ come nella classica ottimizzazione, ma è trovare la direzione in cui modificare l'input per massimizzare la loss in modo da confondere il modello.
##### Budget
Il valore $\epsilon$ è detto budget e ci dice la scala di quanto cambiare l'input.
È importante lavoare con un budget limitato; se questo infatti fosse alto si genererebbe un'immagine totalmente diversa, invece vogliamo che l'input corrotto non sia visibile a occhio nudo.
Si è visto che per valori molto piccoli (0.2) si possono avere grandi errori nella classificazione dell'input.
# Linearità
Il motivo per cui questa cosa funziona è la linearità locale dei modelli.
Semplificando infatti le uniche operazioni non lineari sono le funzioni di attivazione. Se prima si usava la [Sigmoid](Neurone%20artificiale.md#Sigmoid), adesso si usa prevalentemente la [ReLU](Neurone%20artificiale.md#ReLU); queste funzioni sono non lineari globalmente, ma sono lineari localmente cioè in un piccolo intorno di $x$.
Variando $x$ molto poco, in modo che a occhio nudo non si veda. Questo discorso della linearità permette di ottenere una grande variazione dell'output a piccoli cambiamenti dell'input.
# Miglioramenti
L'algoritmo si può tradurre in una versione iterativa come $\tilde{x}_{t+1}=\tilde{x}_t+\tfrac{\epsilon}{T}sign(\nabla_xL(\theta,x,y_{true}))$.
Stiamo usando [gradient descent](Gradient%20descent.md). Come sappiamo questo metodo è rumoroso. Siccome in questo caso facciamo il gradiente rispetto all'input $x$ per essere sicuri che ci porti da qualche parte possiamo usare MI-FGMS che è la larsione di FGSM con il momentum.