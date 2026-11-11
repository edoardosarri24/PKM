La compilazione compila diversi file sorgente e li collega, eventualmente con altre librerie esterne, tramite il linker in un unico eseguibile.
- Viene usato il comando $cc$ (alias per gcc o clang).La forma è $cc\ file_1.c\cdots\ file_n.c$
- Questo produce tanti file oggetto $file_i.o$ e li carica un un unico eseguibile $a.out$.
- Se vogliamo ricompilare solo un file lo si può inserire nel comando $cc$ come $file.c$ insieme agli altri in formato $file.o$.
# Preprocessore
È la prima parte della compilazione. Si occupa principalmente di tre parti:
- Include
	Il suo compito è includere interamente il file che viene passato come parametro.
	Ci sono due modi per specificare il file:
	- $\#include\ <file>$ cerca il file nelle librerie installate o nella [standard library](standard%20library.md).
	- $\#include\ "file"$ inizia a cercare il file a partire dalla directory corrente e se non lo trova lo cerca come farebbe l'altro comando.
- Define
	Sono valori di stringhe costanti e hard-coded, chiamate macro, che vengono brutalmente sostituiti dove c'è necessità. Possono prendere anche parametri, su cui non esiste il tipo (visto appunto che sono sostituito brutalmente).
	Lo scope delle macro è nel file in cui esistono.
##### Header guards
In un file sorgente $.c$ le dichiarazioni devono essere fatte una sola volta altrimenti si hanno errori di compilazione.
Solitamente queste si fanno tramite inclusione dei file $.h$. Per evitare inclusioni annidate anche involontarie (io includo $A.h$ e $B.h$, ma A.h ha già incluso $B.h$) si usano le header guards: in ogni header si aggiunge un controllo (i.e., $\#ifndef$) su una macro (solitamente che ha lo stesso nome dell'header) e all'interno di questo if si definisce la macro controllata e si include ciò che deve essere incluso. Questo funziona perché con $inlude$ si trascrive semplicemente il contenuto di un header e le macro hanno uno scope del file in cui sono dichiarate; se non è la prima volta che si include l'header allora non si entra dentro l'if e non si scrive l'header più volte.
# Pragma
Le pragma iscruction sono delle direttive che diamo al compilatore per istruirlo su come deve essere compilato l'eseguibile.
Per garantire la portabilità del codice, se il compilatore supporta una determinata pragma allora la usa, altrimenti la ignora sollevando uno warning.
La forma delle Pragma è $\#pragma\ nome\_direttiva\ [argomenti]$. Si può splittare su più linee usando il \\.
# Ottimizzazione
In fase di compilazione possiamo forzare il compilatore a eseguire le ottimizzazione di parallelizzazione.
Questo si aggiungendo il comando $-Ox$, dove $x=1,2,3$ a seconda del livello di ottimizzazione che si vuole.