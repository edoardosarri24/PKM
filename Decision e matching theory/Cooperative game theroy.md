È una variante della [[decision]]
Vediamo i componenti:
- Abbiamo un insieme di giocatori $D=\{1,\cdots,d\}$.
- Un gioco è definito dall'assegnamento di un valore per ogni possibile coalizione $S\subseteq D$ tramite una funzione caratteristica. In pratica per ogni possibile sotto insieme dei giocatori si specifica un payoff (cioè una ricompensa).
- La funzione caratteristica è definita come $v:2^d\to\mathbb{R}$: si assegna un reale a un sotto insieme dei giocatori. Se $v$ è definita per tutto l'insieme dei giocatori $D$ allora si dice che $v(D)$ è la grand coalition; se non è definita per nussun sotto insieme allora $v(\emptyset)$ è la null coalition; se è definita per un solo $s\in S$ allora V(s) è l'arbitrary coalition.
##### Esempio
Ho 4 impiegati in una azienda, rosso, blu, verde e nero.
Se tutti gli impiegati partecipano a un lavoro allora guadagno 6 banconote; se ci lavorano solo gli impiegati rosso e blu allora guadagno 4 banconote; se invece ci lavorano solo il blu e nero allora guadagno 2 banconote; se ci lavorano il rosso e verde allora guadagno 5 bancone; in questo modo posso fare tutti i sotto insiemi e associare il guadagno.
La domanda del problema è: come ripartisco gli utili dell'azienda agli impiegati rispetto al contributo al profitto. Questa ripartizione è fatta tramite lo shapley value.
# Shapley value
Sia $G$ l'insieme di giochi con $d$ giocari.
Uno shapley value è un modo per assegnare un vettore di profitti a ogni gioco in modo equo, cioè un vettore in $\mathbb{R}^d$, ovvero un profitto per ogni giocatore in ogni gioco. È quindi una funzione $\phi:G\to\mathbb{R}^d$, dove dato un gioco si assegna un vettore di $d$ elementi; per un gioco $v$ lo shapley value definisce il vettore $\phi(v)=\phi_1(v),\cdots,\phi_d(v)$.
##### Assiomi di equità
Ci sono quattro assiomi di equità. Queste sono proprietà che definiscono quando un'assegnamento di profitti è equo.
- Efficienza
	I crediti si devono sommare al valore della grande coalizione. Vuol dire che $\displaystyle\sum_{i\in D}\phi_i(v)=v(D)-v(\emptyset)$, in teoria $v(\emptyset)=0$. In pratica quello che producono tutti insieme deve essere distribuito tra tutti.
- Simmetria
	Se due giocatori $i,j$ sono interscambiabili, cioè $v(S\cup \{i\})=v(S\cup\{j\}),\forall S\subseteq D$ allora $\phi_i(v)=\phi_j(v)$. Cioè se far entrare $i$ o $j$ in una coalizione $S$ produce lo stesso payoff, allora il premio che do a $i$ o $j$ deve essere lo stresso.
- Giocatore nullo
	Se un giocatore non da contributo, cioè se $v(S\cup\{i\})=v(S),\forall S\subseteq D$ allora $\phi_i(v)=0$, cioè il suo premio deve essere nullo.
- Linearità
	I crediti da attribuire per combinazione lineare dei giochi si devono comportare in modo lineare, cioè $\phi(c_1v_1+c_2v_2)=c_1\phi(v_1)+c_2\phi(v_2)$, con $c_1,c_2\in\mathbb{R}$.
	Vuol dire ad esempio che se si decide di raddoppiare i premi allora tutti devono ottenere il doppio.
##### Funzione
Si può dimostrare che Shapley value $\phi:G\to\mathbb{R}^d$ è l'unica funzione che soddisfa queste proprietà.
Essa è definita come $\phi_j=\displaystyle\tfrac{1}{d}\sum_{S\subseteq D\setminus j}\tfrac{1}{\binom{d-1}{|S|}}(v(S\cup\{j\})-v(S))$, dove:
- La sommatoria fa una media sulle coalizioni.
- Il fattore $\tfrac{1}{d}$ serve per fare una normalizzazione.
- L'inverso della combinatoria ci dice quante possibili scelte ci sono della coalizione $S$ composta da $d-1$ giocatori (perché abbiamo levato $j$).
- La differenza delle funzioni $v$ ci dice la differnza tra avere e non avere $j$ in quella coalizione.
- In pratica per ogni coalizione $S$ che non comprende il giocatore $j$ si vuole guardare come cambia la produzione di quella coalizione con e senza $j$.
- C'è una forma più classica e utile per i calcoli (da non sapere) della Shapley value $\phi_j=\displaystyle\tfrac{1}{d}\sum_{S\subseteq D\setminus j}\tfrac{|S|!(d-1-|S|)!}{(d-1)!}(v(S\cup\{j\})-v(S))=$$=\displaystyle\sum_{S\subseteq D\setminus j}\tfrac{|S|!(d-1-|S|)!}{d!}(v(S\cup\{j\})-v(S))$.
# Esempio
Prendiamo $d=3$, cioè $D=\{1,2,3\}$. Vediamo come calcolare lo shapley valuer per il giocatore 2. Le coalizioni senza il giocatore 2 sono:
- $S=\{\emptyset\}$
	Calcoliamo  $\tfrac{|S|!(d-1-|S|)!}{d!}(v(S\cup\{2\})-v(S))=\tfrac{0!\cdot2!}{3!}(v(\{2\})-v(\{\emptyset\}))=\tfrac{1}{3}(v(\{2\})-v(\{\emptyset\}))$.
- S=\{1\}
	Calcoliamo  $\tfrac{|S|!(d-1-|S|)!}{d!}(v(S\cup\{2\})-v(S))=\tfrac{1!\cdot1!}{3!}(v(\{1,2\})-v(\{1\}))=\tfrac{1}{6}(v(\{1,2\})-v(\{1\}))$.
- S=\{3\}
	Calcoliamo  $\tfrac{|S|!(d-1-|S|)!}{d!}(v(S\cup\{2\})-v(S))=\tfrac{1!\cdot1!}{3!}(v(\{3,2\})-v(\{3\}))=\tfrac{1}{6}(v(\{3,2\})-v(\{3\}))$.
- S=\{1,3\}
	Calcoliamo  $\tfrac{|S|!(d-1-|S|)!}{d!}(v(S\cup\{2\})-v(S))=\tfrac{2!\cdot0!}{3!}(v(\{1,2,3\})-v(\{1,3\}))=\tfrac{1}{3}(v(\{1,2,3\})-v(\{1,3\}))$.