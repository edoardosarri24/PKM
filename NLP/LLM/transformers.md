I transformers sono lo stato dell'arte per gli [LLM](LLM.md). Rispetto alle [RNN](RNN.md) hanno due grandi vantaggi: riescono a catturare maggiormente le lunghe dipendenze, riducendo il problema della [memoria](RNN.md#Memoria), e possono essere addestrati in [parallelo](RNN.md#Sequenzialità).
# Blocco
![transformers block](transformers%20block.png)
Il blocco principale dei transformens è quello mostrato in figura:
- Un [attention](attention.md) layer che può essere self-attention o multihead attention.
- Delle [residual connection](ResNet.md#Residual%20block) (da sapere il collegamento).
- Un layer di [normalizzazione](Tecniche%20CNN.md#Normaliation%20layer) (da sapere il collegamento).
- Un layer denso (i.e., feedforward) per ottenere l'output.
# Econder-decoder
A partire dal blocco elementare si formano poi le architetture basate sui transformers. In generale queste possono essere solo encoder (e.g., [BERT](BERT.md)), solo decoder (e.g., [GPT](GPT.md)) o [encoder-decoder](encoder-decoder.md).