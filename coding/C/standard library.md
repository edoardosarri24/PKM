Lo standard C definisce i componenti di base del [C](C.md) a partire dai quali si possono costruire i programmi. Si fa riferimento a questi spesso con il nome $\texttt{libc}$.
Si tratta di un insieme di file $\texttt{.h}$ che nel tempo sono cresciuti con le versioni standard del C.
# Basi
- $\texttt{stdio.h}$
	Funzioni di [I/O](IO.md) e gestione dei file.
- $\texttt{stdlib.h}$
	Funzioni di utilità generale come [allocazione](allocazione.md) della memoria, conversione numerica, controllo su processi e generazioni numeri casuali.
# Manipolazione dati
- $\texttt{string.h}$
	Funzioni per manipolare stringhe.
- $\texttt{ctype.h}$
	Funzioni per il controllo e la conversione di caratteri.
- $\texttt{math.h}$
	Funzioni matematiche che utilizzano una precisione $double$.
# Tipi
- $\texttt{limits.h}$
	Definisci i limiti massimi dei tipi.
- $\texttt{stdint.h}$
	Definisce i tipi di interi. Sono comodi quelli che garantiscono un minimo di bit in ogni architettura o quelli che garantiscono di usare un numero di bit minimo ma con la velocità massima.
- $\texttt{stddef.h}$
	Definizioni di tipi comuni.