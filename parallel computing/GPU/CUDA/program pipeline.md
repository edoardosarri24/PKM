Un programma [CUDA](CUDA.md) solitamente segue la sequente pipeline:
- Alloca la memoria della GPU.
- Trasferisce i dati da elaborare dalla CPU alla GPU.
- Invoca il kernel, cioè la funzione che gira sulla GPU.
- Il kernel è asincrono e quindi l'host deve sincronizzarsi sul risultato della memoria.
- Liberare la memoria della GPU.
# Dichiarazione funzioni
Ci sono più modi per dichiarare una funzione:
- $\_\_device\_\_$
	È una funzione che gira sul device chiamata dal device.
- $\_\_global\_\_$
	È una funzione chiamata dall'host che gira sul device: è solitamente l'entry point della GPU.
	È una funzione asincrona e quindi deve ritornare $void$.
- $\_\_host\_\_$
	È una funzione chiamata ed eseguita dall'host. Può essere omessa perché di default le funzioni sono dichiarate così.
##### Duplice
Una funzone può essere dichiarata come $host$ e $device$.
In questo caso viene compilata due volte: una produce un eseguibile [C](C.md) o [C++](C++.md) e una il [PTX](CUDA.md#Compilazione).