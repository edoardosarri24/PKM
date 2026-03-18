È la tecnica che permette la condivisione di un canale fisico tra più utenti.
In generale quando gli utenti aumentano conviene avere un canale in grado di trasportare più segnali più tosto che aumentare il numero di linee.
## FDM
- È la tecnica a divisione di frequenza. Il canale principale viene diviso in sottocanali, detti canali FDM. In base al numero di canali FDM aggregati nel canale principale si ottiene la gerarchia FDM: si va da 8 canali FDM a 10800.
- In invio il dispositivo acquisisce un canale FDM e lo utilizza per trasmette la propria informazione.
- In ricezione si usa il filtro passabanda: si manda il canale principale in una struttura che ha come unica porta aperta quella relativa al canale FDM desiderato.
- È usato nella [telefonia analogica](Telefonia.md#Telefonia%20analogica).
## TDM
- È la tecnica a divisione di tempo. Il canale viene assegnato in modo esclusivo ad ogni utente per il tempo di slot; ogni utente della rete ha usato uno slot dopo il tempo di trama (Frame).
- In base al numero di slot in ogni trama si ottiene la gerarchia PCM: si va da 32 slot, di cui 2 sono per la gestione del servizio, a 8828 slot. Questo non è uno standard mondiale: in America il primo livello della gerarchia unisce infatti 25 canali e non 32.
- È usato nella [telefonia numerica/digitale](Telefonia.md#Telefonia%20numerica/digitale).