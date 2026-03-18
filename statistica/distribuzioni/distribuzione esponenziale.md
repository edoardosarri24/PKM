La distribuzione esponenziale è la distribuzione base che modella la [random variable](random%20variable.md) che rappresenta il tempo che trascorre tra arrivi (i.e., eventi) consecutivi. Il duale di questa distribuzione è il [processo di arrivo di Poisson](processo%20di%20arrivo%20di%20Poisson.md), che conta il numero di arrivi in un intervallo di tempo; sono entrambe legate dal solo rate $\lambda$, 
Una [variabile casuale](random%20variable.md) $X$ è esponenziale con rate $\lambda$ ($EXP(\lambda)$) se:
- La [CDF](random%20variable.md#CDF) è $F_X(x))=Prob\{X\le x\}=\begin{cases}0 &\text{ se } x<0\\1-e^{-\lambda x} &\text{ se }x\ge0\end{cases}$.
- La [PDF](random%20variable.md#PDF) è $f_X(x))=\tfrac{d}{dx}F_X(x)=\begin{cases}0 &\text{ se } x<0\\\lambda e^{-\lambda x} &\text{ se }x\ge0\end{cases}$.
# Rate
Il rate, come si vede dati grafici della [PDF](PDF%20esponenziale.png) e della [PDF](CDF%20esponenziale.png), indica l'urgenza di un evento campionato da tale distribuzione: se abbiamo due esponenziali con $\lambda_1$ e $\lambda_2$, con $\lambda_1<\lambda_2$, allora la CDF di $EXP(\lambda_2)$ sarà più ripida, cioè la probabilità che il valore estratto da $EXP(\lambda_2)$ arrivi prima è più alta.
# Momenti
Vediamo i vari [momenti](momento.md) della distribuzione esponenziale.
- Il valore atteso è $\mu_X=\tfrac{1}{\lambda}$.
- La varianza è $\sigma_X^2=\tfrac{1}{\lambda^2}$.
- Il coefficiente di variazione è quindi sempre $CV_X=\tfrac{\sigma_X}{\mu_X}=1$.
# Proprietà
##### Minimo
Date due variabili casuali esponenziali $X_1$ e $X_2$ con rate $\lambda_1$ e $\lambda_2$, il loro minimo è una random variable $X$ esponenziale con rate $\lambda=\lambda_1+\lambda_2$; la PDF è definita come $f_X(x)=(\lambda_1+\lambda_2)e^{-(\lambda_1+\lambda_2)x}$.
- Questo ragionamento si estende a un set di random variable esponenziali.
- Se modelliamo questo tramite una [STPN](stochastic%20TPN.md) allora il minimo rappresenta una race selection (solo una delle due strade arriverà in fondo) e si ha [questo modello](minimo%20esponenziale.png).
##### Massimo
Date due variabili casuali esponenziali $X_1$ e $X_2$ con rate $\lambda_1$ e $\lambda_2$, il loro massimo non è una random variable esponenziale; la PDF è infatti definita come $f_X(x)=f_{X_1}(x)+f_{X_2}(x)-f_{min}(x)$.
- Se modelliamo questo tramite una [STPN](stochastic%20TPN.md) allora il massimo rappresenta una sincronizzazione e si ha [questo modello](massimo%20esponenziale.png).
##### Sommatoria
Date due variabili casuali esponenziali $X_1$ e $X_2$ con rate $\lambda_1$ e $\lambda_2$, la loro somma non è una random variable esponenziale.
- Se modelliamo questo tramite una [STPN](stochastic%20TPN.md) allora la sommatoria rappresenta una cascata di transizioni e si ha [questo modello](somma%20esponenziale.png).
##### Mixture
Date due variabili casuali esponenziali $X_1$ e $X_2$ con rate $\lambda_1$ e $\lambda_2$, la loro mixture (scelta secondo una qualche probabilità, eventualmente casuale) non è una random variable esponenziale.
- Se modelliamo questo tramite una [STPN](stochastic%20TPN.md) allora la mixture rappresenta una scelta tra due percorsi e si ha [questo modello](mixture%20esponenziale.png).
##### Memoryless
La proprietà ci dice che in una coda con waiting time esponenziale, il passare del tempo non incide sulle aspettative future, cioè il tempo rimanente segue la stessa distribuzione. La distribuzione esponenziale è l'unica distribuzione continua che gode della proprietà memoryless.
Facciamo un esempio. Se abbiamo una lampadina che si guasta secondo una distribuzione esponenziale e sappiamo dopo un tempo $x$ ancora non si è guastata, allora non possiamo dire che il guasto sarà più vicino rispetto al tempo 0.
- Questa proprietà è chiara dal [grafico](memoryless%20esponenziale.png) della PDF. Se facciamo passare il tempo $x$ e consideriamo da quel momento in avanti, allora la distribuzione deve essere ancora esponenziale, ma si vede come l'area sotto la PDF non sia 1; allora dobbiamo scalare la funzione, ottenendo la stessa funzione iniziale ma spostata di un valore $x$.
- La matematica che sta dietro questo è: data $X=Exp(\lambda)$ e sia $Y=X|X\ge x$, allora definita $Z=Y-c$ si ha $F_Z(z)=1-e^{\lambda x}$.
##### Random switch
Se abbiamo due distribuzioni esponenziali $X_i$ e $X_j$ con rate $\lambda_i$ e $\lambda_j$, allora $Prob\{X_i<X_j\}=Prob\{X_i\le X_j\}=\tfrac{\lambda_i}{\lambda_1+\lambda_j}$; è la probabilità che il valore campionato da $X_i$ siamo minore di quello campionato da $X_j$. Questo ragionamento si estende a un set di random variable esponenziali.
- Se X e Y campionano due valori secondo le loro distribuzioni esponenziali con i loro rate e io conosco il minimo di questi due valori, allora questa conoscenza non cambia la probabilità di chi ha campionato quel minimo.Si può scrivere formalmente come $Prob\{X_1<X_2|min\{X_1,X_2\}\le x\}=\tfrac{\lambda_1}{\lambda_1+\lambda_2}$.
- Se X e Y campionano due valori secondo le loro distribuzioni esponenziali con i loro rate e io conosco chi ha campionato il minimo di questi due valori, allora questa conoscenza non cambia la probabilità di quale sia questo minimo. Si può scrivere formalmente come $Prob\{min\{X_1,X_2\}\le x|X_1<X_2\}=1-e^{(\lambda_1+\lambda_2)x}$.
# Random generator
Partendo dalla distribuzione uniforme, tramite l'inversione della CDF della distribuzione esponenziale si possono generare numeri casuali.
Se prendiamo un valore $r\in[0,1]$ l'ascissa che $x$ tale che $F_X(x)=r$ è ottenibile in forma chiusa come $x=-\tfrac{ln(1-r)}{\lambda}$ ($r\in[0,1]$ perché senno $ln(1-r)$ non esiste); data la probabilità uniforme di scegliere $r\in[0,1]$, la probabilità di $r$ è la stessa di $1-r$; si ha allora $x=-\tfrac{ln(r)}{\lambda}$.