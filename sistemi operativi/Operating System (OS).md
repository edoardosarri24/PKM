# Kernel
Il kernel è la parte dell'OS che risiede sempre nella memoria centrale.
È l'interfaccia tra le risorse hw e le applicazioni utente. Tale interfaccia è espetata tramite specifiche API dette system call, le quali sono l'unico punto di accesso ai servizi offerti dal kernel.
# Spazi
Un processo per accedere a un servizio del kernel (e.g., leggere un file dalla memoria o [allocare memoria](allocazione.md)) deve chiamare funzionalità del kernel, dette system call. 
Quando un programma che esegue in modalità utente chiama una system call, viene mandato un software interrupt al kernel che esegue l'interrupt handler appropriato in modalità kernel, cioè il codice che implementa la system call richiesta; una volta finito passa il controllo al programma chiamate in modalità utente.
### User space
È l'area di memoia dove girano i processi utente, cioè quei processi che possono eseguire un set di istruzioni limitato (e.g., non può accedere direttamente all'I/O o fermare la CPU) e può accedere solo a una parte della [memoria](memoria.md) (altrimenti segmentation fault).
Qui si trovano lo [Heap](processo.md#Heap), lo [Stack](processo.md#Stack) e le librerie etserne a cui il codice accede.
### Kernel space
È l'area di memoria dove c'è il kernel del sistema operativo, che ha controllo totale sull'hw e può eseguire qualunque istruzione.
# Standard
Definiscono la sintassi e la semantica delle system call.
Questo permette di rendere le applicazioni portabili a livello di source code: all'interno dello standard, per eseguire l'applicazione su OS diversi basta ricompilarla senza fare cambiamenti al source code.
- POSIX
	Ci sono più versioni dello standard, ognuna delle quali definisce dei servizi incrementali:
	- POSIX.1 definisce i core service.
	- POSIX.1b definisce le estensioni per il real time, come semafori, clock, timer, priority scheduling. POSIX.c definisce i thread.
	- POSIX-2 definsice la shell e le utility.
- RT-POSIX
	È l'estensione di POSIX per le applicazioni real time e fornisce i servizi per la programmazione concorrente e la predicibilità; ad esempio la sincronizzazione in muta esclusione tramite l'ereditarietà della priorità, la comunicazione tra task a seconda della priorità e lo sceduling a priorità fissa.
	Il profilo real time di POSIX è definito della versione POSIX.13.
# Boot phase
La boot phase è l'insieme delle procedure di avviamento di un OS.
- Firmware
	UEFI (prima era BIOS) controlla l'hw come CPU, memoria, dischi e tastiera.
- Bootloader
	Il firmware lancia il bootloader, che carica l'OS in memoria. Se ci sono più OS si deve scegliere quale OS caricare.
- Kernel
	Viene prima caricato il kernel in memoria, che prende il controllo del PC.
- Avvio
	Vengono chiamate le prime system call come init/systemd. Queste lanciano i servizi di rete, montano il file system.
- Login
	L'utente si deve loggare e il tutto inizia.