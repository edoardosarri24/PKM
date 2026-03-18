Un riferimento in [C++](C++.md) è un alias a una variabile: ogni operazione eseguita sul riferimento è in realtà fatto sulla variabile.
Sono meno potenti dei [puntatori](puntatori.md), ma sono più sicure e quindi da preferire a meno che non si abbia bisogno di maggiore espressività.
Solitamente sono utilizzata per eseguire un [passaggio per riferimento](coding/C++/funzioni.md#Passaggio%20per%20riferimento) nelle funzioni.
##### Sintassi
La sintassi è $\texttt{<tipo>\& <nome> = ...}$.
##### Compilazione
I riferimenti a basso livello sono implementati con i puntatori.
##### Caratteristiche
- Una variabile è strettamente accoppiata alla sua variabile: non può cambiare variabile a cui riferisce, come invece succede con i puntatori.
- Non esiste un'aritmetica dei riferimenti.
- Non possono avere valori nulli. Questo fa si che non debba essere controllata prima di usarla come si fa con i puntatori.