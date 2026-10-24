È una forma di data aumentation e [regolarizzazione](regolarizzazione.md).
# Obiettivo
Aumentare la robustezza del modello rispetto alle variazioni del segnale o all'intensità dell'evento.
# Cosa perturbare
Solitamente ha senso perturbare sia l'input che il target. Se modifichiamo solo l'input allora perdiamo al relazione della forma che esso ha con il relativo target.
# Varianti
Quelle sotto possono variare anche a seconda della distribuzione da cui si campionano i fattori di scala.
##### Global amplitude scaling
Si aumenta sia l'input che il target di una certa percentuale: ad esempio si può fare $x=x\cdot \mathcal{U}(0.9,1.1)$.
Ha il vantaggio che si è indipendenti dal fattore di scala dei dati.
##### Channel amplitude scaling
Si aumenta sia l'input che il target di una certa percentuale in modo che l'auto che si fa su ogni canale sia indipendente: ad esempio si può fare $x_{c}=x_{c}\cdot \mathcal{U}(0.9,1.1)$.
Ha senso se i canali rappresentano grandezze fisiche diverse. Si potrebbe ad esempio raggruppare i canali per significato.
##### Time-varying amplitude scaling
Non si varia nello stesso modo lungo tutto l'intervallo, ma applica una soft loss, cioè una curva lunga quanto l'intera serie.
Ha senso se i dati sono temporali lunghe dove la precisione del sensore può cambiare nel tempo.