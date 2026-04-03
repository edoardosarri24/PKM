 Uno spazio di probabilità è definito dalla tripla $(\Omega, F, P)$, dove:
- Lo spazio dei possibili output $\Omega$ 
	È l'insieme di tutti i possibili outcome (detti anche esiti o campioni) dell'esperimento.
	Ad esempio nel lancio di una moneta $\Omega=\{Testa,Croce\}$.
- Lo spazio degli eventi $F$ ($\sigma$-algebra)
	$\Omega$ è l'insieme di tutti i possibili outcome, ma nella realtà a non tutti gli esiti è possibile assegnare una probabilità (e.g., nel caso continuo). Per questo si definisce $F$ come un insieme di sottoinsiemi di $\Omega$: dato l'evento $e\in F$ si ha $e\subseteq\Omega$; l'evento $e$ contiene tutti gli outcome che soddisfano certe proprietà e a cui possiamo assegnare una certa probabilità.
	- Discreto
		Se $\Omega$ è discreto allora l'insieme delle combinazioni degli outcome è finito e possiamo assegnare una probabilità a ognuno di essi. In questo caso solitamente $F$ è l'insieme delle parti di $\Omega$.
	- Continuo
		Se $\Omega$ è continuo (e.g., se gli outcome sono un valore temporale, come la probabilità che $x$ sia successo a tempo $t$) allora lo spazio degli outcome è infinito e non è possibile assegnare una probabilità a ogni esito. In questo caso F è uno dei possibili insimi di sottoinsiemi di $\Omega$.
- $P:F\to[0,1]$
	È una funzione che assegna a ogni evento una probabilità. Gode di certe proprietà:
	- P($\Omega$)=1, ovvero uno degli eventi possibili accade sempre.
	- Se $e_1\cap e_2=\emptyset\Rightarrow P(e_1\cup e_2)=P(e_1)+P(e_2)$.