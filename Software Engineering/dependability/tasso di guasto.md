È definito come il numero guasti su unità di tempo.
A parte per il caso [costante](#Costante), il tasso di guasto può essere rappresentato tramite più [distribuzioni](failure%20rate%20distribution.png); l'idea è rappresentare le varie zone (o l'intera) della bath curve tramite.

Ognuna di essa può essere rappresentata da funzioni diverse: PDF, CDF, reliability associata e tasso di guasto. È importante notare come esse siano tutte correlate e che quindi basta conoscerne una per poterle rappresentare tutte.
##### Bath curve
In generale il tasso di guasto un andamento che segue la [bath curve](bath%20curve.png):
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
	- Tenendo fisso $\beta$, il [comportamento](weibull%20distribution%20eta.png) di $\eta$ fa:
		- Variare l'ampiezza della campana nel caso di $\beta>1$.
		- Alzare l'inizio dell'esponenziale se $\beta<1$.
- Shape $\beta$
	Deve essere $\beta>0$. Il [comportamento](weibull%20distribution%20beta.png) dipende da:
	- Con un $\beta$ basso (i.e., $\beta \to 0$) si ha una distribuzione più simile a un'esponenziale.
	- Con un beta alta (i.e. $\beta>1$) ci avviciniamo a una normale.
- Posizione $\gamma$
	Permette di [spostarsi](weibull%20gamma.png) sull'asse temporale delle ascisse e introduce una soglia entro cui i guasti non possono sicuramente accedere.
	- Solitamente $\gamma<0$ indica il tasso di guasto dopo una manutenzione: il componente non è nuovo e quindi al tempo zero il tasso di guasto non sarà 0.
	- Solitamente $\gamma>0$ indica che il componente è già stato testato per un po' di tempo.
##### Guasto
Con questa distribuzione il tasso di guasto varia in [questo modo](failure%20rate%20weibull.png); si nota che tenendo fisso $\eta$ e variando $\beta$ (prima immagine) si ottenono tutte e tre le zone della vasca da bagno.
Solitamente si utilizza una funzione con $\beta<1$ per descrivere la prima zona della vasca da bagno e una seconda con $\beta>1$ per descrivere la seconda e la terza.
# Normale
La [distribuzione gaussiana](distribuzione%20gaussiana.md) è utilizzata in questo ambito per rappresentare l'ultima parte della bath curve; possiamo [vedere](gaussiana%20tasso%20di%20guasto.png) infatti che descrive bene la zona in cui i guasti aumentano. Si adatta quindi bene a quei componenti per cui il tasso di guasto aumenta fin da subito.
# Lognormale
Si tratta della random variable relativa al logaritmo del tasso di guasto e non del semplice tasso di guasto.
È rappresenta dalla normale dove alla dimensione tempo abbiamo applicato il logaritmo e quindi abbiamo che la distribuzione è più schiacciata verso i valori più piccoli, il che la rende ottima per [rappresentare](tasso%20di%20guasto%20lognormale.png) la prima parte della vasca da bagno.
# Test distruttivi
Per decidere quale distribuzione utilizzare ci servono dei dati sul tempo di guasto. I test che ci portano a questi risultati sono i test distruttivi: sono test accelerati, cioè prove dove si superano i valori nominali dei componenti per ridurre il tempo necessario a osservare l'effetto della sollecitazione, che solitamente portano il sistema a guastarsi.
##### Classificazione
Ci sono diversi modi in cui portare avanti questi test e quindi diversi dati che posso ottenere:
- Tempi di guasto
	Il tempo di guasto può essere descritto in modo assoluto o tramite un intervallo.
	Ottenere il tempo preciso è meglio, ma è anche più costo perché devo monitorare costantemente i componenti sotto esame. Se monitoro i componenti ogni intervallo di tempo, allora è meglio che questi intervalli siano il più piccoli possibili.
- Numero di guasti
	Se faccio guastare tutta la popolazione sotto esame allora ho un test esaustivo, dove per ogni componente ho dei dati.
	Se faccio andare avanti l'esperimento fino a un certo tempo $t$ allora avrò un dataset censurato, dove solo alcuni componenti hanno il relativo tempo di guasto.
	- Censura destra di tipo 1
		Fisso la durata del test e alla fine avrò dei componenti guasti e altri ancora sani.
		Ho il vabtaggio di sapere quanto ci vuole per il test; lo svantaggio è che questo potrebbe risultare non coclusivo perché si sono guastiti pochi componenti.
	- Censura destra di tipo 2
		Fisso il numero di guasti che voglio raggiungere.
		Ho lo svantaggio di non sapere quanto dura il test; ho il vantaggio che alla fine avrò un risultato conclusivo.
	- Censura a sinistra
		Non ci diamo un limite né di tempo né di guasti, ma si fanno delle valutazioni a intervalli di tempo.
##### Zero guasti
Nel caso alla fine dei test non si sono ottenuti guasti, probabilmente abbaiamo sbagliato qualcosa o nella scelta dei fattori di stress o nel tempo del test.
In questo caso quello che possiamo fare è utilizzare una distribuzione chi-quadro, dove il tasso di guasto è dato da $\chi=-\frac{\ln\alpha}{\text{total text hours}}$. In ogni caso questa è una stima molto grezza e incerta.