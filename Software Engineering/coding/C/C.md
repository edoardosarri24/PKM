Il primo approccio al C è quello di K&R C con il libro [Linguaggio C](Linguaggio%20C.pdf). Il primo C standardizzato è del 1989 ed è chiamato C89 o Ansi C. Gli aggiornamenti successivi hanno sempre garantito la retro compatibilità e sono:
- C99: ha portato l'utilizzo vero e proprio semplificando di molto la sintassi.
- C11: si è concentrato sul multi threading e sulla sicurezza.
- C17: non ha introdotto novità ma ha corretto dei bug.
- C23: è la versione moderna che ha pulito di molto il linguaggio e avvicino i C al C++.
# Costanti
- Definite da $\#define\ NOME\ valore$.
- Tipi
	- Senza nulla è un intero. Uno 0 davanti al numero vuol dire che è rappresentato in forma ottale; uno 0x in forma esadecimale
	- Con $l$ o $L$ è un long.
	- u o U è unsigned.
	- ul o UL sono unsigned long.
	- In virgola mobile
# Main
Il $main$ ha 2 argomenti: $main(int\ argc,char\ *argv[])$.
- $argc$ indica il numero di argomenti da passare al programma. È sempre maggiore di 1 perché il primo argomento è il nome con cui si è invocato il programma.
- $char *argv[]$ è un vettore di stringhe
- Si può usare questi due argomenti per implementare funzioni che gestiscano parametri opzionali (ved pag20 [Linguaggio C](Linguaggio%20C.pdf)).