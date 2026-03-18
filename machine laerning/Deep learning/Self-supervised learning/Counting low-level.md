È una tecnica usata nel [Self supervised learning](Self%20supervised%20learning.md) per far apprendere alla rete i dettagli semantici usati poi per scopi generici.
Se il nostro obiettivo è imparare la semantica di un'immagine, possiamo predisporre il proxy task in modo che confronti il conteggio di un dato elemento in un'immagine con quello dello stesso elemento ma in più pezzi della stessa immagine.
# Proxy task
![esempio counting low-level](esempio%20counting%20low-level.png)
L'architettura usata in questo contesto è quella della rete siamese. È stata usata una sotto rete per ogni pezzo dell'immagine originale, più una per l'immagine intera, più una per l'immagine diversa (vedi [cheating](Counting%20low-level.md#Cheating)).
# Cheating
Per assicurarci che la rete apprenda dettagli semanticamente importanti si confronta la somma dell'output dei pezzi dell'immagine con l'output di un'immagine molto diversa.
Se minimizziamo solo la distanza tra l'output dell'immagine intera e la somma di quelli dell'immagine divisa, cioè se la loss è $L=|d-t|^2$ allora la rete potrebbe porre sempre $c=t$ e risolvere il compito.
Usando un'ulteriore immagine, definendo un valore $M$ e aggiungendo alla loss il termine $max\{0,M-|c-t|^2\}$ allora se $c=t$ la loss non è minima. La rete dovrà quindi trovare un compromesso e questo la spinge a imparare dettagli semantici.