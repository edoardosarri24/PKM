L'injection è quel meccanismo del [CDI](Context%20and%20Depencency%20Injection%20(CDI).md) per cui non si deve implementare in una classe un oggetto di un dato tipo, ma questo viene passato al container CDI.
# Tipi
L'injection può essere specificata in tre modi tramite l'annotazione $@Inject$:
##### Field injection
Si mette l'annotazione sopra la dichiarazione del campo.
- È più semplice e richiede meno codice.
- Non è necessario definire il costruttore della classe.
- I campi non possono essere $final$ e quindi non si può garantire la mutabilità del campo.
##### Constructor injection
L'injection viene fatta implicitamente dal costruttore e quindi non è necessario usare l'annotazione $@Inject$. È la soluzione da preferire.
- Richiede più codice boilerplate.
- I campi possono essere dichiarati come final e quindi si può garantire l'immutabilità.
##### Initializer method
Il concetto è lo stesso del costruttore, ma l'inizializzazione dei campi è fatta da un metodo $init$.
In questo caso si deve annotare con $@Inject$ il metodo stesso.
# Flusso
Quando il CDI deve fare l'injection a run time segue dei passaggi:
- Controlla che il tipo da iniettare appartenga solo a un bean tramite i typeset. Se il controllo non passa allora la build fallisce.
- Si passa l'istanza già creata in quello scope o una nuova se non è mai stata creata.
- Si fanno eventuali injection ricorsive.
- Viene chiamato un eventuale metodo della classe del bean annotato con $@PostConstruct$ (e.g., per il logging). Quindi questo metodo viene chiamato ogni volta che viene creato un oggetto di quel bean, a seconda del contesto del bean; in questo modo inoltre il costruttore può essere omesso.
- Prima di distruggere il bean viene chiamato un eventuale metodo annotato con $@PreDestroy$ (e.g., per il logging o per rilasciare risorse).