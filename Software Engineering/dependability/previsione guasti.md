Le banche dati sono dei documenti tecnici (non sono norme) nati in ambito militare con la Military Handbook del 1965 in America, un manuale che contiene un insieme di modelli matematici che permettono di fare [previsione](Software%20Engineering/dependability/misure.md#Analitico) di affidabilità.
È importante notare che nessuna banca dati identifica i motivi del guasto.
# Supposizione
Il military handbook si basa su delle ipotesi fondamentali:
- Tasso di guasto costante
- Guasti indipendenti
- La configurazione dei componenti è quella in [serie](reliability%20block%20diagram.md#Serie). Questo non limita l'analisi perché se la inseriamo in RBD possiamo calcolare l'affidabilità del nostro sistema con la nostra configurazione.
- Il modello di degrado del componente è quello di Arrhenius, il quale afferma che all'aumentare della temperatura aumenta il tasso di guasto.
# Modello
Il Military Handbook calcola il tasso di guasto di un componente come $\lambda=f(\lambda_{g},\pi_{i})$, cioè in funzione del tasso di guasto base $\lambda_{g}$, ottenuto da esperimenti, e alcuni valori correttivi $\pi_{i}$, che tengono conto delle condizioni di uso specifiche (e.g., qualità, temperatura di utilizzo, progettazione e ambiente di utilizzo $\pi_E$).
Ci sono due modi per eseguire il calcolo del fattore di guasto $\lambda$.
##### Part count
Serve per farsi una prima idea sull'utilizzabilità dei componenti: si usa in fase di progettazione senza sapere come e quanto essi verranno stressati.
Si ottiene $\lambda=\sum_{i}{N_{i}(\lambda_{g}\Pi_{q})_{i}}$, dove: la somma è sul numero di elementi del componente; $N_{i}$ è la quantità dell'elemento all'interno del componente; $\lambda_{g}$ è il failure rate di base del componente; $\Pi_{q}$ è il fattore relativo alla qualità dei componenti.
##### Part stress
Si applica nella fase finale di progettazione conoscendo il [livello nominale](1-intro.md#Range%20in%20specifica) di stress a cui è sottoposto ogni componente. L'idea in quasta fase è di fare derating, cioè non andare a trovare il punto di rottura.
# Ambiente di utilizzo
Il parametro più importante che definsce il modello è forse $\pi_{E}$, visto che i parametri [RAMS](attributi.md) fanno riferimento all'ambiente in cui usiamo il sistema.
Sono definiti 14 ambienti operativi di cui i più importanti sono i primi tre.
- Ambiente fisso protetto (GB – Ground Benign)
	È l'ambiente dei lavoratori, dove le temprature sono controllate e non abbiamo nessuna sollecitazione meccanica.
- Ambiente fisso non protetto (GF – Ground Fixed)
	È l'ambiente in cui le sollecitazione, termiche e meccaniche, sono minime e sono relative all'utilizzo del sistema in una stanza.
- Ambiente mobile (GM – Ground Mobile)
	È l'ambiente in cui sono presenti sollecitazione termiche e meccaniche più elevate.
- Altri
	Ci sono poi due ambiente navali (i.e., sopra e sotto coperta), due ambienti millistici (i.e., in volo e in fase di lancio), un ambiente spaziale e un lancio da cannone.
# Military handbook 217
È la prima banca dati realizzata (MIL-HDBK-217) che non riceve aggiornamenti del proprio [modello](#Modello) di calcolo dal '95; nonostante questo sono aggiornati i guasti base $\lambda_{g}$ dai produttori e quindi è ancora utilizzata.
##### Duty cycle
Il modello considera un duty cycle del 100%: il sistema è considerato operativo 24/7. Siccome non sempre è vero si deve definire il tasso di guasto reale separando le fasi di operatività e di non operatività: $\lambda=\lambda_{operating}\cdot DT+\lambda(100-DT)$, dove $DT$ è il duty cycle reale.
##### Derivate
A parte dalla MIL-HDBK-217 ci sono moltre altre banche dati. Le banche dati differiscono per dominio di utilizzo e per modello di calcolo.
- SAE
	È relativa al settore automotive.
- CNET
- PRISM
	Definsice il modello di calcolo sulla base di modelli bayesiani. È l'unico che include fattori che rappresentano le condizioni non operative.
- IRPH
	È nato dalla collaborazione tra molte aziende europee ed è l'unico che considera i fattori presenti nella norma IEC61709.
	Inoltre da informazioni sui possibili motivi di guasto.
- SN 29500
	È la banca dati della Siemens, azienda importante nell'automazione industriale.
- 217 PLUS
	È un'evoluzione della MIL-HDBK-217 e definisce il tasso di guasto in base di più fattori correttivi e questo permette di ottenere un tasso guasto meno conservativo.
	Un fattore consdierato è il processo di costruzione. Altri contributi fisici sono dati dall'operatività, l'ambiente in cui viene utilizzato, la presenta si software.
- Telcordia
	È un modello che viene usato sempre di più perché semplifica di molto quelli derivati dal MIL-HDBK. Questo è un vantaggio visto che il numero di fattori è ridotto, ma il risultato è un tasso di guasto più conservativo.
	Rispetto agli altri modelli ha una opzione che permette di settare un moltiplicatore per il primo anno in modo da considerare la mortalità infantile, tenendo conto della prima parte della curva vasca da bagno. Inltre non solo definisce il tasso di guasto, ma da anche indicazioni sull'intervallo di confidenza.
	Si basa su tre metodi che sono selezionabili nel software:
	- Part count
		Non utilizza dati di campo.
	- Metodo 1
		Utilizza dati proveniente da test di laboratori e un modello basato su modelli bayesiani.
	- Metodo 2
		Utilizza dati di campo ed è metodo più completo.
- FIDES
	Nasce nel settore aerospaziale ed è una banca dati abbastanza complessa per cui è da usare solo nell'ambito in cui è nata. La stima è data da $\lambda=\lambda_{phisical}\Pi_{PM}\Pi_{process}$, dove: $\lambda_{phisical}=\sum_{phisical\ contribution}(\lambda_{0}\Pi_{acceleration})\Pi_{induced}$, dove $\Pi_{acceleration}$ rappresenta tutti i possibili stress e non solo quelli di Arrhenius (e.g., termici, elettrici, meccanici, di umidità, chimici), $\Pi_{induced}$ rappresenta i possibili stress che non sono stati inseriti nelle specifiche (cosa verificabile in un contesto aerospaziale); $\Pi_{PM}$ rappresenta la qualità del produttore e dei componenti; $\Pi_{process}$ rappresenta la gestione del processo di produzione.
- NSWC
	È un modello per l'ambiente meccanico, che è più complesso perché i guasti possono verificarsi per tantissimi motivi e spesso esso dipende dalla manutenzione e usura.
	Ha un modello di calcolo specifico per ogni componente, visto che ognuno di essi dipende anche dalle proprie caratteristiche fisiche. In generale tutti comunque si basano su un tasso di guasto base con dei fattori correttivi (e.g., pressione, vistosità del fluido in cui sono usati). La particolarità da osservare è che [supporre](#Supposizione) un tasso di guasto costante nei componenti meccanici ridurre la finestra in cui il valore stimato è valido.
- NPRD
	È un modello per l'ambiente meccanico, più complesso del precedente perché non mette a disposizione un modello di calcolo, ma mette a disposizione dati grezzi per eseguire analisi. È praticamente utilizzabile solo da esperti.
- OREDA
	Nasce per il settore Oil&Gas e si divide in applicazioni in superficie sott'acqua.
	La sua particolarità è che da informazioni sulli manutenzioni, indicando ad esempio quando sono state richieste e un range di time to repair.