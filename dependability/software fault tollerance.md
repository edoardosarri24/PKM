Il software è la causa principale del [fallimento](fault%20error%20failure%20chain.md#Failure) dei sistemi. La difficoltà principale è il numero di stati che il sistema può assumere, come essi sono in relazione e i cambiamenti al sw che avvengono di frequente.
# Indipendenza
In componenti ridondati dove è presente software succede spesso che gli errori siano duplicati, cioè siano gli stessi per ogni componenti, e la motivazione sta nella mancanza effettiva di [diversity](ridondanza.md#Design%20diversity); questa infatti è fattibile e semplice da implementare in hardwere, ma complicata e spesso non raggiunta a causa dello stesso trattamento che input (i.e., gli stati) che portano a errori subiscono.
##### Cause
La correlazione tra versioni diverse, cioè il trattamento degli input nello stesso modo anche se il software è stato progettato in modi diversi, deriva da alcuni aspetti:
- Un errore nella specifica si propaga nell'implementazione.
- Algoritmi comuni tra versioni diverse.
- I programmatori solitamente fanno sempre gli stessi errori.
- Librerie e piattaforme comuni.
##### Soluzioni
Per raggiungere l'indipendenza, cioè per far si che i fallimenti di un software siano disgiunti, ci sono delle tecniche:
- Le versioni devono essere sviluppate da persone diverse senza che queste cooperino.
- Si dovrebbero usare diverse librerie e diversi linguaggi di programmazione.
- Si deve implementare perfettamente le specifiche.
- Una terza persona deve fare una review dell'implementazione.
# Approcci strutturati
L'idea è di usare la [ridondanza](ridondanza.md) del software (versioni diverse).
##### Classificazione
Possiamo usare varie classi di approcci anche in combinazione.
- Error detection
	Ci sono due tecniche: il confronto del risultato con quello ottenuto da un'altra unità; l'asserzione del risultato con i valori contenuti nelle specifiche.
	La scelta deve essere fatta basandosi sui costi e sulla copertura.
- Error correction
	Lo stato corretto può essere recuperato in vari modi:
	- Backword recovery
		Si torna all'ultimo stato corretto e si esegue nuovamente la computazione fallita.
	- Voting
		Non ci interessa se una computazione fallisce, ma utilizziamo solo quella prodotta dalla maggioranza.
	- Forward recovery
		Si va in uno stato corretto che in teoria non è stato ancora raggiunto.
- Esecuzione distribuita
	Gli elementi ridondanti possono essere esguiti in parallelo o sequenzialmente, sulla stessa macchina o in modo distribuito.
- Diverso uso della ridondanza
	La ridondanza può essere usata sempre (spesso per la error detection) oppure on demand.
- Fault unit con granularità diversa
	Una fault unit è una black box di cui noi osserviamo il fallimento, senza sapere dove esso è avvenuto all'interno. Se questa è piccola abbiamo una scoperta dell'errore più rapida, l'errore non viene propagata e possiamo fare una diagnosi migliore; se è più grande abbiamo un costo maggiore.
##### Tecniche
Ci sono varie tecniche con cui implementare quanto sopra.
- Recovery blocks (RB)
	Ci sono due versioni indipendenti, $P$ e $S$, ognuna delle quali ha un proprio $AT$: la prima è sempre attiva, la seconda entra in funziona quando l'$AT$ della prima non è soddisfatto del risultato. Oltre all'$AT$ funzionale può esserci un watchdog timer che controlla vincoli temporali sulla risposta. Se sia $P$ che $S$ falliscono allora il sistema fallisce.
- N-version programmin (NVP)
	Ci sono $N$ versioni indipendenti e un voter che fa passare il risultato della decisione a maggioranza. La classica è 2oo3.
	La difficoltà sta nell'implementazione del voter: deve essere semplice per non avere guasti; spesso nel software, a differenza dell'hardware, i risultati sono sempre diversi e quindi deve capire quali di essi sono in accordo.
# Rejuvination
Il software dopo un po' che è in esecuzione può invecchiare: memory leak oppure coda dei processi della CPU troppo lunga.
La rejuvination può essere fatto a livello di sistema o di singola applicazione.
Può essere (anche combinandoli insieme):
- Time based
	Usato per sistemi che hanno dei momenti di inutilizzo. In questi momenti si può riavviare senza problemi.
- Prediction based
	Usato quando il sistema non può essere riavviato spesso senza problemi o perdite economiche. Si deve fare il ringiovinamento in base alla previsione fatta su quando il sistema andrà in crash.
# Safety net approach
Sono tecniche general-purpose usate al momento del bisogno all'interno del codice per trovare e gestire errori che si presentano in situazioni generiche (e.g., apertura di un file non esistente, passaggio di argomenti errati o $null$).
##### Tecniche
Le classiche tecniche sono:
- Exception hangling
	La classica implementazione è il $try-catcc$, cioè gestiamo l'errore nello stesso momento in cui viene sollevato.
- Interface expection
	L'errore viene propagato e gestito da un altro componente.
	Un esempio è il passaggio di parametri errato.