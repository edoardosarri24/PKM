# Invocazione dal front end
Quando un front end deve chiamare dei servizi deve sapere quale chiamare. Ci sono vari approcci a questo:
- API gateway
	Si definisce un unico elemento con cui il front end comunica e che chiama i micro servizi del back end.
	Il vantaggio è che è possibile ottimizzare le chiamate ai vari micro servizi.
	Lo svantaggio è che è un single point of failure.
- Backend for frontend (BFF)
	Ogni front end ha la propria interfaccia verso il back end.
	Semplifica la gestione di categorie di front end.
# Invocazione tra servizi
Spesso una funzionalità viene fornita tramite più chiamate a più micro servizi. Per gestire l'ordine di queste chiamate ci sono più metodi:
- Orchestatore
	Si tratta di un approccio centralizzato: un componente conosce per ogni servizio offerto dal sistema l'ordine delle chiamate da fare ai vari micro servizi. Solitamente viene combinato con l'API gateway.
	Il grande vantaggio è che i micro servizi non sono minimamente accoppiato e non devono conoscersi.
	Diventa un SPF.
- Coreografia
	Quando un micro servizio riceve una richeista sa quale altro componente chiamare.
	C'è più accoppiamento tra i vari micro servizi e questo peggiora in manutenibilità ed evoluzione del sistema.
	Non abbiamo un SPF e abbiamo un'architettura più semplice.