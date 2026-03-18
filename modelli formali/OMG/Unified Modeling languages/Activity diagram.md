Vuole modellare il business process, ovvero la sequenza di azioni di un sistema; è adatto sia per sistemi object-oriented sia non object-oriented.
È basato sui token (come le [Petri Net](Petri%20Net%20(PN).md); indicano dove sono i token durante il flusso di esecuzione): un'azione viene eseguita se ha un token di autorizzazione. Un token può essere moltiplicato o consumato attraverso la parallelizzazione o la gestione del flusso.
## RAPPRESENTAZIONE
### Azione
- Si rappresenta come un rettangolo smussato agli angoli.
- Rappresenta un comportamento a un determinato livello di astrazione: un'azione può essere composta da più sotto azioni; se siamo a un alto livello di astrazione si possono nascondere le sotto azioni, indicandolo con una specie di antenna.
### Flusso
- Il flusso di controllo è rappresentato da archi.
- Sopra ogni arco ci possono essere delle guardie.
- Di norma una transizione consuma un token; può essere indicato il peso della transizione, cioè quanti token consuma.
- Se c'è un collegamento molto lungo tra due azioni allora si usano i connettori: sono pallini con lettere; a stessa lettera corrisponde un collegamento.
- Un gestore di flusso (Switch) si rappresenta con un rombo, cioè un quadrato girato. In questo caso le guardie sono necessarie per scegliere quale arco prendere.
- Ci possono essere dei percorsi concorrenti che duplicano i token: da un'azione possono partire più archi. Ci possono essere delle sincronizzazioni che consuma i token: in un'azione entrano più archi (Si deve aspettare che tutti siano arrivati prima di procedere).
### Dati
- Per indicare che in una transizione passano dei dati, cioè che c'è uno scambio di informazioni, si inseriscono dei quadratini sull'azione, a inizio e fine freccia. [Esempio](Activity%20diagram%20-%20Dati.png).
- In alternativa si possono usare dei rettangoli che contengono dati e collegarli con l'azione di partenza o di arrivo. [Esempio](Activity%20diagram%20-%20Dati.png).
- Una tipo particolare di nodi sono i $centralBuffer$. Non memorizzano informazioni ma salvano i token in ingresso in modo da poterli usare in futuro. [Esempio](Activity%20diagram%20-%20Central%20buffer.png).
### Nodi
- Il nodo iniziale è rappresentato come un pallino pieno.
- Il nodo finale di attività è rappresentato con un pallino pieno cerchiato. Indica che il sistema si deve bloccare anche se a giro ci sono token che autorizzerebbero azioni.
- Il nodo finale di flusso è rappresentato da un pallino vuoto con una X; indica che il ramo sul quale agisce si interrompe, ma gli altri possono continuare a funzionare.
### Eventi
- L'attesa di un evento è rappresentato con un rettangolo con un lato a forma di V entrante.
- L'invio di un segnale è rappresentato con un rettangolo con un lato a forma di freccia.
### Eccezioni
- Il controllo delle eccezioni si rappresenta con un rettangolo con gli angoli smussati e un quadratino piccolo su un lato. La connessione tra azione e gestore è rappresentata con una freccia a fulmine.
- Se l'eccezione può essere sollevata da una sola azione allora la freccia parte dall'azione. Se l'eccezione può essere sollevata da un blocco di azioni allora all'interno del blocco si definisce evento di attesa e da li parte la freccia a fulmine. [Esempio singola azione](Activity%20diagram%20-%20Eccezione%20azione.png), [esempio blocco](Activity%20diagram%20-%20Eccezione%20blocco.png).
- Se si verifica l'eccezione tutti i token dell'azione o del blocco sono cancellati; viene eseguito il gestore dell'eccezione; poi il flusso continua con l'azione successiva a quella (O al blocco) che ha causato il problema.