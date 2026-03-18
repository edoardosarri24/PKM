È uno stile di computazione usato nei [large scale file system](File%20system%20distributi.md).
L'idea è che il programmatore si occupi di scrivere solo due funzioni:
- map
	Dato il proprio chunk un nodo estrae ciò che è di interesse e lo organizza in una coppia $(key,value)$.
	Ad esempio se vogliamo contare l'occorrenza di ogni parola $x$ del web, ogni server deve produrre la coppa $(parola, occorrenza)$. Se vogliamo invece calcolare la media delle valutazioni di un film su un dataset distribuito in più nodi, ogni server deve produrre la coppia $(film,media)$.
- reduce
	Dato un insieme di coppie $(key,value)$ aggrega, ordina, filtra, solitamente secondo la chiave.
	Nell'esempio sopra mette insieme tutte le occorrenze delle parole trovate su vari server.
# Word count algorithm
Arriviamo in modo progressivo al word count algorithm, cioè il conteggio delle parole del web che abbiamo descritto sopra.
##### Tutto in memoria
Se il file delle parole sta in ram allora si crea una matrice e si aggiorna la coppia $(parola,occorrenza)$ all'interno della matrice per ogni parole letta.
##### Tabella in memoria
Se il file non sta in ram, ma in ram entra la matrice allora si può caricare via via in memoria ogni parte del file e aggiornare la matrice; il problema è che non sappiamo quante parole andremo a leggere.
##### Mar/Rudece
![word count algorithm](word%20count%20algorithm.png)
Il flusso del problema è il seguente:
- Il programmatore scrive una map e una reduce. Stabilisce poi un master che deve orchestrare il processo; questo assegna il compito di mapper ai server che contengono copie diverse dei chunk di dati; assegna poi il compito di reducer ad altri server.
- Ogni nodo che riceve l'incarico di mapper, deve produrre le coppie $(parola,occorrenza)$ relativa al proprio chunk e memorizzarle in locale. Quando finisce lo comunica al master; se fallisce il master passa lo stesso compito a un altro server dove è memorizzato lo stesso chunk di dati (come abbiamo detto i dati sono duplicati).
- Quando tutti i mapper hanno finito, il master comunica ai mapper di inviare i dati e ai reducer di ricevere le coppie $(parola,occorrenza)$ intermedie. È importante che tutte le coppie che condividono la stessa chiave finiscano nello stesso nodo; questo è possibile grazie all'operazione hash fatta sulla chiave.
- I reducer a questo punto hanno rivecuto più chiavi con il relativo valore, ma data una chiave quella è presente solo in un reducer. L'operazione reduce sta nell'aggregare i valori che condividono la stessa chiave.
- Se fallisce il master allora l'intero processo dovrà ripartire con un altro master.
# Moltiplicazione matrice-vettore
È il problema di calcolare $X=M\cdot V$, dove $x_i=\sum_jm_{ij}\cdot v_j$.
- Se la matrice e il vettore stanno entrambe in memoria si usa una libreria ed è facile.
- Se solo V sta in memoria allora uso map reduce.
	- map
		Ogni mapper ha $V$ in memoria e prende un chunk di $M$, cioè una sequenza di righe della matrice.
		Per ogni elemento $m_{ij}$, al variare degli $i$ che il chunk contiene e al variare dei $j$ che costituiscono la dimensione del vettore, produce la coppia $(i,m_{ij}\cdot v_j)$; il mapper si ritrova quindi $j$ elementi con chiave $i$.
		Invia i dati ai reducer usando la funzione hash.
	- reduce
		Tutti i valori he hanno la stessa chiave $i$, cioè appartengono alla stessa riga della matrice, sono ricevuti dallo stesso reducer grazie all'hash.
		Una volta ricevute le coppie, somma tutti i valori che condividono la stessa chiave arrivando alla coppia $(i,x_i)$.
- Se ne $M$ ne $V$ sta in memoria.
	Si divide $M$ in strisce verticali e $V$ in fasce orizzontali della stessa dimensione con l'obiettivo che stiano in memoria.
	Una volta che ogni mapper ha ricevuto un chunk di $M$ e $V$ si prosegue con il classico map reduce.
# Moltiplicazione matrice-matrice
È il problema di calcolare $C=A\times B$ ([esempio](moltiplicazione%20tra%20matrici%20map%20reduce.pdf)). Osserviamo che solitamente $A$ e $B$ saranno sparse; per memorizzarle si utilizzano quadruple $(riga, colonna, valore, matrice)$, in modo che dove il valore è 0 posso non memorizzare la quadrupla.
Si hanno due mapper/reduce in casacata:
- La prima fase usa come chiave $k$ e computa tutti i prodotti $a_{ik}\cdot b_{kj}$.
	- Il mapper raggruppa i valori secondo la chiave $k$: per la matrice $A$ la chiave è quindi la colonna, per la matrice $B$ la riga.
	- Il reducer produce tutti i possibili prodotti dove la matrice è diversa e se li memorizza in locale.
- La seconda fase somma i prodotti per ogni $i$ e $j$.
	- Il reduce della fase 1 diventa il mapper della fase 2. Questo può essere implementato in due modi: tenendo i dati sul nodo reduce1 e facendolo diventare mapper2; spostando i dati dal nodo reduce1 a un altro nodo che sarà mapper2. Il primo metodo è migliore perché il costo dell'algoritmo è numero di dati trasferiti e in questo modo questo è più basso.
	- In ogni caso il mapper2 usa come chiave la coppia $(riga, colonna)$ e raggruppa le quadruple con la stessa chiave.
	- Il reduce2 somma tutti i valori relativi alla stessa chiave, ottenendo il valore di ogni entry della matrice.