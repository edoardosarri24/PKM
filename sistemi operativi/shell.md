La shell in un [OS](Operating%20System%20(OS).md) è l'interprete dei comandi, cioè il software con cui l'utente può dare istruzioni al kernel tramite comandi oppure avviare programmi.
# Shebang
Uo script shell inizia sempre con lo Shebang, cioè con il comando che indica quale interprete di shell utilizzare.
Solitamente si usa $\texttt{\#!/bin/bash}$: quello che succede è che il kernel quando legge lo script lo passa come argomento al programma nel [file system](file%20system.md#bin) $\textit{/bin/bash}$.
##### Sh
È la shell più antica, ma a oggi è molto limitata.
##### Bash
La shell bash è lo standard moderno presente in tutti i kernel che si rispettino.
# Estensine
L'estensione in uno scritp shell potrebbe anche essere omesso: solitamente si usa $\texttt{.sh}$ per identificarlo subito e per una grande compatibilità.
Se vogliamo invece rendere lo script un programma nativo dell'os, dovremmo omettere l'estensione e spostare il programma in $\textit{/bin/bash}$ in modo che sia utilizzabile come un comando di sistema (e.g., come $\texttt{ls}$).
Se viene omesso allora lo script sarà l'input del porgramma di deafult.
# Variabili
Gli script di shell sono debolmente tipizzati e di solito tutto è visto che stringhe.
Le variabili sono indicate con le maiuscole (e.g., $\texttt{NOME="Valore"}$) e si fa riferimento al loro valore con $\texttt{\$NOME}$.
# Struttura condizionale
La struttura condizionale più semplice (i.e., if-else) ha [questa sintassi](if-else%20shell%20script.png):
Solitamente oggi si predilige le doppie parantesi quadre che permettono di omettere le virgolette alla variabile e di utilizzare i connettivi logici AND e OR.
##### Test sui file
dentro le parentesi possiamo testare direttamente il file system:
- $\texttt{-f filename}$
	Vero se il file esiste ed è regolare.
- $\texttt{-e filename}$
	Vero se il file esiste (di qualsiasi tipo).
- $\texttt{-d dirname}$
	Vero se la directory esiste.
##### Confronta tra numeri
Possiamo usare $\texttt{-qualcosa}$ per confrontare due numeri, dove qualcosa è simile alla sintassi Latex.
##### Cortocircuito
Usare $\texttt{||}$ è una condizione di errore per cui la seconda parte del comando viene eseguita solo se la prima fallisce, cioè se il suo codice di errore è diverso da 0.
# Argomenti
Sono identificati con numeri che seguono $\$$: come in [C](C.md) l'argomento 0 è il nome dello script.
# Output
Per ridirezionare l'output possiamo:
- $\texttt{>}$
	Per sovrascrivere da qualche parte.
- $\texttt{>>}$
	Per appendere da qualche parte.
- $\texttt{2>}$
	Per mandare da qualche parte gli errori.
- $\texttt{\&>}$
	Per mandare da qualche parte sia l'output che gli errori.