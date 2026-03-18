[Micrometer](https://micrometer.io) è un framework per la raccolta di metriche molto utilizzata in applicazione a [microservizi](microservizi.md).
È lo standard utilizzato in [quarkus](quarkus.md) per la raccolta delle metriche: Quarkus fornisce anche l'implementazione della specifica [metrics](metrics.md) di [MicroProfile](MicroProfile.md), ma usare MicroMeter permette di avere più vantaggi.
# Tipi
In MicroMeter ci sono più tipi di metriche:
- Single value
	È associato un solo valore per ogni metrica.
	- Counter
		Si tratta di un semplice contatore che può essere solo incrementato.
		È una metrica multidimensionale, le cui dimensioni sono $class$, $method$, $exception$ e $result$.
	- Gauge
		Si tratta di un contatore che può essere incrementato e decrementato.
- Multiple value
	A ogni metrica sono associati più valori. Per default questi sono il massimo, il minimo e la somma dei valori.
	- Timer
		Misura il tempo di completamento di una certa operazione.
	- Distribution
		Restitusice un istogramma e non è correlato una misura temporale.
# Dimensionalità
Una metrica non è definita da un nome e un singolo, ma può avere più dimensionalità. Una dimensione viene aggiunta quando alla metrica si unisce un coppia chiave-valore detta tags. Ogni metrica è unica rispetto all'insieme del nome con i tags.
##### Utilità
Usando bene le dimensionalità si possono valutare metriche composte: ci possiamo far restituire tutti i contatori che sono legati a uno stesso tag.
# Programmare metriche
Per esporre una metrica ci sono due modi:
- Dichiarativo
	Un metodo viene annotato con un'annotazione presente in $io.micrometer.core.annotation$. Si può specificare un nome e una descrizione, oltre ad altri parametri.
	È meno flessibile e potente ma più facile e meno verboso.
- Imperativo
	Si dichiara una variabile di tipo $MaterRegister$ e la si annota con $@Inject$. Questo oggetto rappresenta il registro di tutte le variabili dell'applicazione. Su questo metodo poisi fanno tutte le chiamate che ci servono.
	E più potente ma più complesso.
# Quarkus
Per utilizzare MicroMeter con [prometheus](prometheus.md)  dobbiamo fare una semplice cosa: agigungere l'estensione $quarkus-micrometer-registry-prometheus$.
In questo modo stiamo fornendo l'integrazione con MicroMiten nel formato che richide Promethues.