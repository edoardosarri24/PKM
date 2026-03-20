La Failure Modes and Effects Analysis (FMEA) è una tecnica per l'[analisi del rischio](analisi%20del%20rischio.md) qualitativa e induttiva.
Gli obiettivi principali sono:
- Identificare i guasti che possono avere effetti negativi su sicurezza, affidabilità e disponibilità.
- Identificare i miglioramenti che possiamo fare.
- Migliorare la conoscenza del sistema.
##### Confronto
La FMEA una tecnica più orientata all'analisi dei guasti rispetto all'analisi del rischio, cosa che invece succede aggiungendo la criticità nella [FMECA](FMECA.md).
##### Non conformità
È definita dalla norma IEC 60812 e in questo contesto non si parla tanto di guasto, ma di non conformità (NC): il guasto è un evento funzionale, cioè qualcosa che smette di funziona; una NC è un qualcosa di più generale, cioè un componente che si discosta dai suoi requisiti.
# Varianti
Ci sono diverse varianti:
- System FMEA
	Si analizza come gli effetti di un guasto si propagano nel sistema. L'obiettivo è quello di analizzare i punti critici del sistema su cui intervenire.
- Design FMEA
	Gli obiettivi sono gli stessi della system FMEA, ma si esegue in fase di progettazione.
- Process FMEA
	Individua come le scelte di progettazione possono portare a guasti del sistema. L'obiettivo è individuare le fasi critiche della progettazione, per poi eventualmente cambiarle e ottenere un prodotto migliore.
- Service FMEA
	Rileva le non conformità (NC) derivanti dalla fase di progettazione di un servizio e permette di analizzare gli effetti di queste NC quando il servizio sarà in esecuzione
# Fasi
Le fasi della FMEA si basano su:
- Si devono identificare le funzioni del sistema, cioè lo scopo per cui è stato realizzato.
- Modi si guasto
	I modi del guasto (i.e., una non conformità, cioè il modo in cui mi accorgo che il sistema ha un guasto.) vengono divise in categorie; per identificare la categoria a cui un guasto appartiene possiamo usare una tabella che la norma mette a disposizione, perché un guasto può avere molti modi di guasto.
- Cause di guasto
	Devo individuare le cause di guasto (i.e., sono le circostanze che hanno portato al guasto) per trovare il modo di risolverlo.
- Meccanismi di guasto
	Non sempre, ma a volte si devono trovare anche i meccanismi di guasto (i.e., il processo che ha portato al guasto).
- Effetti dei guasti
	Si valutano gli effetti dei guasti, sia a livello di componente che a livello di sistema: l'idea nel fare questo deve essere quella della [fault error failure chain](fault%20error%20failure%20chain.md). Possiamo avere molti effetti perché un modo di guasto può avere più cause di guasto, le quali possono portare a più effetti.
##### Risultato
Si deve quindi compilare una tabella come [questa](tabella%20effetti%20FMEA.png). Questo è il nostro output.
# Scenari
Gli scenari di utilizzo sono diversi.
##### Dove
Si utilizza la FMEA:
- Sistemi meccanici, elettrici ed elettronici.
- Processi produttivi.
##### Quando
- All'inizio del progetto.
- Nel caso di aggironamento del sistema.
- Con un bufget limitato.
- Insieme alla [FTA](analisi%20del%20rischio.md#FTA%20(Fault%20Tree%20Analysis)).
# Conseguenze
Le conseguenze sono.
##### Vantaggi
- Insieme all'analisi permette di capire meglio il sistema.
- Permette di studiare sistemi complessi tramite l'analisi dei suoi componenti.
- Non necessità di conoscere il top-event a priori come succede per [ETA (Event Tree Analysis)](analisi%20del%20rischio.md#ETA%20(Event%20Tree%20Analysis)).
##### Svantaggi
- Non è adatto all'analisi dei sistemi ridondati.
- Considera un solo guasto alla volta e non è possibile studiare combinazione di essi.