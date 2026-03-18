Il classico fine tuning dove si aggiornano tutti i pesi con [GD](Gradient%20descent.md) negli [LLM](LLM.md), come in tutte le reti neurali, ha dei grandi limiti:
- Servono molti dati annotati
- È molto costoso in termini di tempo.
- Il nuovo modello deve essere memorizzato totalmente.
- Può portare al catastrophic forgetting, dove il modello apprende il nuovo task ma non si ricorda di quelli vecchi.
# Instruction tuning
Quando abbiamo un modello pre-trained abbiamo una grande biblioteca senza un bibliotecario: il modello sa molte cose ma queste sono difficili da reperire.
L'intruction tuning serve per insegnare al modello a capire l'intendo che ha l'utente nei prompt; viene eseguito usando dati annotati presentati come coppie domanda-risposta.
Questo permette inoltre di addestrare il modello a risolvere un task A, senza che questo task sia effettivamente presente nei dati usati per l'istruction tuning; questo ci fa capire come la comprensione del testo, imparata dal modello con queste coppie domanda-risposta, sia fondamentale in moltissimi task di NLP e come questi effettivamente siano correlati.
# PEFT
Il Parameter Efficient Fine-Tuning (PEFT) è una tecnica che permette di spcailizzare il modello cambiando solo una piccola parte dei suoi epsi, solitamente meno dell'1%.
##### LoRa
Il Low-Rank Adaptation (LoRa) è la tecnica PEFT stato dell'arte.
L'idea è aggiungere delle matrici con rango $r$ e imparare i pesi solo si queste: se un layer lineare impara $h=Wx$, dove $W\in\mathbb{R}^{d\times d}$ sono i pesi del layer e $x\in\mathbb{R}^d$ è l'input, allora con LoRa tale layer diventa $h=(W+BA)x$, dove $A\in\mathbb{R}^{d\times r}$ e $B\in\mathbb{R}^{r\times d}$.
L'inizializzazione avviene usando inizializzando B a 0 e A campionando da una distribuzione gaussiana a media 0.
In questo modo durante l'inferenza il nuovo modello diventa $W+BA$: non ci sarà una perdita di prestazioni a livello di tempo perché si tratta di una semplice somma.
Nel caso in cui $r=d$ allora sto aggiornando tutti i parametri, cioè stiamo facendo [full fine tuning](LoRa.png); se invece $r<d$ (solitamente $r<<d$) sto aggiornando meno pesi.
##### Vantaggi
Questo ha dei grandi vantaggi:
- Il costo in termini di tempo è molto ridotto.
- Il nuovo modello può essere memorizzato salvando solo i nuovi pesi. Questo permette di eseguire lo switch dei modelli per ogni utente: si può caricare velocemente il modello specializzato per l'utente che lo sta usando molto velocemente.
- I checkpoint sono leggeri: si devono salvare solo i nuovi pesi.