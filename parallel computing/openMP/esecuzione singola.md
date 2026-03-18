# Single
Con $\#pragma\ omp\ single$ si permette di eseguire una parte di codice solo al primo thread che la incontra; questo thread può essere uno qualunque.
##### Barriera implicita
Definsice una [barriera implicita](barrier.md#Implicita) alla fine del blocco. Questo vuol dire che nessun thread può continuare finché il thread che sta eseguendo il blocco single non ha terminato.
È utile quando si deve fare delle inizializzazioni solo una volta.
# Master
Con $\#pragma\ omp\ master$ si definisce un blocco di codice che deve essere eseguito dal main thread, quello che ha eseguito il fork e che ha $id=0$.
##### Barriera implcita
È importante osservare che non viene definita nessuna barriera implicita in questo caso.
Ithread che non sono il master continuano con le istruzioni dopo tale blocco.