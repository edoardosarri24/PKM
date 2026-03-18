Il task è, data un'immagine di una folla densa, stimare il numero di persone nell'immagine.
In problemi di questo tipo è molto difficile avere annotazioni delle immagini precise; i dataset di questo tipo contano al più un migliaio di immagini annotate.
# Proxy task
Il proxy task si basa sul concetto per cui in una sotto immagine devono essere contate meno persone di quelle contate nell'immagine che la contiene.
##### Training
In questo compito la loss del proxy task e quella del downstream task sono state usate insieme. Si è fatto contemporaneamente un addestramento fully supervised e self supervised per garantire che la rete apprendesse features rilevanti.
L'obiettivo di questo era evitare che il modello facesse cheating della soluzione.