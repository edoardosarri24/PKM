Il text classification è un task di [apprendimento supervisionato](Apprendimento%20supervisionato.md) svolto dal [text mining](text%20mining.md) o dell'[NLP](NLP.md): dato un documento $d$ e un insieme di classi $C=\{c_1,\cdots,c_n\}$, volgiamo predire la categoria $c$ a cui il documento appartiene.
Viene applicato in task come la sentiment analysis o lo spam detection.
# Regole
L'approccio più semplice è basato su regole.
- Si ottiene un'alta precision (pochi falsi positivi), visto che le regole sono fatte a mano da esperti.
- Manutenere le regole o crearle è molto complesso.
# Naive bayes
Si rappresentano i documenti come [bag of words](modelli%20IR.md#Boolean%20model) e ci si basa sulla regola di bayes: la probabilità che il dato documento $d$ sia di classe $c$ è dato da $P(c|d)=\tfrac{P(d|c)P(c)}{P(d)}$, dove $P(d|c)$ è la likeliwood e rappresenta la probabilità di un documento data la classe, cioè che un documento di quella classe contenga esattamente quelle parole.
La classe predetta è quella che massimizza la probabilità a posteriori, cioè si ha che $c=\displaystyle \arg\max_{c\in C}P(c|d)=\arg\max_{c\in C}\tfrac{P(d|c)P(c)}{P(d)}= \arg\max_{c\in C}P(d|c)P(c)$ (perché $P(d)$ non dipende da $c\in C$ ed è ugule quindi per tutte le classi).
Il problema di questo approccio e che, se un documento $d$ è composto da $x_1,\cdots,x_n$, allora si ha $c=\displaystyle\arg\max_{c\in C}P(x_1,\cdots,x_n|c)P(c)$. La complessità di questa cosa è $\mathcal{O}{(|X|^n*|C|)}$, dove $X$ è l'insieme delle parole; questo perché un documento è una delle possibili combinazioni delle parole del vocabolario.
##### Multinomial
Avendo rappresentato i documento con i bag of words sappiamo che la posizione delle parole non conta.
Supponendo allora che le probabilità $P(x_i|c)$ siano indipendenti $\forall i$, possiamo scrivere $P(x_1,\cdots,x_n|c)=P(x_1|c)\cdot\cdots\cdot P(x_n|c)$ e quindi abbiamo che $c=\displaystyle\arg\max_{c\in C} P(c)\prod_{x\in X}P(x|c)$.
Il problema è che stiamo moltiplicando molte probabilità piccole; per evitare un underflow e avere 0 come risultato per qualunque documento allora si possono usare i logaritmi (ricordando che $log(ab)=log(a)+log(b)$) e ottenere $c=\displaystyle\arg\max_{c\in C}(logP(c)+\sum_{x\in X}logP(x|c))$.
##### Osservazioni
- Aver aggiunto il logaritmo non cambia il ranking delle classi, visto che si tratta di una funziona monotona crescente.
- Si tratta di un classificatore lineare: una funzione lineare nell'input.
- Rimuovere le stop words non migliorare il metodo.
##### Apprendimento
Per imparare il classificatore si usa: la prior $P(c)$ è $P(c)=\tfrac{N_c}{N_{total}}$, cioè il rapporto tra il numero di documenti di quella classe sul numero totale di documenti; se la likeliwood si usa $P(x_i|c)=\tfrac{count(x_i \text{ in }c)}{\sum_{x'\in c}count(x'\text{ in }c)}$, cioè si conta quante volte la parola compare in documenti di quella classe sul numero di parole in documenti di quella classe.
- Se una parola manca in una classe, e quindi $P(x_i|c)=0$, allora si può usare la tecnica di Laplace smoothing: si aggiunge 1 al numeratore e al denominatore si ottiene $P(x_i|c)=\tfrac{count(x_i \text{ in }c)+1}{\sum_{x\in c}(count(x\text{ in }c)+1)}$.
- Se nel test compare una parola che non era nel train set, cosa diversa rispetto al caso precedente, una possibile soluzione è ignorarla.
# MLP
La prima soluzione che ha utilizzato i [neural language model](language%20model.md#Neural%20language%20model). Un task di classificazione può essere la sentiment analysis.
![sentiment analysis in NLM](sentiment%20analysis%20in%20NLM.png)
Il problema principale del MLP è che ha dimensioni fisse nell'input: se la frase è più piccola o più grande di quella per cui il modello è pesato abbiamo dei problemi.
Ci sono due cose che possiamo fare:
- Input lungo
	Assumere che le frasi siano le più lunghe possibili e aggiungere tanti padding quante sono le parole che mancano per raggiungere tale lunghezza.
- Trocamento
	Troncare la frase se la lunghezza è maggiore di quella prefissata. In questo modo però si potrebbero perdere informazioni importanti.