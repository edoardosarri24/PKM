Si vuole separare la costruzione di un oggetto dalla sua rappresentazione.
- Creazionale.
## SOLUZIONE
- Il $Builder$ è una static inner class chiamata $Builder$ o $xxxBuilder$ (Con $xxx$ nome della classe da istanziare) con gli stessi campi della classe da istanziare.
- La classe da istanziare ha costruttore e setter privati. Il $Builder$ ha un costruttore pubblico (O privato con uno [[Static factory method]] che lo richiama)) che prende i campi richiesti e dei setter pubblici (convenzionalmente chiamati $with$ o $withxxx$) basati su fluent interface per quelli facoltativi.
- All'interno del costruttore e dei setter si possono validare i campi.
- Il Builder ha un metodo $built$ per costruire effettivamente l'oggetto.
## CONSEGUENZE
- Una volta creato l'oggetto non si può invalidare perchè costruttore e setter sono privati.
- Creare un'oggetto complesso con un $Builder$ fa è più facile e sicuro.