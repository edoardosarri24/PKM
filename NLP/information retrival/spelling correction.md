Quando si parla di spelling correction ci sono due principali usi: suggerire una query all'utente a partire da una query parziale inserita (risolta con gli [LLM](LLM.md)); correggere gli errori di battitura in un documenti (e.g., mentre si scrive su word).
I possibili approcci sono: considerare solo la sintassi della parola; considerare anche la sua semantica (quello che fanno gli [LLM](LLM.md)).
# Correzione sintattica
Nella correzione sintattica si correggono errori di battitura.
L'idea è:
- Abbiamo una lista con tutte le parole corrette del dizionario standard. Questo si può fare usando un vocabolario o un vocabolario di un settore specifico.
- Quando inserisco una parola nella query (o nel documento) se non la trova tra le parole corrette allora cerco la parola corretta più vicina secondo una qualche distanza (e.g., [Edit](Distanza.md#Edit)).
##### Trovare l'errore
Il problema principale è che non sappiamo dove si trova l'errore; dovremmo quindi controllare ogni parola in input con tutte le parole corrette, e questo non è possibile perché troppo costoso.
Per risolvere questo problema si usano i $k$-gram: non si calcola la distanza tra la parola inserita e tutte quelle del vocabolario, ma solo con quelle con cui la parola inserita ha più di $x$ $k$-gram in comune (solitamente $k=2$ al massimo $K=3$).
Per vedere se la parola in input ha $x$ $k$-gram in comune con quelle appartenenti al dizionario si considera ogni parola un insieme di $k$-gram e si computa la distanza di [Jaccard](Distanza.md#Jaccard).