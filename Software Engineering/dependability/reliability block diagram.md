Un sistema complesso per essere analizzato dal punto di vista della [dependability](dependability.md) deve essere scomposto in sottosistemi. In questo modo possiamo capire su quali componenti andare a lavorare e quali non impattano la nostra analisi.
Per scomporre un sistema complesso si utilizza il Reliability Block Diagram (RBD): si tratta di un diagramma definito da blocchi e archi indipendente dal dominio del sistema; i blocchi rappresentano i sotto sistemi e la scomposizione può essere a un livello più o meno alto; se stiamo analizzando un componente critico allora la scomposizione sarà massima.
##### Utilizzo
Il suo compito è quello di mettere in relazione le funzionalità del sistema con gli [attributi](attributi.md) RAMS, cioè di capire quale è il componente critico del sistema, dove è necessario cambiare [configurazione](#Configurazioni%20canoniche) per aumentare la reliability del sistema e valutare se il sistema rispetta i limiti economici.
Possiamo anche valutare una singola funzionalità del sistema per capire quali siano i componenti critici che realizzano quella funzionalità e quali siano invece quelli che non sono rilevanti. Pr fare questo si usa la FME(C)A.
##### Unicità
Per un sistema esistono più RBD a seconda del workflow (i.e., attività) che stiamo analizzando: due attività diverse attiveranno componenti diversi; per analizzare la dependability di quel workflow devo analizzare solo i componenti che servono per realizzarla.
##### Sintassi
La sintassi è la seguente:
- Blocco $E_i$: è il sottosistema/componenti $i$-esimo.
- $e_i$: evento del blocco $i$-esimo senza guasti in $[0,t]$.
- $\bar{e_i}$: evento del blocco $i$-esimo guasto in $[0,t]$.
- $R(t)$: reliability al tempo t, i.e. probabilità che il sistema sia funzionante a tempo $t$. Si può definire anche con il pedice per il singolo blocco.
- $F(t)$: probabilità che il sistema sia guasto a tempo $t$ e solitamente di indica anche con $1-R(t)$ Si può definire anche con il pedice per il singolo blocco.
# Configurazioni canoniche
##### Serie
![serie configuration](RBD%20serie%20configuration.png)
Il sistema è operativo se e solo se tutti i componenti sono operativi.
La reliability al tempo $t$ è data dalla probabilità che tutti i componenti siano operativi, cioè $R(t)=P\{\cap_{i=1}^ne_i\}=P(e_1)P(e_2|e_1)\cdots P(e_n|e_1\cap\cdots\cap e_{n-1})$. Semplificando e supponendo che gli eventi siano indipendenti, cioè che il guasto a uno non implichi nulla sul guasto degli altri, abbiamo $R(t)=\prod_{i=1}^np(e_i)$. In ogni caso è chiaro che aumentando i componenti si diminuisce la reliability del sistema.
- Il tasso di guasto del sistema è dato dalla somma dei tassi di guasto dei componenti, cioè $\lambda=\sum_{i=1}^n\lambda_i$.
- Se il tasso di guasto è costante allora abbiamo il [main taime to failure](attributi.md#Maintenability) è $MTTF=\tfrac{1}{\sum_i\lambda_i}$.
- Abbiamo $R(t)\le \min_i{R_i(t)}$ (moltiplicazione di numeri minori di 1), cioè l'anello debole del sistema è il componente con la reliability più bassa.
##### Parallelo
![RBD parallel configuration](RBD%20parallel%20configuration.png)
È la configurazione della [ridondanza](ridondanza.md) dove tutti i componenti sono attivi ed è il modo migliore per aumentare la reliability. Il sistema è guasto quando tutti i componenti (copie che fanno la stessa cosa) sono guaste.
L'inaffidabilità del sistema al tempo $t$ è data dalla probabilità che tutti i componenti siano operativi, cioè $F(t)=P\{\cap_{i=1}^n\bar e_i\}=P(\bar e_1)P(\bar e_2|\bar e_1)\cdots P(\bar e_n|\bar e_1\cap\cdots\cap \bar e_{n-1})$. Semplificando e supponendo che gli eventi siano indipendenti, cioè che il guasto a uno non implichi nulla sul guasto degli altri, abbiamo $F(t)=\prod_{i=1}^np(\bar e_i)=\prod_{i=1}^np(1-e_i)=\prod_{i=1}^np(1-e^{-\lambda_it})$. Da questo deriva che la reliability è data da $R(t)=1-\prod_{i=1}^np(1-e_i)$.
- Aumentare il numero dei componenti aumenta la reliability ma aumenta anche il costo di spazio e soldi. Solitamente quello che si fa è adottare una configurazione in serie e ridondare i componenti più critici.
- Abbiamo $R(t)\ge \max_i{R_i(t)}$ (1 meno una moltiplicazione di numeri minori di 1), cioè l'affidabilità è maggiore o uguale del componente più sicuro.
- Il tasso di guasto in parallelo non si può semplificare nel caso di tasso costante, e in generale abbiamo $MTTF=\int_0^\infty R(t)dt$. Se invece i componenti hanno lo stesso tasso di guasto $\lambda$ allora si può semplificare e ottenere $MMTF=\displaystyle\tfrac{1}{\lambda}\sum_{i=1}^n\tfrac{1}{n}$.
- In un sistema parallelo il tasso di guasto è in funzione del tempo anche se i tassi dei componenti sono costanti:
	- Per $t=0$ abbiamo $\lambda(t)=0$.
	- Se i componenti hanno $\lambda_i=\lambda_j=\lambda$ allora per $t\to\infty$ abbiamo che $\lambda(t)\to\lambda$.
	- Se i tassi di guasto sono diversi dobbiamo calcolare $\lambda(t)=-\tfrac{1}{R(t)}\tfrac{dR(t)}{dt}$. Due componenti ridodanti possono avere tasso di guasto diverso per la [design diversity](ridondanza.md#Design%20diversity).
##### k out of n (koon)
![RBD k on nconfiguration](RBD%20k%20on%20n%20configuration.png)
Il sistema è operativo se $k$ componenti su $n$ sono funzionanti, mentre gli altri possono essere in attesa.
La reliability al tempo $t$ è dal coefficiente binomiale $R(t)=\displaystyle\sum_{i=k}^n\binom{n}{i}p^i(1-p)^{n-i}=\sum_{i=k}^n\binom{n}{i}R_i(t)^i(1-R_i(t))^{n-i}$.
- Se $k=1$ allora abbiamo una configurazione in parallelo e se $k=n$ abbiamo una configurazione in serie.
- Se il tasso di guasto è costante e tutti i componenti sono uguali allora abbiamo il $MTTF=\displaystyle\int_0^\infty[\sum_{i=k}^n\binom{n}{i}(e^{-\lambda t})^i(1-e^{\lambda t})^{n-i}]dt$.
- Vista la sua complessità si usa solitamente con $\lambda_i=\lambda_j=\lambda$, anche se è possibili risolverla anche per $\lambda_i\ne\lambda_j$. Si utilizzano quindi elementi con lo stesso tasso di guasto.
- La configurazione più utilizzata è la 2/3 (i.e., $n=3$ e $k=2$) perché permette di realizzare la TMR (=Triple Modular Redundancy): abbiamo tre moduli in parallelo seguiti da un voter in serie. Questa architettura permette di identificare molto facilmente il fallimento di un modulo. Data una reliability dei moduli e del voter di 0.999 il sistema perde un [nines](Software%20Engineering/dependability/misure.md#Nines) vista le serializzazione del voter; inoltre la [reliability del TMD](TMR%20reliability.png) diminuisce di poco se diminuiamo l'affidabilità dei moduli ma di molto se diminuiamo quella del voter.
# Configurazioni non canoniche
Sono configurazioni che rientrano in nessuna di quelle canoniche.
##### Stand-by
Abbiamo una main unit, una standby unit e uno switch: lo swich deve decidere se la main unit è guasta e quindi passare alla standaby unit. Questa configurazione ha una [reliability maggiore](RBD%20standby%20configuration.png) rispetto a quella delle forme canoniche.
Ci sono diverse modalità di stand-by:
- [Warm stand-by](ridondanza.md#Warm%20stand-by)
	L'affidabilità del sistema è data da $R_s(t) = R_A(t) + P_{sw} \int_{0}^{t} f_A(\tau) \cdot R_B(t - \tau) \, d\tau$, dove: $R_A(t)$ è la reliability della main unit; $P_{sw}$ è la probabilità di successo dello switch; $f_A(\tau)$ è la probabilità infinitesima che la main unit si guasti nell'istante $\tau$; $R_B(t - \tau)$ è la reliability della standby unit che non ha accumulato degrado prima di $\tau$. L'integrale in pratica somma tutte le infinite possibilità in cui il guasto della main unit può acccadere tra 0 e $t$.
	- Se i tassi di guasto sono uguali e costanti allora ho $MTTF=MTTF_A+MTTF_B=\tfrac{1}{\lambda_A}+\tfrac{1}{\lambda_B}$. 
	- Se i tassi di guasto sono uguali e costanti allora ho $R(t)=e^{-\lambda t}(1+\lambda t)$.
- [Cold stand-by](ridondanza.md#Cold%20stand-by)
	L'affidabilità del sistema è data da $R_s(t) = R_A(t) + P_{sw} \int_{0}^{t} f_A(\tau) \cdot R_{B,full}(t-\tau)\cdot R_{B,warm}(\tau) \, d\tau$, dove rispetto a prima: $R_{B,warm}(\tau)$ indica che la standby unit deve essere ancora operativa quando avviene lo switch; $R_{B,full}(t-\tau)$ indica la relibaility della standby unit quando diventa pienamente operativa. È come se la standby avesse due tassi di guasto quando è in standby e quando è operativa.
##### Ponte
![RBD ponte](RBD%20ponte.png)
È una configurazione dove esiste un elemento centrale che fa si che la configurazione non sia più in serie o parallelo. Il sistema è funzionante se l'informazione può fluire lungo l'architettura stessa.
Ci sono quattro modi in cui risolvere il sistema, dove tutti portano alla stessa soluzione:
- Spazio degli eventi
	Si esegue l'analisi delle probabilità che i componenti sia funzionanti in stati diversi; solitamente si eliminano le probabilità condizionate supponente gli eventi (i.e., gli stati) siano disgiunti.
	In pratica: si fa una tabelle degli stati ($2^5=32$ stati possibili), dove uno stato corrisponde a una combinazione $\{funzionante, guasto\}$ dei componenti; si calcola la reliability del sistema come $R(t)=\sum_{i=1}^nP(e_i)$, dove $e_i$ è uno stato funzionante.
	Per definire $P(e_i)$ si moltiplicano tra loro i valori della reliability dei componenti: se nello stato il componente $j$-esimo è funzionante allora si considera $R_j$, altrimenti $(1-R_j)$ se è non funzionante. Ad esempio se $P(e_6)=A\bar{B}\bar{C}DE$, allora si ha $P(e_6)=R_A(1-R_B)(1-R_C)R_DR_E$.
- Tagli
	Un taglio è un insieme di componenti che quando falliscono portano al non funzionamento del sistema. Osserviamo che: i tagli sono funzionalmente connessi in serie; un sistema può avere più tagli; se un taglio si attiva allora il sistema non è più funzionante; l'ordine di un taglio è il numero di componenti da cui è composto.
	Nel sistema a ponte i possibili tagli sono: A e D perché sono gli unici due ingressi; C ed E perché sono le uniche due uscite; B, D ed E perché non permette l'attraversamento; A e C perché si dovrebbe entrare da D ma in questo caso i può uscire solo da C.
	L'inaffidabilità del sistema è data da $F=P[\cup_{i=1}^4T_i]$, dove $T_i$ rappresenta la probabilità che l'$i$-esimo taglio sia attivato, cioè che i suoi componenti siano guasti contemporaneamente. Se supponiamo che i componenti siano uguali (i.e., $\lambda_i=\lambda_j=\lambda$) e che i guasti siano indipendenti allora abbiamo che $F=3F^2-F^3-2F^4+F^5$, dove $F(t)=1-R(t)=1-e^{-\lambda t}$.
- Unioni
	Un'unione è un insieme minimale di componenti tali che il cui funzionamento garantisce il funzionamento del sistema. Osserviamo che: gli elementi dell'unione sono connessi in serie; le unioni sono funzionalmente connesse in parallelo; l'ordine dell'unione è il numero di componenti di cui è composta; un sistema può avere più unioni; se un'unione si attiva allora il sistema è funzionante.
	Nel sistema a ponte le possibili unioni sono: C e D; A, B e C; A ed E.
	Se supponiamo che i componenti siano uguali (i.e., $\lambda_i=\lambda_j=\lambda$) e che i guasti siano indipendenti allora abbiamo che $R=2R^2+R^3-3R^4+R^5$, con $R(t)=e^{-\lambda t}$.
- Scomposizione
	È detto metto della scomposizione perché, utilizzando la probabilità condizionata, mi permette di scomporre la configurazione a ponte in due configurazioni equivalenti; quali siano queste configurazione dipende dall'assunzione che faccio.
	In pratica tratto $B$ come un elemento di cui conosco il funzionamento e lo [rimuovo dal RBD](RBD%20scomposizione%20in%20ponte.png) e calcolo la realibility con le tecniche della serie e del parallelo insieme: la probabilità $P(S|B)$, con $S$ sistema, mi permette di ottenere una serie di due paralleli; la probabilità $P(S|\bar{B})$ mi permette di ottenere un parallelo di due serie.