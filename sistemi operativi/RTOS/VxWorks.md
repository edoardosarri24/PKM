In Vx-Works ci sono due tipi di shell: la command shell (classica shell unix) sono invocazioni programmi, la C-shell sono invocazioni di funzioni.
# C-SHELL
Se invochiamo nella C-shell una funzione (tipo $ls()$) allora vediamo che questo ritorna 0, che in C è il valore di ritorno delle funzioni che terminano correttamente.
Con $help$ possiamo vedere le funzioni eseguibili; ci sono dei programmi 
Con $i$ possiamo vedere i task attivi. Ci dice una serie di informazioni.
- Se mettiamo $temp=1$ stiamo allocando nella tabella dei simboli del kernel un nuoo simwbolo; ci dice anche la posizione in memoria.
- Siccome c'è un allineamento con i comandi da shell e da C-shell, allora anche se sto invocando delle funzioni la C-shell li considera come comandi: posso scrivere $printf\ "\%d",\ 10$ (come mando) ma anche $printf(\%d",\ 10)$.
- con $sp$ si fa lo spowning di un task. In questo caso si usano una funzione e non un comando e quindi servono le parentesi. In questo modo quando il task ha completato allora sparisce (non lo vedimao se facciamo $i$ da terminale). Per vedere che resta allora si rilascia un task periodico con $period()$.
# MODULI
Nella user space si compila un sorgente e lo si esegue. Nel kernel non si può eseguire un programma: si deve scrivere un modulo e caricarlo nel kernel.
- Quando scriviamo $void start(void){printf(""helloword")}$ e poi lo caricheremo quello che facciamo è caricare nella tabella dei simboli i simbolo start, che poi sarà chiamato da qualcuno. per caricarlo si fa file built e poi si deve riconnete il simulatore.
- Nei moduli del kernel lo spazio è flat, cioè non ci sono gerarchie. Non posso quindi avere due moduli con lo stesso nome.
- Una volta caricato posso chiamare il modulo (start) oppure darlo in gestione al gestore dei task (con $sp$ o $period$ o altre cose).
# SPAWN
sp si può fare da shell ma si può fare anche all'interno dei programmi con la funzione.