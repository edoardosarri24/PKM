In un codice ci possono essere istruzione che sono in relazione tra oro, cioè che sono dipendenti l'una dall'altra. Questo avviene solitamente quando gli indirizzi delle variabili sono condivisi da istruzioni diverse.
Queste istruzioni dipendenti, spesso non possono essere [parallelelizzate](parallel%20pragramming.md), ma devono sempre essere eseguite dallo stesso processo.
È importante eliminare le dipendenze perché quando poi andiamo a parallelizzare il codice è sempre bene che i vari processi non siano dipendenti.
##### Funzioni esterne
Quando si valuta le dipendenze non si deve guardare solo le nostre istruzioni, ma anche le librerie esterne che si usano.
Una funzione potrebbe usare uno stato, che se condiviso tra thread allora potrebbe causare problemi. Lo stato in questo caso va memorizzato nello spazio del thread e non nella memoria globale.
Per questi motivi si devono preferire librerie che sono thread-safe.
# Flow dependance
Si parla di dipendenza **read after write**.
Solitamente non può essere eliminata.
# Anti dependance
Si parla di dipendenza **write after read**.
Solitamente può essere eliminata usando variabili diverse nel codice, in modo che quando esso viene parallelizzato queste diventano variabili locale al processo che le esegue.
# Output dependance
Si parla di dipendenza **write after write**.
# Input dependance
Si parla di dipendenza **write after write** ed è detta anche falsa dipendenza. Infatti non comporta nessun problema dividere due letture in più processi.