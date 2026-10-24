È una tecnica di data aumentation e [regolarizzazione](regolarizzazione.md) principalmente usata per dati sequenziali e/o serie temporali, cioè dati dove l'ordine dei dati è rilevante.
# Obiettivo
Aumentare la robustezza del modello rispetto a una serie non completa.
# Cosa perturbare
Solitamente ha senso perturbare sia l'input che il target. Se modifichiamo solo l'input allora perdiamo la relazione temporale tra i dati e quello che il modello deve predirre.
# Varianti
Quello che facciamo è tagliare la sequenza in cimo o in fondo e sostiturla con degli zeri.