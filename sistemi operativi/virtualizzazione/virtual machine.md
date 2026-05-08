Una virtual machine (VM) è una [virtualizzazione](virtualizzazione.md) a livello fisico, cioè vuole virtualizzare l'hardware dell'intera macchina host.
# Hypervisor
Per fare questo utilizza un software chiamato hypervisor su cui è possibile installare più [OS](Operating%20System%20(OS).md) guest indipendenti; ogni OS guest vede tutte le risorse della macchina e le percepisce come proprie, ma in realtà sono virtuali.
Ci sono due [tipi di hypervisor](hypervisor%20type.png):
- La prima (il tipo B nella foto) è la classica dove l'OS host è presente e possiamo usarlo. In pratica possiamo installare applicazioni sull'OS host: questo spazio in computer personali è la maggior parte; in server è pochissimo usato magari per applicazioni di monitoraggio.
- La seconda è la situazione in cui non si deve usare l'OS host. In questo caso allora possiamo eliminarlo definitivamente e usare l'hypervisor per gestire le risorse fisiche e parlare con gli OS gust.
# Conseguenze
- Ogni VM è al sicuro: se una crasha o viene infettata le altre e l'host non ne risentiranno.
- Le VM sono meno efficienti rispetto ai container e solitamente utilizzano più risorse. Ad esempio quando si deve scrivere in memoria è l'OS guest parla con l'hypervisor e questo parla con l'OS host che lavora sulle risorse fisiche.
- L'avvio di una VM è molto più lento, sopratutto per la [fase di boot](Operating%20System%20(OS).md#Boot%20phase).