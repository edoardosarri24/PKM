Un bias è definito come un errore sistematico nel processo decisionale che porta a un risultato [unfair](Fairness.md).
Nel mondo del ML la previsione del modello viene presa basandosi sui dati del train set; se questi dati contengono dei bias allora l'outcome sarà unfair.
- Un esempio è che se passiamo una foto di un matrimonio occidentale allora ci dice che è un matrimonio, ma la foto è di un matrimonio orientale allora ci dice che sono persone.
- Un altro esempio è che se i dati esprimono (e succede davvero) che le donne hanno uno stipendio più basso, allora i modelli di raccomandazione propongono alle donne dei lavori con stipendio più basso.
- Se un dataset contiene foto di lupi dove nella maggior parte i lupi sono sulla neve, allora se chiediamo di classificare un Husky sulla neve ci darà che è un lupo.
# Bias nei dati
Vediamo alcuni tipi di bias nei dati.
- Sampling
	È detto bias di campionamento è succede quando il train set non è ottenuto in modo casuale dalla distribuzione della popolazione, ma ci sono alcune categorie di individui che hanno più probabilità di essere campionate.
	È quindi un problema di come sono raccolti i dati.
- Representation
	Avviene quando ci sono delle sotto popolazioni che sono rappresentate meno. È un concetto molto simile al sampling, ma qua l'attenzione è sulle sotto popolazioni e non sul campionamento in generale
- Artefatti
	Si presenta quando c'è una correlazione tra artefatti (e.g. patch in una foto) e classe di appartenenza del sample.
	In pratica il modello usa l'artefatto per classificare i futuri samples.
- Aggregazione
	Si presenta quando l'output individuale viene presdeciso considerando l'intera popolazione.
	Un esempio è il [paradosso di Simpson](Bias.md#Paradosso%20di%20Simpson).
##### Paradosso di Simpson
Il paradosso di simpson si verifica quando una tendenza osservata in più gruppi separati si inverte o scompare quando i dati vengono aggregati. Questo è dovuto al fatto che la distribuzione all'interno dei gruppi è sbilanciata, cioè quando ci sono più sample in una categoria rispetto a un altra.
Una cosa che può portare al paradosso di Simpson sono le variabili di confusione (o fattori di confusione): è una variabile che non è stata considerata inizialmente ma che influenza sia le variabili dipendenti che quelle indipendenti.
- Vediamo intanto un [esempio](esempio%20paradosso%20di%20simpson.pdf). Se guardiamo il risultato aggregato allora vediamo come il trattamento B sia migliore del trattamento A; se però osserviamo ogni categoria dei due trattamenti allora il gruppo A è sempre migliore del gruppo B. Vediamo che nell'esempio il trattamento A cura molti più casi difficili del trattamento B e questo porta a ribaltare la situazione nei casi curati quando si aggregano i dati. Se osserviamo la variabile di confusione vediamo che la dimensione dei calcoli deve essere considerata, perché questa a quanto pare influenzare sia il risultato (curato/non curato) ma anche la terapia a cui il paziente è sottoposto (ci sono più pazienti con i calcoli grandi curati con la terapia A).
##### Sure-thing principle
Il paradosso di Simpson porta a questo principio.
Un'azione $A$ che aumenta la probabilità di un evento $E$ in ogni sotto popolazione, la deve aumentare anche nella popolazione nel suo insieme. Questo può non valere solo nel caso in cui l'azione cambia la distribuzione della sotto popolazione, cioè se l'azione modifica quanti individui appartengono a ogni gruppo (cioè se l'azione è una variabile di confusione).
# Bias negli algoritmi
Vediamo alcuni tipi di bias negli algorimi.
- Algoritmico
	Non ci sono bias nei dati, ma è l'algoritmo che introduce dei bias per come è costruito.
- D'interazione
	Quando il bias proviene dall'interazione dell'algoritmo verso alcune categorie di utenti.
- Di conferma
	Se chi ha progettato l'algoritmo ha introddo un bias perché il suo pensiero è affetto da bias.
- Generativo
	Se i dati hanno dei bias e un modello generativo genera risposte affetti da questi bias. È strettamente legato ai modelli generativi.