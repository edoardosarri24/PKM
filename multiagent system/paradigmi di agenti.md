Ci sono due modi principali, con tutte le sfumature possibili, in cui un [agente](agenti.md) può agire: deliberato o reattivo.
Oggi si parla più di agenti ibridi, cioè che agiscono contemporaneamente in entrambi i modi.
# Deliberato
![deliberate paradigm](deliberate%20paradigm.png)
Il modello deliberato si basa sul fornire all'agente un modello del mondo su cui esso può fare predizione.
##### Flusso
- L'agente interroga l'ambiente (non tutto il mondo come in figura) tramite i sensori e ne percepisce lo stato.
- Aggiorna il modello basandosi sull'[osservazione](modello.md#Observation%20model) fatta e sul proprio stato.
- Sulla base delle possibili azioni esegue una predizione del futuro basandosi sull'attuale modello, cioè definisce una lista di azioni per raggiungere l'obiettivo.
- Esegue la prima azione della lista. In questo modo cambia il mondo, cioè il prorpio stato e quello dell'ambiente.
- Inizia nuovamente il ciclo.
##### Conseguenze
- È una tecnica usata per svolgere compiti specifici, dove il mondo può essere modellato in modo preciso.
- Se l'ambiente è molto complesso, allora il modello di esso sarà una semplificazione eccessiva.
- Per la maledizione della dimensionalità se il modello è troppo complesso sarà troppo costoso fare previsione.
- Se l'ambiente è molto dinamico allora la previsione sul futuro non avrà senso tra pochi istanti di tempo e diventa difficile seguire una pianificazione a lungo termine.
# Reattivo
![reactie paradigm](reactie%20paradigm.png)
Il modello reattivo è model-less e si basa solamente sulle informazioni percepite dai sensori.
##### Comportamento
Dopo aver raccolto informazioni dall'ambiente, l'agente produce una lista di comportamenti. Un comportamento è un'azione semplice che ha un singolo obiettivo.
##### Fusion
L'agente esegue la migliore azione secondo un qualche criterio:
- Esegue una sola azione, ad esempio quella che ha il peso maggiore.
- Esegue più azioni contemporaneamente.
- Si basa su una gerarchia: i comportamenti più di basso livello vengono eseguiti immediatamente senza valutare quale altro comportamento sia migliore. Un esempio per un robot è evitare di cadere dalle scale. Questo permette di avere delle azioni per cui la latenza è minima; lo svantaggio è che non si riesce a fare pianificazione.
##### Conseguenze
- È una tecnica usata per svolgere compiti generali e che non necessitano di una pianificazione a lungo termine.
- Se i sensori non funzionano o l'ambiente li limita, questo paradigma non può essere usato.
- Se il modello del mondo semplifica troppo la realtà allora è una buona soluzione.