Un LLM è un [language model](language%20model.md) molto grande, cioè  è una rete con un gran numero di parametri.
- [determinismo](https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/?utm_source=tldrai%5D(https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/?utm_source=tldrai) (non da sapere è roba mia)
# Open
In base a come le aziende gestiscono i rilasci dei modelli abbiamo:
- Open
	Sono modelli che possiamo eseguire in locale. Si dividono in:
	- Open weights
		Abbiamo solo i pesi del modello e non sappiamo in nessun modo come è costituito il training set.
	- Open source
		Abbiamo a disposizione sia il training set che i pesi del modello.
- Close
	Sono modelli proprietari (e.g., [GPT](GPT.md), Gemini) e per questo non abbiamo nessuna informazione né sul training set né sui pesi.
	Per avere una minima idea possiamo guardare quella che è chiamata model card presente su hugging face, ma spesso dice veramente poco.
	Possono essere usati tramite un'interfaccia web oppure trami API (i.e., in Python); solitamente se usiamo questi modelli poco allora sono gratis, altrimenti dobbiamo pagare a seconda token (in ingresso e in uscita) che usiamo.
# Dimensione
Come ci insegna la [storia di GPT](GPT.md#Storia), le prestazioni degli LLM sono molto influenzate dalle loro dimensioni: l'aumento delle prestazioni cresce più che linearmente rispetto alle dimensioni del modello, ma a un certo punto crescono in modo esponenziale.
##### Emergent abilities
Ci sono degli studi che mostrano come aumentando la dimensione del modello si ha anche quella che è nota come emergent abilities: non solo si migliorano le prestazioni dei vecchi task, ma è possibile anche svolgerne di nuovi. L'[in-context leaning](prompting.md#In-context%20leaning), soprattutto con il zero-shot learning, è un esempio di quello che possiamo fare solo se il modello è abbastanza grande.
##### Quantizzazione
La dimensione dei modelli è però anche un problema per la loro esecuzione: solitamente i pesi sono memorizzati in $float16$ o $float32$ (2 o 4 Byte); per eseguire un modello da $xB$ (miliardi) di parametri servono almeno $2xGB$ (o $4xGB$ se si usa $float32$) di gpu; se non si ha una gpu grande allora non si può eseguire il modello.
Per evitare questo problema si usa quantizzazione, cioè si rappresentano i valori come $int8$. I vantaggi sono: minor consumo di memoria, inferenza più veloce e minor consumo di energia.
# Utilizzo
Ci sono in pratica tre metodi con cui usare gli LLM:
- [prompting](prompting.md)
- [fine tuning](fine%20tuning.md)
- [RAG](RAG.md)