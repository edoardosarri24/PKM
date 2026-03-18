È definito come il numero guasti su unità di tempo.
##### Distribuzioni
A parte per il caso costante, il tasso di guasto può essere rappresentato tramite delle distribuzioni. Ognuna di essa può essere rappresentata da funzioni diverse: PDF, CDF, reliability associata e tasso di guasto. È importante notare come esse siano tutte correlate e che quindi basta conoscerne una per poterle rappresentare tutte.
# Bath curve
In generale ha un andamento che segue la [bath curve](bath%20curve.png):
- La prima zona è relativa ai difetti di fabbrica e alla mortalità infantile.Abbassare il tasso di guasto in questa zona vuol dire diminuire il numero di prodotti restituiti che sono ancora in garanzia e quindi diminuire i costi per l'azienda. Una tecnica è quella dello [stress screening](Software%20Engineering/dependability/misure.md#Classi).
- La zona di mezzo è quella dove il prodotto si è stabilizzato.
- L'ultima zona è relativa all'usura.
# Costante
Una semplificazione del tasso di guasto è quella di considerarlo costante durante tutto il ciclo di vita del sistema. Questa è una semplificazione che nella realtà, soprattutto in sistemi dove è presente l'usura, non è così.
##### A regime
Il tasso di guasto non sempre è costante, ma c'è una situazione dove anche nella realtà possiamo fare questa assunzione senza troppi problemi: nella fase centrale della [bath curve](bath%20curve.png), quando il sistema si è stabilizzato.
Questa fase per i componenti elettronici questa durata è più lunga rispetto ai componenti meccanici.
# Esponenziale
In questo caso $\lambda$ non dipende dal tempo (i.e., $\lambda(t)=\lambda$) e quindi le formule si semplificano molto:
- La reliability è data da $R(t)=e^{-\lambda t}$.
- La CDF è data da $F(t)=1-e^{-\lambda t}$.
- La PDF è data da $f(t)=\lambda e^{-\lambda t}$.
	Questo è molto come il valore della [reliability](attributi.md#Reliability) nel caso di tasso costante con un fattore $\lambda$.
- Il MTTF in questo caso è $MTTF=\frac{1}{\lambda}$.
##### Svantaggi
Il problema principale è la prorprietà di [memoryless](distribuzione%20esponenziale.md#Memoryless) della distribuzione esponenziale: un componente con tasso di guasto esponenziale non sa da quanto sta lavorando. In pratica non stiamo considerando l'invecchiamento del componente: questo va solitamente bene per componenti elettronici, ma non per componenti meccanici soggetti a usura.
# Weibull
È la distribuzione che approssima meglio il tasso di guasto nella maggior parte dei casi grazie alla presenza di parametri che possiamo settare.
##### Parametri
I parametri sono:
- Scale $\eta$
	È un parametro dimensionale con unità di tempo le ore.
	- Deve essere $\eta>0$. Solitamente è nell'ordine di $10^3$.
	- Tenendolo fisso tutte le curve al variare di $\beta$ partono dallo stesso punto, detto characteristic life point.
	- Tenendo fisso $\beta$, il [comportamento](weibull%20distribution%20eta.png) di $\eta$ fa variare l'ampiezza della campana nel caso di $\beta>1$ o fa alzare l'inizio dell'esponenziale se $\beta<1$.
- Shape $\beta$
	Deve essere $\beta>0$. Il [comportamento](weibull%20distribution%20beta.png) dipende da: con un $\beta$ basso (i.e., $\beta \to 0$) si ha una distribuzione più simile a un'esponenziale; con un beta alta (i.e. $\beta>1$) ci avviciniamo a una normale.
- Posizione $\gamma$
	Permette di [spostarsi](weibull%20gamma.png) sull'asse temporale delle ascisse e introduce una soglia entro cui i guasti non possono sicuramente accedere.
	Solitamente $\gamma<0$ indica il tasos di guasto dopo una manutenzione: il componente non è nuovo e quindi al tempo zero il tasso di quasto non sarà 0.
	Solitamente $\gamma>0$ indica che il componente è già stato testato per un po' di tempo.
##### Guasto
Il tasso di guasto varia in [questo modo](failure%20rate%20weibull.png) con questa distribuzione.
Si nota che tenendo fisso $\eta$ e variando $\beta$ (prima immagine) si ottenono tutte e tre le zone della vasca da bagno.
Solitamente si utilizza una funzione con $\beta<1$ per descrivere la prima zona della vasca da bagno e una seconda con $\beta>1$ per descrivere la seconda e la terza.
# Normale