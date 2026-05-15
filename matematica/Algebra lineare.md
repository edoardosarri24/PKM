Assumiamo che i vettori siano vettori colonna; un vettore riga è quindi un vettore trasposto.
# Indipendenza lineare
Due vettori $v_1$ e $v_2$ sono linearmente indipendenti se $\exists|\alpha=0$ tale che $\alpha v_1+\alpha v_2=0$.
- Non sono uno multiplo dell'altro. Geometricamente non giacciono sulla stessa retta.
- Possono formare una base per lo spazio che rappresentano.
# Spazio
Data una matrice $A$ abbiamo che:
- Lo spazio delle righe di $A$ è l'insieme dei vettori (in teoria vettori trasposti) che possono essere scritti come combinazione lineare delle righe di $A$.
- Lo spazio delle colonne di $A$ è l'insieme dei vettori che possono essere scritti come combinazione lineare delle colonne di $A$.
# Rank
Il rango di una matrice $A$ è il numero di righe/colonne linearmente indipendenti.
Questi vettori stabiliscono una base per lo spazio delle righe (colonne) di $A$. Vuol dire che se il rango è inferiore al numero di righe/colonne, allora ci sono alcune righe/colonne che sono linearmente dipendenti da altre; questo comporta che lo spazio che quelle righe/colonne generano minore del numero di righe.
# Base
Una base per uno spazio vettoriale è un insieme di vettori linearmente indipendenti che generano tutti i vettori appartenenti a quello spazio.
Vuol dire che tutti i vettori dello spazio possono essere rappresentati come combinazione lineare della base. Ad esempio se abbiamo $k$ vettori linearmente indipendenti allora questi formano una base per $\mathbb{R}^k$.
# Autovalori
Data una matrice $A$, un autovettore è un vettore $v$ tale che $\exists\lambda\in\mathbb{R}:Av=\lambda v$; $\lambda$ è il relativo autovalore.
Il numero degli autovalori/autovettori di una matrice quadrata è pari alla dimensionalità.
##### Proprietà
- Sia $A$ semidefinita positiva. Se $A$ è simmetrica allora tutti gli autovalori sono reali.
- Se $A$ è semidefinita positiva allora tutti gli autovalori sono $\lambda\ge0$.