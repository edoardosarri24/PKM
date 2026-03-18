È la tecnica base per utilizzare un [LLM](LLM.md). Stiamo in pratica prendendo il modello pre addestrato, detto foundation model, e usandolo senza specializzarlo per un qualche compito.
##### Limiti
Questa tecnica ha dei limiti:
- Ci sono compiti troppo complessi perché vengano risolti con il solo prompting (è necessario eseguire un [fine tuning](fine%20tuning.md)).
- A volte il task richiede un ragionamento molto ricco e un LLM troppo generalista non può risolvere se usato con il solo prompting.
##### In-context leaning
Usando il modello solo tramite prompting facciamo quello che si chiama in-context learning: diamo un contesto al modello che condizionerà la generazione dell'output; è detto learning anche se non stiamo cambiando i pesi perché stiamo sollecitando alcuni parametri più di altri.
A seconda di quanti esempi forniamo si parla di zero-shot (cosa possibile e prestazioni buone solo con una [dimensione](LLM.md#Emergent%20abilities) elevata del modello), one-shot o few-shot learning.
# Step-back promping
Con la tecnica step-back prompting si ottiene l'output finale attraverso più interazioni di prompt/risposta con l'LLM: le informazioni che il modello ci fornisce in una risposta sono messe nel successivo prompt.
Questo alza le prestazioni perché tramite il prompt successivo stiamo condizionando la risposta dell'LLM su qualcosa che il modello conosce bene perché ce lo ha ritornato in precedenza.
Un'evoluzione di questa tecnica è avere una conversazione per farsi generale il prompt che poi usere per uno specifico task.
# Chain-of-Thought
La CoT è una tecnica molto potente che spinge il modello a ragionare e a risolvere task molto specifici.
Per forzare questo comportamento possiamo eseguire un one-shot (o few-shot) learning dove l'esempio contiene il ragionamento per la soluzione. In alternativa se non abbiamo l'esempio possiamo semplicemente chiedere al modello di ragionare step by step (i.e., let’s think step by step).
# Self-consistency
È una tecnica che si basa sul [non determinismo](generazione.md) degli LLM: usiamo lo stesso prompt più volte e scegliamo la risposta secondo una votazione a maggioranza.
Solitamente si usa un prompt CoT e una temperatura non troppo alta; in questo modo si aumenta la varianza e si ottengono risposte diverse.