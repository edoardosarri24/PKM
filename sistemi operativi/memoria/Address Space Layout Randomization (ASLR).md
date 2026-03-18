È una tecnica che gli OS applicano per aumentare la sicurezza del sistema.
# Funzionamento
Ogni volta che un programma viene eseguito, il sistema operativo cambia casualmente la posizione in [memoria](memoria.md) delle varie sezioni del [processo](processo.md) (i.e., stack, heap, librerie, codice).
# Utilità
Supponiamo che un programma abbia un buffer overflow.
Se un attaccante trova questa vulnerabilità durante la prima esecuzione, senza l'ASLR può sfruttarla all'iterazione successiva. Con l'ASLR attivo invece tali indirizzi di memoria cambieranno e l'attaccante non sa più dove poter scrivere.