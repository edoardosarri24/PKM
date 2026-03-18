Si vuole dare la possibilità di creare istanze della classe solo con metodi statici senza usare l'istruzione $new$.
## SOLUZIONE
- Si rende privato il costruttore.
- Si creano metodi statici pubblici che richiamano il costruttore.
## CONSEGUENZE
- Se si vuole un nuovo modo di creare oggetti si può creare un nuovo metodo statico: non si rompe nulla e non si hanno metodi in overloading.
- Si migliora la leggibilità.
- Si possono riusare istanze (Come si fa per il [[Singleton]]) in più modi.