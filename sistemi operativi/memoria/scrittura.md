Quando un thread ha finito di eseguire un calcolo su un dato, deve riscriverlo in memoria centrale. Il flusso è questo:
- La CPU lo scrive nella cache.
- Ci sono dei meccanismi hw che portano le informazioni dalla cache ai livelli di cache più alti, fino alla RAM.
# Problemi
Se il dato è condiviso ed è usato al momento anche da altri [thread](thread.md), allora lo stesso indirizzo di memoria centrale che contiene il dato sarà in altre cache.
La riscrittura di quel dato nella cache del core che lo ha modificato provoca un'invalidazione della riga di cache degli altri thread. Il meccasnimo hw che si occupa di questo deve riscrivere subito il dato nella memoria centrale e gli altri thread devono riprenderlo da li.
# Ottimizzazioni
Quando un [thread](thread.md) deve usare una variabile per fare delle operazioni ricorrenti, può succedere che il suo valore venga scritto nella cache (e quindi nella RAM con i meccanismi di sincronizzazione della cache) solo alla fine del compito.
In questo modo di evita di usare cicli della CPU per scrivere nella memoria.
##### Località
Quando un dato si trova nei registri e quindi la sua ultima versione non è memorizzata nella cache, si tratta di un dato locale visibile solo al core che lo sta usando.
##### Problemi
Se altri thread vogliono accedervi, magari perché la variabile è globale, allora non vedranno l'ultimo valore ma solo quello che è stato caricato dalla cache a inizio calcolo.
Per risolvere questo ci sono dei meccanismi che garantiscono la [coerenza](memory%20consistency.md#Coerenza) tra thread.