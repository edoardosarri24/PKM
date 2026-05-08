Anche se vorremmo che un sistema funzionasse perfettamente senza guasti, ogni volta che se ne verifica uno si hanno dei rischi: la [gestione del rischio](gestione%20del%20rischio.md) studia come individuare i guasti e i loro potenziali effetti e ha l'obiettivo di ottenere sistemi [dependable](dependability.md).
In generale si tratta di una disciplina molto soggettiva, per cui si lavora spesso in gruppo e si cerca di utilizza standard e convenzioni univocamente riconosciute.
# Storia
L'analisi del rischio nasce dal concetto del [parmesan cheese](parmesan%20cheese.png) del 1990: io ho un pericolo, inserisco una qualunque barriera e idealmente non ho più pericolo, se ho ancora pericolo, metto più barriere. Questo con gli anni è diventato il concetto dello [swiss cheese](swiss%20cheese.png) perché la barriera può non essere corretta e più barriere possono avere lo stesso difetto.
Il tempo ha fatto si che i vecchi rischi sono stati ridotti, ma con lo sviluppo di nuove tecnologie i rischi sono in numero assoluto aumentati. Quando sviluppiamo un prodotto dobbiamo stare attenti a quante tecnologie impieghiamo (e.g., librerie esterne) e quante di queste sono nuove: avere un progetto che dipende da molte cose fa aumentare i rischi.
# Tipologie di analisi
L'analisi del rischio può essere fatta in due modi:
- Predittivo
	Si svolge all'inizio della progettazione.
- Correttivo
	Si usa per correggere i rischi già presenti.
# Definizioni
##### Danno (harm)
È una lezione fisica o logica al sistema, all'utente che lo utilizza o all'ambiente dove il sistema esegue.
Per definire l'entità del danno si usa spesso il worst case scenario.
##### Pericolo (hazard)
È la potenziale causa del danno: uno stato del sistema che se raggiunto porta al danno.
Può essere interno al sistema (e.g., problemi meccanici, termici, elettrici) o esterno (e.g., manomissioni, errori umani).
##### Rischio (risk)
È la gravità di un danno pesata per la probabilità che esso si verifichi: in pratica $rischio=frequenza \times criticità$.
Vorrei avere, e solitamente è così, che danni molto elevati abbiano una probabilità di verificarsi molto bassa.
##### Rischio accettabile
Il livello di rischio accettabile è definito come il costo che si è disposti a pagare al verificarsi di eventi indesiderati. Quando definiamo il rischio accettabile dobbiamo introdurre anche un margine di sicurezza: tutti i calcoli che facciamo sono approssimati.
Per capire se un livello di rischio è accettabile ci sono quattro tecniche:
- Analisi rischi-costi-benefici: se il costo è più alto del beneficio che ottengo allora non lo sostengo.
- Si esegue un'indagine per capire le preferenze del personale.
- Ci si basa sui livelli di rischio definiti in passato.
- Ci si basa su indici standard (e.g., IEC21010) usando norme, e per questo è il più usato. Permette di definire la zona ALARP (As Low As Reasonably Praticable), cioè un [grafico](ALARP.png) che permette di identificare zone con livelli diversi di rischio.