La probabilità condizionata serve a stabilire la probabilità di un evento dato che un altro è già accaduto.
È definita formalmente come $P(A|B) = \frac{P(A\cap B)}{P(B)}$, dove $P(A\cap B)=P(A,B)$ è la portabilità congiunta. Stiamo quindi normalizzando la probabilità congiunta con la probabilità di $B$.
##### Variabili indipendenti
Se le variabili sono indipendenti allora abbiamo, ma queste sono indipendenti allora abbiamo $P(A\mid B) = P(A)$.
# Probabilità congiunta
C'è una relazione tra la probabilità condizionata e congiunta che deriva dalla definizione: $P(A,B)=P(A|B)P(B)$.
# Formula di Bayes
Permette di invertire la condizione di una probabilità condizionata come $P(A \mid B) = \frac{P(B \mid A)  P(A)}{P(B)}$.
# Chain rule
Serve per scomporre in più parti una probabilità condizionata su più eventi: la probabilità che tutti gli eventi accadano contemporaneamente è uguale al prodotto delle probabilità di ogni singolo evento condizionato solo a quelli che lo precedono; con questa idea si impone un ordine (non importa quale sia) negli eventi.
In particolare si ha $P(A_1, A_2, \dots, A_n) = \prod_{i=1}^{n} P(A_i \mid A_1, A_2, \dots, A_{i-1})$.
##### Indipendenza
Se gli eventi sono indipendenti allora $P(A_i \mid A_1, A_2, \dots, A_{i-1}) = P(A_i)$ e quindi la chain rule diventa $P(A_1, A_2, \dots, A_n) = \prod_{i=1}^{n} P(A_i)$.