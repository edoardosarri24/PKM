In [C](C.md) un vettore di un certo tipo non altro che una sequenza consecutiva di celle in memoria di tale tipo.
# Puntatori similitudine
Supponiamo di vere un vettore dichiarato come $\texttt{int a[10]}$, cioè 10 interi consecutivi. Supponiamo di avere un vettore $\texttt{int *p}$ e di fare $\texttt{p=\&a[0]}$.
Il nome di un vettore ($\texttt{a}$ in questo caso) non rappresenta altro che l'indirizzo in memoria del primo elemento del vettore: dopo l'assegnazione usare $\texttt{p}$ e $\texttt{a}$ è la stessa identica cosa:
- Per ottenere il valore dell'elemento $i$-esmo possiamo sia fare $\texttt{*(p+i)}$, $\texttt{a[i+1]}$ oppure $\texttt{*(a+i)}$. 
- Possiamo scrivere $\texttt{int *p=a}$ anche al posto di  $\texttt{int *p=\&a[0]}$.
##### Motivazione
La motivazione è internamente il C tratta i nomi di vettori come puntatori. Questo permette una maggiore velocità di compilazione nel caso si usi il metodo con i puntatori.
# Stringhe
Una stringa non è altro che una sequenza consecutiva di [caratteri](tipi.md#char) che termina con il carattere speciale di fine stringa $\0$, cioè un vettore di caratteri.
In questo modo abbiamo che una stringa viene acceduta tramite l'indirizzo del suo primo elemento; possiamo dichiarare una stringa quindi sia come un vettore che usando i [puntatori](puntatori.md).
# Multidimensionali
Le matrici in C sono utili se abbiamo una dimensione dei dati fissa; se invece ci serve un'allocazione dinamica solitamente si usano i [vettori di puntatori](puntatori.md#Vettori%20di%20puntatori).
##### Memoria
Una matice è in realtà implementata come un singolo vettore costruito concatenando le righe una dopo l'altra: se abbiamo una matice $n\times m$ questa verrà memorizzata come un vettore di dimensione $d=n\cdot m$.
Questo implica che nello scorrere la matrice l'indice che varia più velocemente è quello relativo alla colonna.