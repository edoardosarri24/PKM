Comporre oggetti in strutture ad albero. Utile quando si hanno dati ricorsivi.
- Strutturale.
- Inheritance e object composition
## STRUTTURA
![[Composite.png]]
- $Component$
	Dichiara i membri comuni a tutta la struttura; eventualmente implementa le parti comuni altrimenti lascia stratto. Può introdurre un riferimento al padre, che sarà di tipo statico $Composite$.
- $Leaf$
	Implementa le operazioni che deve eseguire. Ci può essere una classe base.
- $Composite$
	Memorizza i figli, di tipo statico $Component$, in una lista. Implementa le operazioni delegandole ai figli.
- $Client$
	Si interfaccia col tipo astratto $Component$.
## CONSEGUENZE
- È facile aggiungere nuovi partecipanti alla struttura.
- Non si possono imporre dei vincoli sul tipo o il numero dei figli.
## VARIANTI
Le operazioni per gestire i figli non hanno senso se implementate in $Leaf$; devono essere implementate perché sono dichiarate in $Component$.
- Design for uniformity
	In $Leaf$ sono implementate sollevando delle eccezioni, non facendo nulla o restituendo $null$.
	Il client non fa distinzione tra $Leaf$ e $Composite$, ma si rompe l'[[Liskov sobstituition principle|LSP]].
- Design for type safety
	Sono implementate e dichiarate in $Composite$.
	Se il client deve solo accedere alla struttura allora può avere un riferimento al tipo statico $Component$; se deve modificarla allora il riferimento deve essere al tipo statico $Composite$.
	Il vantaggio è che non si hanno problemi a runtime.