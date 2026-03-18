Un sanitizer è un dynamic code analyzer integrato nel compilatore che permette di scoprire errori a runtime nel codice [C](C.md) e C++.
In pratica il compilatore instrumenta il codice in modo da memorizzare per ogni variabile dei metadati (e.g., se la variabile è inizializzata, è all'interno di una ciritical section) permettendo di rilevare errori durante l'esecuzione che il compilatore non può vedere in quanto lavora in modo statico.
Si usa in fase di debugging e non nella versione di release.
# Tipi
Ci sono diversi tipi di sanitizer. i più famosi e usati sono:
- AddressSanitizer (ASan)
	Rileva errori di corruzione della memoria come buffer overflow (su stack, heap o variabili globali), use-after-free o memory leak.
- ThreadSanitizer (TSan)
	Rileva solitamente [race condition](race%20condition.md) e deadlock.
	Per eseguirlo si deve disabilitare (non a livello di sistema sennò si compromette la sicurezza) il ASLR (Address Space Layout Randomization), altrimenti il TSan fallisce.
- MemorySanitizer (MSan)
	Rileva l'uso di variabili non inizializzate.
- UndefinedBehaviorSanitizer (UBSan)
	Rileva implementazioni che non seguono lo standard, cioè undefined behaviours.
- LeakSanitizer (LSan)
	Individua memory leak, cioè memoria non libera che rimane occupata ma non usata. È incluso in ASan.
# Limiti
- Insturmentando il codice si aggiungono controlli e complessità. Questo postra a rallentare le performance anche di 2x o 3x.
- Alcuni sanitizer sono incompatibili tra loro.
# Utilizzo
Il sanitizer è usato in fase di progettazione e non durante lo sviluppo: quando si sta sviluppando il programma e lo eseguiamo allora la compilazione avverrà con il sanitizer attivo; quando rilasciamo il programma allora la compilazione avverà senza il sanitizer attivo.
##### Comandi
Per compilare con il sanitizer si esegue $\texttt{gcc -fsanitize=[tool] -g main.c -o main}$ e poi si esegue il programma con $\texttt{./main}$.