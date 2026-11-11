Il sistema CORA (Correlation and Analysis), è una metrica usata per valutare la correlazione tra segnali definiti nel dominio del tempo (i.e., time series). È particolarmente influente nell'ambito dell'automotive e della biomeccanica per i crash test: viene usato per valutare la distanza tra le curve ottenute tramite simulazione FEM ed esperimenti empirici.
# Struttura del modello
La metrica definisce il suo punteggio combinando due metriche.
##### Corridoio
Valuta quanto la curve simulata rientra all'interno di [bande di confidenza](bande%20di%20confidenza.md) della curva sperimentale. Riesce quindi a catturare la distanza delle forme geometriche delle due curve.
##### Cross correlazione
Calcola tre sotto indicatori:
- Fase
	Valuta anticipi e ritardi tra le due curve.
- Area
	Valuta l'area totale sotto le curve. Il problema, ad esmepio rispetto a [ISO TS 18571](ISO%20TS%2018571.md), è che due curve molto diversi possono avere esattamente lo stesso integrale.
- Forma
	Utilizza la tecnica della cross correlazione normalizzata per capire se le due curve hanno la stessa forma.