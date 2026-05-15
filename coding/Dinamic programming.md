La programmazione dinamica (DP) è una tecnica per costruire algoritmi efficienti basandosi sul divide et impera. Si vogliono risolvere problemi complessi scomponendoli in sotto problemi più semplici; i risultati dei sotto problemi vengono memorizzati in modo risolverli una sola volta.
# Tipologie
Ci sono due tipologie:
- Top-down
	- Si scrive una funzione ricorsiva.
    - Si salvano i risultati intermedi in una tabella (tipicamente un dizionario o un array).
    - Se si incontra di nuovo lo stesso sottoproblema, si usa il risultato già calcolato.
- Bottom-up
    - Si risolvono prima i sottoproblemi più piccoli.
    - Si costruisce una tabella (array/matrice) con i risultati, usandola per arrivare alla soluzione del problema originale.
# Fibonacci
- Ricorsivamente
```
def fib(n):
	if n <= 1:
		return n
	return fib(n-1) + fib(n-2)
```
- Top-down
```
memo = {}
def fib(n):
    if n in memo:
        return memo[n]
    if n <= 1:
        memo[n] = n
    else:
        memo[n] = fib(n-1) + fib(n-2)
    return memo[n]
```
- Bottom-up
```
def fib(n):
    if n <= 1:
        return n
    dp = [0] * (n+1)
    dp[1] = 1
    for i in range(2, n+1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
```
