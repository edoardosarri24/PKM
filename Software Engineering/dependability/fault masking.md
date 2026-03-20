La [fault tollerance](fault%20tollerance.md) ci permette di far continuare l'esecuzione del sistema anche se sono presenti dei fault. Questo obiettivo spesso è raggiunto utilizzando la [ridondanza](ridondanza.md) in modo da isolare il componente che ha causato tale guasto.
La fault masking è una forma di ridondanza statica: si vuole costruire un circuito di componenti che isoli in modo automatico quelli che non sono più correttamente funzionanti.
Queste tecniche solitamente permettono a un sistema di essere tolleranti a un guasto; dal momento che questo occorre il sistema non può permettersi più un nuovo guasto.
# N-modular ridundancy with voter
È una tecnica in cui abbiamo un componente ridondato $N$ volte prima di un voter che fa passare l'ouput più rappresentato.
##### Casi particolari
- L'architettura più utilizzata è quella della Triple Modular Redundancy (TMR, i.e. 2-out-of-3), dove ci può essere al più un componente fallito.
- Un'estensione è quella di avere $N$ copie dello stesso componente, dove solitamente $N$ è dispari per evitare situazioni di incertezza.
##### Costo
Ci sono più aspetti negativi:
- Soldi
	Dobbiamo comprare $N$ componenti più il voter.
- Tempo
	Serve una sincronizzazione dei componenti ridondati e la velocità del voter è quella del componente più lento. In più dobbiamo. Aggiungere il tempo che il voter ci mette a eseguire.
##### Decomposizione
Possiamo ridondare a due livelli:
- Sistema
	In questo caso gli input del voter saranno gli ouput di tre sistemi ridondati. In questo modo il fallimento di due soli componenti all'interno di sistemi diversi provoca un fallimento dell'intero sistema globale.
- Componente
	Possiamo decomporre il sistema in più componenti e applicare la ridondanza a ognuno di questi componenti; avremmo alla fine tanti voter quanti sono i componenti.
- Voter
	Possiamo ridondardare il voter: l'ouput di ogni componente ridondato finisce un tanti voter quanti sono tali componenti ridondati; ogni voter prende poi un flusso isolato; alla fine l'output del sistema è deciso però da un singolo voter.
##### Reliability
Il collo di bottiglia generale di questa architettura è il voter: se questo componente ha un'alta [reliability](attributi.md#Reliability) allora la reliability del sistema sarà buona, altrimenti avremo un sistema poco affidabile.
Se $R_{V}$ è la reliability del voter allora la reliability massima del sistema sarà data da $R=\frac{1}{(3-2R_{V})R^\alpha}$, dove $\alpha=\frac{2R}{3-2R}$ è la reliability del singolo componente.
##### Voter behaviour
In un'architettura 2oo3 (i.e., 2 out of 3) dove i segnali sono digitali (i.e., gli input e gli ouput sono bit) il voter confronta bit a bit. Se arrivano i bit $a$, $b$ e $c$ allora l'ouput del voter è dato da $out=(ab)\cdot(ac)\cdot(bc)$.