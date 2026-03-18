È un tipo di programmazione di tipo [dichiarativo](Tipi%20di%20linguaggi.md#Dichiarativo). Il linguaggio di programmazione più usato è [Prolog](https://swish.swi-prolog.org) e nella pratica trova spazio nei [ILP system](ILP%20system.md).
# Sintassi
- Variabile
	Si usano simboli in maiuscolo. Ad esempio X e Y.
- Costanti
	Si usano simboli in minuscolo. Ad esempio alice, bob.
- Predicato
	Si esprime tramite la sua arietà; si usano simboli in minuscolo. Ad esempio $father/2$. I parametri di un predicato possono essere sia costanti che variabili.
	- Esempio
		- $father(X,Y)$
		- $father(X,alice)$
		- $father(alice, bob)$
- Termine
	È una variabile, una costante o una funziona di $n$ argomenti (ciò ch restituisce xun valore, non un predicato).
	Solitamente un termine è una variabile o una costante, visto che le funzioni sono poco usate.
- Atomo
	È una formula $p(t_1,\cdots,t_n)$, dove $p$ è un predicato di arietà $n$ e $t_1,\cdots, t_n$ sono termini. In questo modo gli esempi dei predicato sono tutti atomi.
	Un atomo si dice ground se $t_1,\cdots,t_n$ sono tutte costanti. Nell'esempio dei predicati solo $father(alice,bob)$ è ground.
- Negazione
	È un $not$ seguito da un atomo.
- Letterale
	È un atomo o la sua negazione.
- Clausola o regola
	Ha una forma del tip $h_1,\cdots,h_n\vdash b_1,\cdots,b_n$, dove $h_1,\cdots,h_n$ è la testa della clausola, $b_1,\cdots,b_n$ è il body e $h_1,\cdots,h_n,b_1,\cdots,b_n$ sono letterali. Una clausola si dice ground se non ci sono variabili ma solo costanti (e funzioni).
	Siamo interessati in particolare alla clausola di Horn, la quale ha solo un letterale positivo nella testa: $h\vdash b_1,\cdots,b_n$. La notazione a cui siamo abituati è $b_1\land\cdots\land b_n\Rightarrow h$; questo si può tradurre anche come $\lnot b_1\lor\cdots\lor\lnot b_n\lor h$.
- Theory
	È un insieme di clausole.
# Semantica
- Herbrand universe
	È l'insieme di tutti i possibili termini ground che posso formare con costanti e funzioni (che in realtà non useremo).
	Solitamente sono le entità del dominio.
	- Esempio
		$\{alice, bob,\cdots\}$.
- Herbrand base
	È l'insieme di tutti i possibili atomi ground che posso formare con insiemi di predicati e l'Herbrand universe.
	- Esempio
		$\{happy(alice), happy(bob),\cdots,healty(alice),healty(bob),\cdots\}$.
- Herbrand Interpretation
	Si assegna un valore di verità a ogni elemento dell'Herbrand base.
	- Esempio
		$happy(bob)=false$ oppure $healty(alice)=true$.
# Definizioni
##### Modello
Si dice che una Herbrand interpretation è un modello per clausola $c$ se $c$ è soddisfatta per tutti i suoi grouding.
Vuol dire che qualunque costante usi nella clausola tra quelle possibili, la clausola è soddisfatta.
##### Entailtment
$T\vDash c$ è un Entailtment e vuol dire che ogni modello di $T$ è anche un modello per $c$.
$T$ è la theory, $c$ è clausola e $\vDash$ è il simbolo di Entailt.