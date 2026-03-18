Il Part Of Speech (POS) tagging è una tecnica basata su regole che consiste nell'assegnare una categoria a ogni parola del testo: è quindi una funzione che mappa un input $x$ in un output $y$, dove $x$ è un vettore di caratteri e $y$ è un vettore di categorie; in poche parole è l'analisi grammaticale.
# Osservazioni
- Può succedere che una parola abbia più di un POS.
- Solitamente circa il 97% dei tag sono corretti. Questa percentuale viene raggiunta con i migliori metodi; la baseline (raggiunta con metodi base) è 92%.
# Task
Ci sono molti usi che si può fare di questa cosa nell'NLP:
- Si può fare un parser sintattico.
- Si possono riordinare le parole secondo le categorie.
- Sentiment analysis
	Si valuta il sentiment di una frase dalla qualità dei suoi aggettivi.