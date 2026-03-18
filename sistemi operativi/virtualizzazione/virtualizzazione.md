La virtualizzazione è un modo per creare più risorse astratte all'interno di un singolo server.
La virtualizzazione viene implementata in due modi diversi: tramite Virtual Machine e tramite container. È una buona pratica anche usarle insieme: in un server solitamente sopra l'hw non c'è un sistema operativo singolo o reale, ma l'OS viene virtualizzato tramite VM. In una VM non gira una sola applicazione, ma più applicazioni, ognuna delle quali è all'interno di un container.
![container vs VM](container%20vs%20VM.jpeg)
# Container
Un [container](container.md) è un'astrazione a livello di sistema operativo, cioè si vuole virtualizzare solo il software su cui l'applicazione gira.
Ogni applicazione che gira nel proprio container gira in un [user space](Operating%20System%20(OS).md#User%20space) dell'OS host diverso, ma condivide lo stesso kernel. Non percepisce di avere un'intera macchina a disposizione, ma è isolato rispetto ai processi, alle reti e al file system,.ono processi isolati che vengono eseguiti in [user space](Operating%20System%20(OS).md#User%20space) diversi, ma condividono le stesse chiamate di sistema del sistema operativo host.
##### Conseguenze
- Sono più efficienti di una VM perché sono più leggeri e richiedono quindi meno risorse.
- L'avvio di un container è solitamente molto veloce.
# Virtual machine
Una virtual machine (VM) è un'astrazione a livello fisico, cioè vuole virtualizzare l'hardware dell'intera macchina host.
Per fare questo utilizza un software chiamato hypervisor su cui è possibile installare più [OS](Operating%20System%20(OS).md) guest indipendenti; ogni OS guest vede tutte le risorse della macchina e le percepisce come proprie.
##### Conseguenze
- Ogni VM è al sicuro: se una crasha o viene infettata le altre e l'host non ne risentiranno.
- Le VM sono meno efficienti rispetto ai container e solitamente utilizzano più risorse.
- L'avvio di una VM è molto più lento, sopratutto per la [fase di boot](Operating%20System%20(OS).md#Boot%20phase).