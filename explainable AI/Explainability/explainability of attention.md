L'[attention](attention.md) non è nata per la XAI, ma successivamente ci siamo chiesti se essa sia interpretabile per natura; più nello specifico si sono chiesti se i pesi dell'attention portano a una qualche spiegazione.
I lavori più famosi sono [Attention is not Explanation](https://arxiv.org/abs/1902.10186) e [Attention is not not Explanation](https://arxiv.org/abs/1908.04626).
# Attention is not explaination
Si fanno due esperimento nell'articolo:
- Si è valutato quanto gli attention weights sono correlati con altre misure di features importance.
	Il risultato è che gli attention weights non correlano così bene con altre tecniche per valutare l'importanza delle features.
- Si valuta l'impatto di una permutazione (sia random che avversarial) degli attention weights.
	Il risultato è che una permutazione produce una spiegazione diversa.
# Attention is not not explaination
Vengono obiettati i due esperimenti:
- Per il primo esperimento viene obiettato che il risultato ottenuto è valido anche per altri modelli, cioè che le features importanti dipendono dalla tecnica che si usa.
- Il secondo articolo viene obiettato affermando che esso valuta solo una spiegazione di tipo faithfull, mentre la spiegazione usando gli attention weights potrebbe essere comunque plausibile.