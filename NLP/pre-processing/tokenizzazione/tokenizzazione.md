È uno dei passaggi fondamentali del [pre-processing](pre-processing.md) del testo.
Solitamente una tokenizzazione, oltre al processo di trasformare una parola in un token, mappa anche questo token in un ID unico, dove la relazione tra token e ID è contenuta in un vocabolario.
# Difficoltà
Per passare da un termine a un [token](unità.md) ci sono molti aspetti complicati:
- Nel parlato ci possono essere parole che non sono token, come le disfluenze (i.e., gli "mmm").
- Il punto può essere un separatore ma può far parte anche della parola. Esempio sono: USA è U.S.A.; oppure un indirizzo IP.
- Si devono gestire i caratteri speciali.
- Si devono gestire gli idiogrammi.
- Ci sono stessi concetti che possono essere rappresentati in modo diverso. Ad esempio le date in $mm-gg-aaaa$ oppure $gg-mm-aaaa$.
# Subword tokenization
In un algoritmo di tokenizzazione sub-words, i token non corrispondono per forza ai termini, ma possono riferire anche a loro parti (sub-word); queste parti solitamente sono i [morfemi](unità.md#Morfemi).
Questo tipo di algoritmi, invece che usare regole statiche, crea il tokenizer in modo automatico a partire dai dati: non siamo più noi che definiamo le regole ma è il modello che, in base ai dati, decide come tokenizzare l'input per ottenere la sua migliore rappresentazione.
##### Vantaggi
- Questo migliora ad esempio il problema dell'OOD (Out Of Distribution) e il problema di vocabolari enormi.
- Se si vuole uscire dall'idea di tokenizzare le parole allora si può pensare di tokenizzare una loro parte: i caratteri è troppo a grana fine; i k-gram andrebbero bene ma è complesso scegliere il valore di $k$; la scelta ricade sui [morfemi](unità.md#Morfemi).
##### Parti
Si basa su due parti:
- Learner
	Definisce un vocabolario, composta da quelle parti di parole che compaiono più spesso. A ogni elementi del vocabolario, cioè ad ogni token, si associa un ID univoco.
- Segmenter
	Esegue la segmentazione delle parole in input secondo il vocabolario.
##### Algoritmi
- [wordpiece tokenizer](wordpiece%20tokenizer.md).
- [Byte Pair Encoding (BPE)](Byte%20Pair%20Encoding%20(BPE).md)