Gli undefined behaviours in [C](C.md) sono comportamenti che ogni compilatore può  implementare in modo diverso e che quindi non sono definiti dallo [standard C](C.md).
# Casi
Sono undefined behaviours:
- Più chiamate di funzioni con operatori nel mezzo.
- Gli assegnamenti nidificati.
- Valore di ritorno non specificato.
- Modifica al valore di una costante.
- Inizializzazione memoria con [malloc](allocazione.md).
- Non definire il tipo di ritorno di una funzione.