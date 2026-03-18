Fornisce un'interfaccia unificata e semplice per un sottosistema complesso.
## APPLICABILITÁ
- Al client non interessa la personalizzazione data dall'uso diretto del sottosistema.
- Si vuole creare un sistema a strati e far interagire strati adiacenti tramite interfacce.
## STRUTTURA
![[Facade.png]]
- $Facade$
	Classe o interfaccia che usa il sottosistema ed è usata dai client.
- $Sottosistema$
	Classi e interfacce che non sono visibili al client, ma utilizzate tramite la facciata.
## CONSEGUENZE
- Se il sottosistema fa delle modifiche per cui il nostro codice dovrebbe cambiare ogni parte in cui lo usa, in questo modo si deve cambiare solo la facciata.