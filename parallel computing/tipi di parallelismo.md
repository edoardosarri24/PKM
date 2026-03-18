Oggi le CPU e le GPU sono pensate per esereguire calcoli in [parallelo](parallel%20pragramming.md). Ci sono più tipi di parallelismo che vanno in base al livello di astrazione che si usa.
# Bit level
Una forma di paralismo è aumentare la capacità del processore. Nella storia sia partiti da architetture a 8bit, mentre oggi siamo a 64bit. Non si pensa che arriveremo a 128bit.
Se abbiamo un'architettura a 64bit vuol dire che:
- I registri contengono al più 64bit.
- Il processore scrive e legge dalla memoria al più 64bit.
# Instruction level
Quando un'istruzione viene caricati in memoria essa è composta da 5 (nella realtà di più) fasi: fetch, decode, execution, memory, writeback. Queste operazioni sono indipendenti e operano con parti della ALU diverse; per questo motivo vengono eseguite in paralellelo.
A seconda del modo in cui l'OS decide di iniziare il ciclo di un'istruzione cambiano le performance: possono essere eseguite nell'ordine in cui arrivano; eseguite in un qualunque ordine che non violi le [dipendenze](dipendenze.md); usare algoritmi speculativi che permettono di supporre che le istruzioni dentro un ciclo debbano continuate a essere eseguite sempre.
# Data level
È un livello di astrazione, noto come SIMD (Signel Instruction, Multiple Data) che lavora sui dati per eseguire la stessa operazione su un batch invece che su un singolo dato alla volta.
È un'operazione molto usata in sistemi multimediali.
# Memory level
In un sistema parallelo ci sono due modi in cui le varie CPU possono comunicare, cioè due modi in cui può avvenire l'ICP (Inter Process Comunication).
##### Shared memory
Ogni CPU ha la sua cache, ma la memoria centrale è unica. Quando due processi devono comunicare condividono la stessa area di memoria.
Sono più semplici ma non scalano dopo un certo numero di CPU. Inoltre è più facile introdurre bug (e.g., race condition) e sono sistemi più difficili da debuggare.
##### Distributed memory
Ogni CPU ha la sua memoria principale. Quando due processi devono comunicare lo fanno tramite una rete di qualche tipo.
Sono più semplici da debuggare perché le interazioni tra processi sono ben definite. Sono la soluzione per creare sistemi fault-tollerant.
##### Heterogeneous
Sono sistemi basati sia su shared memory che su distributed memory.