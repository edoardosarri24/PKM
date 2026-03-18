Nella [dependability](dependability.md) gli attributi sono le proprietà devono essere verificate per avere un'affidabilità del sistema. In sistemi diversi ogni attributo ha un peso diverso, eventualmente anche zero.
Avendo essi una dimensione quantitativa, ci danno inoltre un modo per confrontare due sistemi. In questo scenario devono essere visti non come misure binarie, ma come probabilità.
##### Misure
Oltre a quelle specifiche dei singoli attributi, altre misure che comprendo più attributi sono:
- Degrado
	In sistemi composti da più nodi è il livello di nodi online.
- Performability
	È la misura del valore del servizio offerto.
##### Tempo
L'affidabilità e in generale molte misure sono in funzione del tempo. È difficile dire se un valore è alto o basso, ma possiamo contestualizzarlo meglio se parliamo di quando abbiamo riscontrato quel valore.
# Availability
È intesa come la disponibilità, cioè la continuità di servizio durante tutto il tempo a disposizione.
Se il sistema ha bisogno di un'alta Availability allora vogliamo che esso sia online la maggior parte del tempo. I sistemi con più alta avaibility al momento sono rete elettrica e i data center.
##### Misura
La misura di availability $A(t)$ è definita come la probabilità che al tempo $t$ il sistema sia online.
Può essere definita in modo adimensionale con un valore in $[0,1]$ come $A=\tfrac{MTBF}{MTBF+MTTR}$.
# Reliability
È la capacità di un sistema di svolgere la sua funzione nel modo corretto per un determinato periodo di tempo e nelle condizioni definite dai requisiti. Se consideriamo solo i due stati del sistema di corretto ed errato funzionamento, allora la reliability è una misura del tempo consecutivo passato nello stato corretto 
Se il sistema ha bisogno di un'alta reliability (e.g., aerei) allora stiamo chiedendo che quando il sistema è online esso funzioni senza [fallimenti](fault%20error%20failure%20chain.md#Failure).
##### Misura
La misura di realiability $R(t)$ è definita come la probabilità che, dato un sistema senza fallimenti in $t=0$ al tempo $t$ il sistema non abbia mai avuto un fallimento. Si misura con la legge fondamentale della reliability $R(t)=e^{-\int_{0}^{t}\lambda(t)dt}$: essa dipende quindi dal tempo ed è in funzione del [tasso di guasto](tasso%20di%20guasto.md) $\lambda$; è il massimo in $t=0$ dove vale 1 ed è una funzione monotona decrescente.
- Supponendo un [tasso di guasto sempre costante](tasso%20di%20guasto.md#Costante) allora la reliability diventa $R(t)=e^{-\lambda t}$, e quindi [questa](reliability%20with%20constant%20fault%20rate.png) è la curva. Ovviamente in questo caso vogliamo un $\lambda$ più piccolo possibile.
- Supponendo un tasso di guasto che segue la distribuzione di [Weibull](tasso%20di%20guasto.md#Weibull), possiamo notale l'[andamento](reliability%20with%20weibull.png) della reliability.
##### Software
Nel software la realibilty è spesso molto [correlata](failure%20rate%20software.png) al testing e ai nuovi rilasci. Possiamo dire appena abbiamo un una nuova release i la reliability diminuisce (cresce il failure rate) e via via diminuisce quando si fa bub fixing.
##### Residual reliability
Si parla di residual reliability quando si vuole calcolare la realiability per $t=MTTF$. Abbiamo che $R(t=MTTF)\to0$ ma non è mai 0.
- Singolo elemento
	Se consideriamo un singolo componente (cioè una configurazione in [serie](reliability%20block%20diagram.md#Serie) con $n=1$) con un tasso di guasto costante allora $R(t)=R(MTTF)=e^{-\lambda\cdot MTTF}=e^{-\lambda\tfrac{1}{\lambda}}=e^{-1}=0,36$: abbiamo quindi che il 37% dei sistemi sono ancora funzionanti.
- Parallelo (2 elementi uguali)
	Se applichiamo la regola per definire la reliability nel caso [parallelo](reliability%20block%20diagram.md#Parallelo) abbiamo che $R(t)=2e^{-\lambda t}-e^{-2\lambda t}$ e allora $R(MTTF)=0,397$: abbiamo aumentato la reliability e del $MMTF=\tfrac{3}{2\lambda}$.
- 2/3 (elementi uguali)
	Abbiamo che $R(t)=2e^{-\lambda t}-e^{-2\lambda t}$ e quindi $R(MTTF)=0,402$ con $MMTF=\tfrac{5}{6\lambda}$.
# Safety
La [safety](Software%20Engineering/dependability/safety.md) è un'estensione del concetto di [reliability](#Reliability): lo stato corretto del sistema è quello in cui il sistema funziona correttamente unito a quello dove non funziona correttamente, dal punto di vista delle specifiche funzionali, ma non si verifica conseguenze per la salute delle persone.
##### Misura
Si misura con il SIL (Safe Integrety Level) ed è il livello di assenza di rischi che coinvolgono la vita umana. Non è la sicurezza intesa come security (i.e., protezione da eventi dannosi volontari).
##### Esempio
Un incrocio dove tutti i semafori sono rossi non va bene per la reliability ma va bene per la safety.
# Confidentiality
Il servizio non deve divulgare dati senza autorizzazione.
# Integrity
Il sistema non deve essere alterato in modo non corretto.
# Maintenability
È l'abilità di riparare il sistema dopo un problema, cioè di portare il sistema nuovamente nello stato di corretto funzionamento.
- Ciò che fa diminuire la manutenibilità è la complessità, l'età del sistema e la povertà di documentazione.
- Ciò che fa aumentare la maintenability è la corretta progettazione (e.g., [DFR](1-intro.md#Design%20for%20Reliability)): si deve prevedere come il sistema si danneggerà e come ripararlo; questo è vero soprattutto nel caso di componenti critici.
##### Misura
Se consideriamo solo i due stati del sistema di corretto ed errato funzionamento, allora la mantenability è anche espressa dal TTR (Time To Repair), cioè del tempo passato nello stato errato.
Nei sistemi che sono riparabili abbiamo:
- Mean time to failure (MTTF/MTBF)
	È il valore atteso del tempo in cui il sistema è in uno stato di corretto funzionamento espresso in ore.
	In generale abbiamo $MTTF=\int_0^\infty R(t)dt$; se abbiamo un [tasso di guasto costante](tasso%20di%20guasto.md#Costante) allora non si deve risolvere l'integrale e abbiamo $MTTF=\tfrac{1}{\lambda}$.
- Mean time to recovery (MTTR)
	È il tempo medio in cui il sistema è continuamente in uno stato errato e non funzionante.
- Mean time to repair (MTTR)
	È il tempo medio che è necessario per riparare effettivamente un guasto, cioè del tempo necessario per eseguire l'operazione.
##### Supportability
Legato alla mantenability abbiamo la supportability (o anche Integrated Logistic Support (ILS)): è la capacità di gestire a livello logistico (e.g., acquisto e trasporto) di tutto ciò che è necessario per la riparazione.
Una buona supportability riduce il MTTR (mean time to recovery). Inoltre è necessario garantire un livello di approvvigionamento delle scorte alto sopratutto per quei componenti che sono critici.
