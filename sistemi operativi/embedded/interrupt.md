Un interrupt è un meccanismo di comunicazione a basso livello, solitamente event driven e asincrono ed è quindi l'opposto del polling.
Solitamente si usa quando ci sono più periferiche da gestire e fare polling è troppo costoso e si perde troppo tempo.
##### Tassonomia
- Hardware
	È un meccanismo asincrono. Si dividono in:
	- Maskable (IRQ)
		Sono disattivabili dal software tramite registri di controllo. Disattiverli serve a proteggere sezioni critiche.
	- Non-Maskable (NMI): Non sono disattivabili tramite software. Sono riservati a guasti critici (e.g., caduta di tensione e quindi transistor instabili tra 0 e 1, errori di parità della RAM).
- Software
	Sono generati da un'istruzione della CPU e solitamente sono l'esecuzione delle system call chiamate dallo spazio utente.
- Exception
	Sono generati dall'hardware a causa di errori di a runtime (e.g., divisioni per 0, page fault o accesso a memoria non valido).
# Creazione di un ISR
L'implementazione di un interrupt è detta Interrupt Service Routine (ISR).
##### Ciclo di vita
Quando scatta l'evento la CPU esegue a livello HW (i.e., non c'è righe di codice che implementano l'operazione, ma sono eseguite a livello di transistor) operazioni simili al context switch del kernel di un sistema operativo:
- La periferica genera un segnale HW passando al processore il suo ID.
- Salvataggio dello stato
	La CPU salva nello stack i registri di lavoro correnti e il Program Counter (i.e., l'indirizzo della prossima istruzione).
- Lookup della routine
	Legge dall'Interrupt Vector Table (IVT) all'indice ID passato dalla periferica e preleva l'indirizzo della funzione associata a quell'evento.
	La IVT è una mappa chiave-valore memorizzata a un indirizzo fisico, dove il valore è un puntatore alla ISR.
- Esecuzione dell'IRS
	La IRS è l'Interrupt Service Routine.
- Ripristino dello stato
	La CPU ripristina i registri dallo stack e rimette il PC sul codice interrotto.
##### Interrupt map
Per inizializzare la IVT dobbiamo mettere il corretto puntatore della ISR al corretto indice della IVT. Questo indice deve essere lo stesso ID della periferica che l'interrupt gestisce.
Il mapping tra ID e periferica è fatto dal produttore della scheda all'interno del manuale che collega infatti in modo fisico la periferica a un pin della scheda.
# Osservazioni
- Lo stack degli interrupt è lo stesso di quello del thread che lo ha generato.
- Il tempo di esecuzione degli interrupt, oltre ad essere breve, deve essere deterministico. Non si devono chiamare quindi operazioni senza upper bound dell'execution time, come $\texttt{malloc}$.
- Se il codice sta eseguendo una sezione critica (e.g., lettura di una variabile a 32 bit su un'architettura a 8/16 bit) allora possiamo momentaneamente disattivare con $\texttt{\_\_disable\_irq()}$ e riattivare con $\texttt{\_\_enable\_irq()}$.
- Il jitter tra la chiamata e l'esecuzione di un'interrupt non è deterministica e dipende da molti fattori.