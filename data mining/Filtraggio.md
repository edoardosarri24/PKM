Il problema è il seguente: abbiamo uno stream di dati e non possiamo o non vogliano memorizzarli tutti, ma dobbiamo eseguire un filtraggio con l'idea dati non memorizzati vengano persi.
# Hash-based filter
Abbiamo un insieme $S$ di keyword (e.g. indirizzi mail) e uno stream $F$ di coppie $(s,t)$ (e.g. indirizzo mail e testo). Vogliamo filtrare e memorizzare $t$ solo se $s\in S$.
##### Soluzione
Se $S$ sta in memoria allora ogni volta che arriva una coppia scorriamo la lista e vedimao se $s\in S$.
Il problema sta quando $S$ non [entra in memoria](File%20system%20distributi.md). La soluzione è creare una tabella hash $B$ di dimensionalità $n$ indicizzata da una funzione hash $h$. Per ogni $s\in S$ facciamo $B[h(s)]=1$.
Quando arriva lo stream, salviamo ogni suo elemento $a$ se e solo se $B[h(a)]==1$.
##### Falsi positivi
A causa delle collisioni della funzione hash non possiamo avere falsi negativi ma solo falsi positivi. Ad esempio può passare una mail che non proviene da un indirizzo importante (se collide con un indirizzo importante), ma se una mail è importante allora questa passa il controllo.
Vediamo come approssimare la probabilità che ci sia un falso positivo. Se $|B|<<|S|$ allora ogni allora non ci saranno collisioni tra keyword è quindi ognuna di esse ha a disposizione 8 posizioni per essere indicizzata. Se $|B|<<|S|$ allora la probabilità che ci sia un falso negativo è circa $\tfrac{|S|}{|B|}$.
# Bloom filter
Invece di usare una sola funzione hash se ne usano $k$.
Durante l'inizializzazione per ogni $s\in S$ e per ogni $i\in\{1,\cdots k\}$, si fa $B[h_i(s)]=1$.
A run time quando arriva un elemento $a$ dallo stream lo si salva se $B[h_i(a)]==1$ per ogni $i$.
##### Collisioni
Il problema è che se $k$ è troppo alto il nostro filtro è troppo debole perché tutti i valori di $B$ saranno a 1. Si può [osservare](bloom%20filter.png) che per il giusto valore $k$ la probabilità di avere un falso positivo è più bassa rispetto a usare $k=1$.
Il numero $k$ di funzioni hash ottimo è dato da $\tfrac{B}{S}log2$.