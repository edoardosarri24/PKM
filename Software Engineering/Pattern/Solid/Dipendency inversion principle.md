---
aliases:
  - DIP
---
I moduli di alto livello (La business logic, la parte che prende decisioni) non deve dipendere dai moduli di basso livello (Quelli che non caratterizzano l'applicazione, come la UI).
## EURISTICA
- Riferimenti solo ad astrazioni e non a classi concrete, a meno che quest'ultime siano non volatili.
- Struttura i codici a layer. I moduli di alto livello metteranno a disposizione interfacce per quelli inferiori.
- Non estendere classi concrete o o ridefinire metodi concreti.
- Dichiarare una variabile col tipo più astratto possibile.