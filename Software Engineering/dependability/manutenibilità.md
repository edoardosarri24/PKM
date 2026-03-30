Si tratta di un [attributo](attributi.md#Maintenability) della [dependability](dependability.md) ed è l'abilità di riparare il sistema dopo un problema, cioè di portare il sistema nuovamente nello stato di corretto funzionamento. È fondamentale perché il [tasso di guasto](tasso%20di%20guasto.md) non è mai zero e il [l'errore umano](human%20reliability%20analysis.md) è sempre possibile.
# Problemi
- Ciò che fa diminuire la manutenibilità è la complessità, l'età del sistema e la povertà di documentazione.
- Ciò che fa aumentare la maintenability è la corretta progettazione (e.g., [DFR](1-intro.md#Design%20for%20Reliability)): si deve prevedere come il sistema si danneggerà e come ripararlo; questo è vero soprattutto nel caso di componenti critici.
# Time To Recovry
Se consideriamo solo i due stati del sistema di corretto ed errato funzionamento, la misura principale è il TTR (Time To Recovry), cioè il tempo (eventualmente la sua media) passato nello stato errato. Esso è composto da altre misure:
- Diagnosi
	Si deve capire cosa si è gustato.
	Per diminuire questo tempo e i relativi costi, possiamo stabile i possibili guasti e le loro cause già in fase di progettazione.
- Set-up
	Una volta identificato il guasto si deve capire come sistemarlo; legato a questo abbiamo anche il concetto di supportability, visto come l'abilità di gestire a livello logistico (e.g., acquisto e trasporto) di tutto ciò che è necessario per la riparazione.
	Per diminuire questo tempo possiamo rendere il sistema modulabile e accessibile e aumentare la supportability.
- Riparazione
	È il TTR (Time To Repair), cioè il tempo effettivo in cui si esegue la riparazione.
- Riavvio
	Non tutti i sistemi ripartono immediatamente.
# Politiche
Ci sono due modi in cui possiamo eseguire la manutenzione.
##### A guasto
Il sistema viene riparato una volta che si verifica il guasto; non servono sistemi di diagnostica o pianificazione.
Si utilizza in contesti non safety-critical, dove non è necessaria un'alta [availability](attributi.md#Availability) e qundo i pezzi di ricambio sono economici e facilmente reperibili; in questi contesti è la meno costosa, nei complementari lo è invece troppo perché non abbiamo controllo su quando il guasto si può verificare e sull'utilizzo del personale (i.e., potrebbero non fare nulla per mesi e poi aver da fare tutto insieme).
##### Preventiva
La riparazione avviene prima del guasto. Si può eseguire in tre modi. In generale possiamo dire che più il componente è costoso e critic più ci dobbiamo muovere verso un'analisi predittiva; più invece è soggetto a usara ed economico più la manutenzione ciclica (o anche a guasto) va bene.
- Ciclica
	Viene eseguita a intervalli di tempo regolari, definiti dalle statistiche di guasto.
	È utilizzata quando: i componenti da riparare hanno dei guasti ricorrenti, come quelli soggetti a usura (e.g., meccanici); il costo della riparazione di un componenti al momento funzionante è più basso rispetto alla riparazione al guasto.
	Il problema principale è che da sola non basta quasi mai: se il guasto occorre a metà del periodo allora si deve passare alla modalità a guasto; se questo succede sempre allora si ha una politica a guasto pura. Per questo motivo si deve in qualche modo settare i periodi in modo da intervenire il più vicino possibile al futuro guasto.
	Ha il vantaggio che, diminuendo i guasti, si ha un'[availability](attributi.md#Availability) maggiore
- Condition based
	Si deve monitorare (in modo automatico o con un operatore fisico) a intervalli di tempo un parametro che rappresenta lo stato di degradazione del componente e definire una soglia sopra la quale si deve intervenire; se un componente è complesso possiamo utilizzare due livelli di soglia, in modo da poter iniziare a prepararci prima dell'effettivo guasto.
	Ha il vantaggio di riparare il componente quando siamo molto vicini al guasto (risolve il problema della ciclica); lo svantaggio sta nella difficoltà della sceltà della soglia.
- Preventiva
	L'idea è simile alla condition-based: si stabilisce un parametro di interesse; la scelta di quando eseguire l'intervento non è data dal monitoraggio, ma dai risultati di modelli matematici che stimano quando potrebbe avvenire il guasto.
	Il modello deve prevedere il momento in cui inizia ad avvenire il degrado, cioè quando ha inizio l'ultima parte di vita utile del componente.
# Reliability Centred Maintainance
La Reliability Centred Maintainance (RCM), definita dalla norma IEC 60300, serve a identifica la politica da utilizzare dati degli obiettivi di [safety](Software%20Engineering/dependability/safety.md), [availability](attributi.md#Availability) ed economici.
È una tecnica che deve essere utilizzata già in fase di progettazione