È una tecnica di data aumentation e [regolarizzazione](regolarizzazione.md) principalmente usata per dati sequenziali e/o serie temporali, cioè dati dove l'ordine dei dati è rilevante.
# Obiettivo
Aumentare la robustezza del modello rispetto alla velocità con cui si esegue l'azione: si vuole che il modello impari la relazione tra le forme dell'input e dell'ouput anche se la stessa azione viene eseguita a velocità diverse.
# Cosa perturbare
Solitamente ha senso perturbare sia l'input che il target. Se modifichiamo solo l'input allora perdiamo la relazione temporale tra i dati e quello che il modello deve predirre.
# Varianti
Nel caso in cui il tempo venga dilatato è come eseguire l'azione a rallentatore; nel caso in cui il tempo venga contratto è come aumentare la velocità.
Per avere sempre la stessa lunghezza temporale dobbiamo croppare o fare un padding.
##### Zero padding
Nel caso della dilatazione temporale può succedere che l'evento esca dai bordi della finestra temporale considerata e quindi che si perda informazione.
Nel caso di contrazione temporale mettere tutti zero può essere un po' troppo drastico; si potrebbe invece mettere l'ultimo valore disponibile o medie di qualche tipo.
##### Warping Non Lineare
Invece che fare warping omogeneo lungo tutto il tempo, potremmo deformare il tempo con scale diverse.