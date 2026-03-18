È un task di [NLP](NLP.md).
L'input è un documento di cui non abbiamo la rappresentazione originale, ma una scansione o foto; l'output sono le informazioni che il documento contiene.
# Input
L'input è:
- Composto da segni grafici su un qualche supporto. Solitamente i segni sono inchiostro e il supporto è la carta.
- Rappresentato in una qualche lingua grafica. Solitamente questa lingua è composta da caratteri che compongono le parole.
##### Struttura
La struttura dell'input può essere:
- Fisica
	Sono gli oggetti di cui è composta, come ad esempio testo, e la loro posizione assoluta.
- Logica
	È la relazione tra gli oggetti, come ad esempio la posizione relativa tra due oggetti o il linking tra testo e figura.
##### Grafica
Un input può:
- Contenere solo testo.
- La grafica è poca e si riassume ad esempio in loghi.
- La grafica è abbondante e rilevante, come ad esempio le tabelle.
- La grafica è la parte principale.
##### Qualità
La qualità di un input dipende da molti fattori: difetti nel supporto (i.e., carta), difetti introdotti durante la stampa del documento o durante la sua digitalizzazione.
La qualità di un documento in input si misura secondo tre attributi:
- Decifrabilità: la capacità di leggere correttamente una lettera.
- Legibility: la capacità di leggere correttamente una parola del documento.
- Readability: la capacità di capire correttamente il significato di una frase.
# Task
- Sorting delle mail
- Data mining di una collezione di documenti vecchi.
- Analisi di disegni o tabelle.
- Degradazione
	Spesso è utile degradare la qualità di un documento in modo da poter creare un dataset etichettato per addestrare un qualche modello con l'idea di avere un maggior numero di immagini.
- [layout analysis](layout%20analysis.md)
# Soluzioni
Ci sono due soluzioni di categorie per risolvere i task della docoment analysis.
- Bottom-up
	Prima si riconoscono i caratteri e poi unendo informazioni piccole si arriva all'informazione totale.
- Top-down
	Si parte dalla pagina, la si divide in unità e poi si riconoscono queste entità singolarmente.
##### Pipeline
Solitamente si seguono degli step:
- [pre-processing](document%20pre-processing.md)
	Solitamente servono per migliorare la qualità delle immagini.
- Segmentation
- Features extraction
- Object recognition
	Si classificano gli oggetti delimitati dalla fase precedente.
- Post processing