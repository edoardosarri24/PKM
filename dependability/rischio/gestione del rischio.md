Una volta che abbiamo definito il [rischio](rischio.md) è importante gestirlo.
La gestione del rischio è una [pipeline](gestione%20del%20rischio.png) strutturata in più fasi che sono eventualmente svolte in loop fino a che il sistema è online.
# Analisi del rischio
La prima fase è l'[analisi del rischio](analisi%20del%20rischio.md).
Alle tecniche che sono elencate è possibile aggiungere un'ottimizzazione detta [logica fuzzy](logica%20fuzzy.md), che permette di migliorare la tecnica a cui è applicata (e.g., [FMECA](FMECA.md)).
# Valutazione dei rischi
Una volta che ho capito quali sono i rischi devo rappresentarli con una delle tecniche sotto per capire quali sono i loro valori di rischio [accettabile](rischio.md#Rischio%20accettabile).
##### Curva del rischio
Una volta che ho le misure quantitative del rischio (i.e., danno e frequenza), posso creare delle classi (e.g., minore, significativo,pericoloso). Gli standard poi associano a ogni classe un tasso di rischio accettabile, che è inserito nelle specifiche del sistema e che deve essere rispettato.
Le classi di rischio sono riportate anche in grafici che rappresentano l'andamento della coppia $<criticità,frequrenza>$ e che hanno una forma di [questo tipo](curva%20del%20rischio.png): solitamente la forma non è diversa, altrimenti eventi molto gravi avrebbero una probabilità molto alta;  
##### Matrice dei rischi
L'idea della curva del rischio è che tutto quello che sta sotto è accettabile e tutto quello che sta sopra è non accettabile: in pratica non vengono ammesse delle zone di tolleranza.
Per questo motivo su usa spesso la matrice dei rischi: le celle che rappresentano punti sotto la curva sono verdi, sopra sono rossi, le celle che rappresentano i punti sulla curva sono gialli e rappresentano la zona di incertezza. Se siamo in uno di questi punti allora dobbiamo fare delle considerazioni con più attenzione per decidere se farli diventare versi oppure rossi.
Non esiste una rappresentazione univoca (i.e., potremmo usare anche 4 colori), per cui è sempre bene dettagliare matrice o curva con una precisa descrizione.
# Controllo del rischio
È la fase che comprende tutte le azioni che possiamo fare per mitigare i rischi e renderli [accettabili](rischio.md#Rischio%20accettabile). L'obiettivo è portare il rischio a un livello di tollerabilità, il quale dipende dal dominio.
##### Approcci
Ci sono due approcci che posso seguire:
- Prevenzione
	Riduco la probabilità di accadimento dell'evento. La probabilità è la parte più difficile da stimare: le persone hanno la tendenza a sovrastimare eventi che hanno una probabilità molto bassa, e sottostimare quelli con una probabilità molto alta.
- Protezione
	Riduco la gravità dell'evento che succede. La severity è la parte più difficile da ridurre.
##### Tecniche
Per ridurre il rischio ci sono principalmente quattro tecniche che dovrebbero essere usate tutte per arrivare a un livello di [rischio accettabile](rischio.md#Rischio%20accettabile).
 - Cambiamenti progettuali
	È la tecnica più efficace ed è l'unico modo per ridurre il fattore del danno del rischio. Visto che si deve cambiare parte del progetto, prima la facciamo e meno è costoso.
 - Dispositivi di sicurezza
	Possono essere attivi o passivi: i primi interagiscono direttamente con il sistema; i secondi interagiscono con l'ambiente in cui il sistema opera (e.g., transenne che limitano un percorso pericoloso).
	Una volta che abbiamo aggiunti i dispositivi di sicurezza, soprattutto per quelli attivi, è importante valutare i nuovi rischi introdotti.
 - Dispositivi di indicazione
	È una tecnica meno efficace perché va a impattare sulla probabilità di accadimento dell'evento e dipende da fattori esterni come le persone.
 - Formazione
	È la tecnica meno efficace perché dipende esclusivamente dall'essere umano.
# Valutazione rischio residuo
Dobbiamo ora calcolare il rischio residuo e valutare se adesso rientra all'interno dei valori accettabili usando la [matrice dei rischi](#Matrice%20dei%20rischi): vorremmo che le celle gialle fossero diventate verdi e che alcune di quelle rosse siano almeno gialle.
Dobbiamo prestare attenzione alle misure che facciamo: queste infatti possono avere degli errori e quindi portare a un ulteriore rischio.
Dobbiamo in questa fase valutare sia i rischi in modo locale (i.e., singolarmente), che tutti i rischi a livello globale: può capitare infatti che il singolo rischio sia accettabile, ma quando li andiamo a mettere tutti insieme otteniamo un rischio globale non accettabile.
# Rapporto
Si deve compilare il rapporto del rischio, un documento che monitora tutte le attività svolte fino a questo momento.
È importante che oltre ai dati, contenga le legende che spiegano come interpretare questi dati.
# Monitoraggio
Si devono raccogliere tutte le informazioni che derivano dal campo in modo da poter eventualmente rientrare nel loop e migliorare il sistema.
Se rientriamo nel loop in questa fase le conseguenze sono più costose: a seconda del dominio potremmo ad esempio (e.g., automotive) dover richiamare dei prodotti.