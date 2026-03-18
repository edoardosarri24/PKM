La distribuzione binomiale è la distribuzione base che modella la [random variable](random%20variable.md) che conta quante volte un evento "successo" si verifica in numero fissato di tentativi indipendenti contro un evento (o un insieme di eventi) "fallimento".
# Formarmente
La probabiltà che si verifichino $k$ successi dati $n$ tentivi è data da $P(X=k)=\binom{n}{k}p^k(1−p)^{n−k}$, dove $p$ è la probabilità di successo di ogni evento.
Notiamo che $p$ deve essere la stessa per ogni esperimento.
- Valore atteso
	È esattamente $n\times p$.
- Varianza
	Si calcola come $n\times p\times(1-p)$.
# Osservazioni
- Quando il numero $n$ di tentativi tende a infinito, allora la distribuzione binomiale tende alla [distribuzione gaussiana](distribuzione%20gaussiana.md).
- Per $p=0.5$ la distribuzione è [perfettamente simmetrica](distribuzione%20binomiale.png). Più $p\to0$ e $p\to1$ più la distribuzione diventa asimmetrica.