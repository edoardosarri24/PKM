Il Compute Unified Device Architecture (CUDA) è un framework che permette di programmare le [GPU](GPU.md) NVIDIA in modo general-purpose.
# Platform
La piattaforma CUDA permette di usare vari livelli di astrazione del codice:
- Linguaggi
	Il codice del device solitamente è scritto in CUDA C, mentre quello dell'host in [C](C.md) o [C++](C++.md). È possibile scrivere tutto in un unico file $.cu$ oppure usare file diversi.
- API
	CUDA fornisce due livelli di API:
	- Driver API
		Sono di livello più basso e quindi più complesse. Aumentano le prestazioni ma non così da tanto da giustificare il loro utilizzo.
	- Runtime API
		Sono di più alto livello, hanno prestazioni peggiori ma non tanto da perdere la loro semplicità d'uso.
- Librerie
# Compilazione
![cuda program](cuda%20program.png)
Un programma CUDA $.cu$ è composto sia da [host code](GPU.md#Host%20vs%20device) che device code.
Il compilatore CUDA $nvcc$ compila separatamente i due codici che dovranno essere eseguiti in device diversi. La compilazione segue varie fasi:
- Si separa il codice host da quello del device.
- Il codice host viene compilato dal compilatore adeguato sul sistema.
- Il codice del device viene prima tradotto in un linguaggio assembly intermedio PTX (Parallel Thread eXecution) e poi viene generato il SASS specifico per un'architettura targhet.
##### Fat binary
È possibile anche non generare il SASS, ma eseguire il PTX con un JIT compiler.
Per evitare questo costo a runtime possiamo includere il binario per più architetture in quello che è il fat binary.