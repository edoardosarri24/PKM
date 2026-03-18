È una classe della [STL](STL.md) che permette di lavorare con il tempo.
# Clock
Definisce tre tipi di clock:
- $\texttt{system\_clock}$
	Rappresenta il wall clock del sistema ma può essere affetto da errori douto a compiti del sistema operativo.
- $\texttt{steady\_clock}$
	È un clock monotono crescente che non è affetto da cambiamenti del tempo del sistema operativo.
- $\texttt{high\_resolution\_clock}$
	È garantito essere il wall clock con la precisione maggiore che il sistema host riesce a garantire.
# Duration
Rappresenta un intervallo di tempo e non un punto nel tempo.
- Permette di rappresentare il tempo con diverse unità temporali: secondi, millisecondi e nano secondi.
- Con due oggetti di tipo duration si possono fare tutte le operazioni che vogliamo in modo sicuro.
- Si può ottenere il valore con $\texttt{count()}$.