Le espressioni regolati sono un modo formale per ricercare specifiche stringhe in un testo naturale.
È parte integrante del [NLP](NLP.md), ma in generale viene usato per estrarre informazione dal testo e convertirlo da una forma naturale a una standard.
Il tool più utilizzato è $re$ in Python.
# Regole
Si usano quasi sempre gli slash $/$ seguiti da qualcosa.
- Disgiunzione
	Per dichiarare una serie di scelte si usano le parentesi quadre in cui inseriamo le possibili scelte. Ad esempio se volgiamo sia "Ciao" che "ciao" e sia "bau" che "Bau" allora cerchiamo $/[Cc]iao/$ e $/[Bb]au/$.
- Congiunzione
	Si usa il segno di pipe concatenando le più cose ricercate.
	Ad esempio se volgiamo sia "Ciao" che "ciao" e sia "bau" che "Bau" allora cerchiamo $/[Cc]iao|[Bb]au/$.
- Range
	Se volgiamo una scelta in un range si usa il $-$.
	Ad esempio per tutte le cifre si ha $/[0-9]/$.
- Negazione
	Si usa ^ che precede l'elemento negato tra le parantesi quadre. 
	Ad esempio per dire tutto tranne il 6 si usa $\^[6]$.
# Option e wildcard
- $?$ rende opzionale il carattere precedente.
- $*$ indica zero o più istanze del carattere precedente.
- $+$ indica una o più istanze del carattere precedente.
- $.$ indica un qualunque carattere.
- $\{n\}$ indica esattamente $n$ istanze del carattere precedente.
- $\{n,\}$ indica almeno $n$ istanze del carattere precedente.
- $\{,m\}$ indica al più $m$ istanze del carattere precedente.
# Ancore
- ^, senza quadre, indica l'inizio di una linea.
- $, senza quadre, indica la fine di una linea.
- \b indica la parola confinata. Ad esempio "the" isolata si indica /\bthe\b/.
# Operatori
Ci sono degli operatori per abbreviare le cose. Le maiuscole di queste indicano la loro negazione.
- \d indica qualunque cifra.
- \w indica qualunque carattere alfanumerico e l'underscore.
- \s indica un white space (i.e., space, tab, newline).