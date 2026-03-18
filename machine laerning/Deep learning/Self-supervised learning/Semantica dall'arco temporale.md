È una tecnica usata nel [Self supervised learning](Self%20supervised%20learning.md) per far apprendere alla rete i dettagli semantici temporali usati poi per scopi generici.
L'idea è quella di usare l'ordine dei frame di un video qualunque (e quindi abbiamo quanti samples vogliamo) per imparare l'ordine semantico delle azioni (es: la mela cade dal basso verso l'alto).
# Studio
Nello studio poi hanno preso un video e hanno estratto 4 frame. L'obiettivo della rete è stato quello di ordinare i frame; il problema è quindi diventato di classificazione in $n!/2$ classi (tutte le possibili combinazioni dei 4 frame).
# Cheating
Per evitare che la rete risolvesse il compito senza apprendere davvero i dettagli semantici si è fatto il cropping dei frame, cioè si sono prese delle sotto immagini.