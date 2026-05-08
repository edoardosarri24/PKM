Quando si costruisce un [container](container.md) si tende a non inserire all'interno della sua [immagine](immagine.md) i dati che l'applicazione necessita per eseguire, ma questi devono essere importati in un secondo momento.
# Politiche di import
Per importare i dati abbiamo due tecniche:
- Image
	Se sono pochi, allora possono essere inserire nell'immagine. È comunque una cosa che si tende a non fare perché rendono un container molto più pesante e per porblemi di privacy.
- Montare
	Possiamo istruire l'utente che scarica e lancia il container a mantarci un volume con i propri dati.
- Download
	Il nostro container può avere uno script, che verrà eseguito quando lo lanceremo che scarica i dati. Uno script di questo tipo dovrebbe essere inserito nel $\texttt{main}$ del codice oppure nel [dockerFile](dockerFile.md) con il comando [Entrypoint](dockerFile.md#Entrypoint).