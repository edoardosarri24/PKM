Ogni oggetto ha un numero finito di stati durante la sua vita. Lo state machine diagram è un diagramma comportamentale usato per:
- Seguire i possibili stati degli oggetti o del sistema.
- Mostrare le transizioni da uno stato dopo un evento.
- Mostrare il comportamento degli oggetti o del sistema in ogni stato.
## Rappresentazione
- Gli stati sono rappresentati come nodi.
- Lo stato iniziale è un pallino pieno.
- Lo stato finale è un pallino pieno con un cerchio esterno.
- La "X" è lo stato di terminazione errata e indica che la macchina non è più eseguibile o accessibile.
## Comportamento
Le attività all'interno di uno stato sono:
- $entry/action$
	L'azione viene eseguita all'ingresso nello stato. 
- $do/action$
	L'azione viene eseguita all'intenro dello stato
- $exit/action$
	L'azione viene eseguita all'uscita dalla stato. Se la transizione ha un'azione allora prima si esegue quella dello stato relativa alla $exit$ e poi quella della transizione.
## Transizione
Una transizione è rappresentata con una freccia tra due stati ([Esempio](State%20machine%20diagram%20-%20Transizione.png)). È composta da:
- Evento
	Possono essere un segnale, un time event, eventi vari indicati con $all$, un evento di completamento.
- Guardia che deve essere vera (Opzionale e inserita tra parentesi graffe). Se ci sono più condizioni si può usare un nodo di decisione, rappresentato con un quadrato girato. [Esempio.](State%20machine%20diagram%20-%20Nodo%20di%20decisione.png)
- Azione eseguita al momento della transizione che è separata dall'evento e dalla guardia con $/$.
- Ci possono essere parallelizzazioni o sincronizzazioni: le prime sono più frecce che escono da una sola freccia e indicano azioni parallele; le seconde sono più frecce che entrano in una e indicano che non si può proseguire finché tutte le azioni non sono concluse.
## Stati composti
All'interno di un digramma ci possono essere stati composti, cioè formati da più stati, rappresentati con un unico stato. Servono per lavorare a layer e avere astrazioni. [Esempio](State%20machine%20diagram%20-%20Stato%20composto.png).
- Se una transizione entra nello stato composto allora questo deve avere uno stato iniziale.
- Se una transizione entra in un sottostato lo stato composto non necessita di uno stato iniziale. Si deve indicare il sottostato in cui si entra con una freccia e un pallino vuoto con il nome del sottostato; Stesso discorso per l'uscita, solo che il pallino ha una "X" all'interno.
- All'interno di uno stato composto ci possono essere due sezioni separate da una linea tratteggiata. Questo significa che le due porzioni sono attive contemporaneamente. Solitamente viene usata dopo una parallelizzazione. [Esempio](State%20machine%20diagram%20-%20Stato%20composto%20parallelo.png).