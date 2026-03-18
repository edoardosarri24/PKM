Le [variabili](coding/C/variabili.md) del [C](C.md) hanno pochi tipi base con cui costruire strutture complesse. Il tipo determina i valori che la variabile può assumere.
# interi
Memorizza sempre un valore usato poi per i calcoli. La dimensione è quella con cui la macchina rappresenta gli interi.
Sono la cosa migliora da usare per prestazioni e cache se non ci serve garanzia sulle dimensioni in memoria; in questi casi però è meglio usare la libreria $stdint.h$ che da garanzie sui segni e sulla dimensione del dato.
##### Qualificatori
- Si può usare $int$ preceduto dai qualificatori $short$ e $long$. In questo caso non abbiamo garanzie su quanti bit usano, ma possiamo essere sicuri che $16bit\le short\le int\le long\ge32bit$.
- Si può usare anche $signed$ e $unsigned$. In questo caso cambia solo il range in cui si rappresentano i numeri.
##### Indici
Per indicizzare un vettore si deve utilizzare $size\_t$ di $stdio.h$.
- Questo definisce un tipo $unsigned$ che garantisce di poter memorizzare il più grande oggettoc che sta in memoria.
- Le funzioni di [allocazione](allocazione.md) prendo il tipo $size\_t$. 
# char
Memorizza sempre un byte in ogni architettura.
Senza specificatori è sempre in grado di contenere un carattere ASCII stardard, cioè i valori in $[0,127]$. Per essere sicuro è buona norma castarlo a $\texttt{unsigned char}$.
Se non volgiamo memorizzare caratteri ma usare $char$ per risparmiare memoria allora si può usare $unsigned$ o $signed$; quello che cambia è solo il range di valori rappresentati. In questi casi però è forse meglio usare la libreria $stdint.h$ che da garanzie sui segni e sulla dimensione del dato.
##### Stringhe
Le stringhe in [C](C.md) sono [vettori di char](vettori.md#Stringhe) che terminano con il carattere speciale $\0$.
Dal punto di vista della memoria richiede un elemento in più per memorizzare il carattere di fine stringa.
# float-double
Il tipo $float$ si usa in contesti di memoria limitata perché contiene 4byte.
Solitamente, anche perché di solito è più veloce, si usa $double$. Se vogliamo massima precisione in floating-point allora si usa $long\ double$. 
# Enumerazioni
Una enumerazione è una lista di valori [constanti](coding/C/variabili.md#Constanti) che inizia con 0 e cresce.
- Possiamo specificare il primo valore esplicitamente e far si che sia diverso da 0; gli altri partiranno da quello definito e cresceranno in modo incrementale.
- Nella stessa enumerazione possono esserci nomi con valori uguali.
- In enumerazioni diverse i nomi devono essere diversi.
# Conversioni
Le conversioni automatiche a manuali devono seguire la dimensione dei dati per non perdere informazione: si può castare un tipo a un tipo più grande, ma non il viceversa.
Le difficoltà maggiori, su cui è sempre meglio controllare la documentazione del compilatore sono:
- $char\to int$: i caratteri $char$ sono di deafult senza segno. Se ci serve convertirli in un $int$ allora è sempre meglio definire anche il segno del $char$ per essere sicuri della corretta conversione (i.e., con o senza segno); altrimenti è undefine behaviour.
- Per variabili che sono $unsigned$.