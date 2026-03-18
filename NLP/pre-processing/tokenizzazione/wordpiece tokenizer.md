È un algoritmo di [subword tokenization](tokenizzazione.md#Subword%20tokenization) che quindi ha prima una parte di apprendimento del vocabolario e poi una suddivisione dei termini in token.
# Apprendimento vocabolario
L'apprendimento del vocabolario avviene su un corpus di testo come segue:
- Si definisce una dimensione del vocabolario $V$. Questa è il numero di token che comporrà il vocabolario.
- Per ogni parola si inizializza il vocabolario con i suoi caratteri: il primi carattere è aggiunto così come è; dal secondo i poi il carattere è preceduto da $\#\#$. Se abbiamo $cat$ allora si aggiunge al vocabolario $\{c,\#\#a,\#\#t\}$.
- Si calcola una distribuzione di probabilità sul corpus di trovare insieme coppie (e successivamente sequenze) di caratteri. Ad esempio se i caratteri da unire sono $a$ e $b$ allora si calcola $score(a,b)=\tfrac{P(ab)}{P(a)\cdot P(b)}$.
- Si aggiunge al vocabolario la sequenza di caratteri che massimizza la likelihood, cioè lo $score$ di cui sopra.
- Ci si arresta quando la dimensione $V$ è stata raggiunta.
# Tokenizzazione
Dato il vocabolario, la tokenizzazione avviene seguendo il greedy longest match:
- Per ogni parola dell'input, si mappa la sequenza di caratteri più lunga che è presente nel vocabolario nel suo ID.
- Se tutta la sequenza è contenuta nel vocabolario allora è trattata come un token.
- Se un suffisso della parola avanza, allora si ripete il processo con questo.
- Se un suffisso (o un'intera parola) non è presente allora si mappa con il token OOD $[UNK]$.
# Conseguenze
- Permette di risolvere il problema dell'OOV (Out Of Vocabulary): il numero di token $[UNK]$ è basso.
- Migliora la generalizzazione, cioè la mappatura di parole non viste in token. Questo implica che con un vocabolario più piccolo si possono tokenizzare più parole.
- È un tokenizer sintattico e non semantico: stesso token può essere usato in contesti diversi.