Per implementare un [backend](backend.md) ci sono diversi linguaggi tra cui possiamo scegliere.
# Python
- È il migliore per facilità e rapidità di sviluppo.
- Permette di integrare nel nostro backend chiamate a [LLM](LLM.md) con molta facilità.
- Le prestazioni sono inferiorii essendo un linguaggio interpretato.
##### Framework
I più comuni sono Django, FastAPI e Flask.
# Go
- Le performance sono paragonate al [C++](C++.md), ma è molto più semplice e gestisce nativamente la concorrenza.
- Non ha interfacce di altissimo livello.
- Consuma pochissima memoria, come C e C++.
##### Framework
Il più nodo è Gin.
# [node](node.md)
- Permette di utilizzare lo stesso linguaggio (i.e., [javascript](javascript.md)) sia nel frontend che nel beckend.
- Ottimo per l'I/O asincrono, cioè se il thread si deve occupare solo di chiamare database o altri servizi.
- È a singolo thread e quindi pessimo se il più del lavoro deve essere fatto direttamente dal backend.
- Consuma poca memoria, più di Go ma molta meno di Python e Java.
##### Framework
I più comuni sono Express e NestJS.