Vediamo come eseguire wildcard query nell'[information retrival](information%20retrival.md), cioè recuperare dei documenti con una query di cui non tutte le parole sono complete.
# Invertex index
Supponiamo di memorizzare le parole del dizionario tramite l'[inverted index](inverted%20index.md).
##### Query $X^*$
Si cerca nell'inverted index tutte le prole che iniziano con $X$. Visto che le chiavi (i termini) sono ordinati, questo si può fare in modo efficiente tramite una ricerca binaria.
- Esempio
	Se la query è $Hel^*$, cioè devo cercare i documenti che iniziano con $Hel$, allora si cercano le parole in $Hel\le t\le Hem$.
##### Query $^*X$
Si deve creare un altro invertex index uguale a quello normale ma dove le chiavi sono scritte in ordine inverso. Invece che memorizzare due volte le liste di posting, in questo secondo inverted index le liste di posting sono puntatore che punta a quelle del normale inverted index.
La ricerca avviene invertendo $X$ e cercando la query $X^*$ in questo nuovo indice.
- Esempio
	Se la query è $^*lo$, cioè devo cercare i documenti che terminano con $lo$, allora si cercano le parole in $ol\le t\le om$ all'interno del nuovo invertex index.
##### Query $X^*Y$
L'idea è quella di combinare i due approcci: si cercano le parole che iniziano con $X$ all'interno dell'invertex index normale; si cercano le parole che finiscono con $Y$ invertito nell'invertex index dove le chiavi sono invertite; mi vengono restituite due insieme di liste di posting; faccio l'intersezione tra questi due insieme in modo tale da eliminare le parole che iniziano con $X$ ma non finiscono con $Y$ e che finiscono con $Y$ ma non iniziano con $X$.
In questo modo il costo è $n\times m$ per fare l'intersezione, con $n$ liste del primo insieme e $m$ del secondo.
# Permuterm index
Siccome con il metodo dell'inverted index alcune query possono risultare complicate, esiste un altro approccio che si basa su un indice diverso, il permuterm index.
##### Costruzione permuterm index
Si deve modificare l'invertex index classico.
Prediamo come esempio la parola $Java$: oltre a questa parola il nuovo indice deve contenere anche $Java\$$, $ava\$J$, $va\$Ja$, $a\$Jav$ e $\$Java$; il pratica si shifta il primo carattere a sinistra sulla destra per ogni carattere, usando il carattere speciale $\$$. Il puntatore alla lista dei posting di questi termini è la stessa della parola $Java$.
Questo porta ad avere un indice che è in media 4 volte più lungo di quello normale. In realtà per ogni parola si aggiungono tante parole quanti sono i caratteri che la compongono.
##### Query
In generale le query si costruiscono mettendo il dollaro $\$$ alla fine e spostando tutti i caratteri in fondo finchè in fondo non compare l'asterisco $^*$.
- Dalla query $X$ si ottiene $X\$$.
- Dalla query $X^*$ si ottiene $\$X^*$.
- Dalla query $^*X$ si ottiene $X\$^*$.
- Dalla query $^*X^*$ si ottiene $X^*$.
	L'unica per cui non vale la regola sopra è questa.
- Dalla query $X^*Y$ si ottiene $Y\$X^*$.
# k-gram index
Questa tecnica risolve il problema di aumentare la lunghezza del permuterm index rispetto all'invertex index.
##### Costruzione indice
Si crea un altro indice che contiene come chiavi i $k$-gram (sequenze di $k$ caratteri) definiti sulle chiavi dell'inverted index normale; prima di definire $k$-gram si mette all'inizio e alla fine del termine il carattere speciale $\$$.
Per ogni chiave del dizionario il valore è una lista delle parole che contengono quel $k$-gram; il valore diventa quindi un puntatore alle chiavi dell'inveted index classico.
Quello che aumenta in questo caso non è tanto la lunghezza del nuovo indice quanto la lunghezza dei suoi posting. Il motivo è che ogni $k$-gram sarà contenuto in più parole.
##### Query
In questo modo se voglio cercare $ca^*o$, cioè tutte le parole che iniziano con $ca$ e finiscono con $o$, e supponendo $k=2$, si fa:
- Cerco i 2-gram: $\$c$, $ca$, $o\$$.
- Ognuno ha la sua lista di posting, cioè una lista di puntatori a parole dell'invetex index che contengono quei 2-gram.
- Faccio l'intersezione tra gli insiemi di parole di ogni 2-gram, trovando quelle parole che sono composte da tutti i 2-gram.
- L'insieme dei documenti collegati a queste parole sono le risposte alla mia query.
##### Osservazione
Con questo metodo di possono ottenere dei falsi positivi. Dopo aver ricercato infatti si deve fare un filtraggio.