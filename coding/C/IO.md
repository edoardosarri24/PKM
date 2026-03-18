In [C](C.md) l'input e l'output vengono sempre considerati come sequenze di caratteri che definiscono linee; ogni linea è separata da un carattere a capo $"\n"$ e la fine della sequenza è indicata da $EOF$ (i.e., che ha valore -1).
L'header per l'io della [standard library](standard%20library.md) è $stdio.h$.
# File standard
Quando un programma va in esecuzione il sistema operativo apre tre file: 
- Standard input
	La stastira
- Standard putput
	Il monitor
- Standard error
# File
Per scrivere e lggere sui file si deve:
- Aprire
	Si usa fopen, che restituisce un file pointer alla struttura $FILE$ che contiene tutte le sue informazioni.
- [Interazione](#Interazione)
- Chiudere
	Si usa $fclose$ che prende il puntatore alla struttura $FILE$.
##### Interazione
- Leggere un carattere
	Si usa $fgetc$. Restituisce un $int$ e non un $char$ per poter lavorare con codifiche diverse dalla normale standard ASCII. In questo caso una chiamata di $fgetc$ non restituisce il carattere ma solo un suo byte; se vogliamo lavorare a livello di carattere dobbiamo usare funzioni e tipi wide come $wint\_t$ e $fgetwc$.
- Leggere una linea
	Si susa $fgets$. Restituisce il puntatore a uuna stringa; il carattere $"\n"$ della fine linea del file viene sostituito con $"\0"$.
- Leggere qualcosa di cui si sa il formato
	Si usa $fscanf$.