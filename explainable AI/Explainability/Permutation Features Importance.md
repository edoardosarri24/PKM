Si tratta di una tecnica di [Explainability](Explainability.md) di tipo [model inspection](Metodi.md#Model%20inspection), model agnostic e usato per dati tabulari.
Si stima l'importanza della feature $j$-esima facendo una permutazione casuale del valore di quella feature tra tutti i salmple del train set.
# Funzionamento
Data una blackbox $f$ e un test set $X$, calcolo l'importanza di ogni features $j$ permutando casualmente i valori di quella feature per tutti i samples del test set $X$.
Prima questa permutazione avevo un errore $Err_{origin}=\tfrac{1}{N}\sum_{i=1}^NL(y_i,f(x_i))$; dopo la permutazione ho un errore $Err_{perm_j}=\tfrac{1}{N}\sum_{i=1}^NL(y_i,f(x_i^{p_j}))$. A questo punto l'importanza della features è data da $I_j=\frac{Err_{origin}}{Err_{perm_j}}$.
##### Conseguenze
- Se la feature $j$ avesse un valore costante allora quella feature non è minimamente importante perchè $I_j=1$.
- Se i pesi associati a quella feature sono bassi l'errore sul test set cambierà molto poco; se invece il peso associato alla features è alto si otterrà una differenza di errore molto elevata.
- Lo svantaggio è che si possono generare degli esempi [out of distribution](Varie.md#Out%20of%20distribution), cioè si possono generare esempi che hanno poco senso. Quello che si può fare per risolvere questo problema è permutare le feature non casualmente ma all'interno di sottogruppi: si dividono i test in gruppo (e.g. maschi/femmine) e le permutazioni avvengono all'interno di questi sotto gruppi.