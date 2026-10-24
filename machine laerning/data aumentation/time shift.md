È una tecnica di data aumentation e [regolarizzazione](regolarizzazione.md) principalmente usata per dati sequenziali e/o serie temporali, cioè dati dove l'ordine dei dati è rilevante.
# Obiettivo
Aumentare la robustezza del modello rispetto alla variazione temporale di quando l'evento occorre:
- Si vuole che il modello impari la relazione tra le forme dell'input e del target.
- Non si vuole che il modello prenda scorciatoie posizionali senza generalizzare.
# Cosa perturbare
Solitamente ha senso perturbare sia l'input che il target. Se modifichiamo solo l'input allora perdiamo la relazione temporale tra i dati e quello che il modello deve predirre.
# Varianti
In generale serve un padding: dopo aver fatto uno shift si deve riempire i vuoti lasciati.
##### Zero padding
Si mettono degli zeri per implementare lo shifting.
##### Edge padding
Il padding viene fatto con l'ultimo o il primo valore rimanente, a seconda che si anticipi o ritardi la serie.
##### Circular
I dati che escono all'inzio/fine rientrano alla fine/inizio.
A senso solo quando le serie sono periodiche.