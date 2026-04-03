Dati $n,k\in\mathbb{N}$ il coefficiente binomiale $n$ su $k$ è definito come il numero di sottoinsiemi di $k$ elementi scelti da un insieme di $n$ elementi, cioè il numero di sottoinsiemi di cardinalità $k$ di un insieme di cardinalità $n$.
# Calcolo
Si calcola come$\binom{n}{k}=\begin{cases} 0&\text{ se }n<k \\ \frac{n!}{k!(n-k)!}&\text{ se }k\ge n\end{cases}$.
# Proprietà
- $\binom{n}{0}=1$.
- $\binom{n}{n}=1$.
- $\binom{n}{1}=n$.
- $\binom{n}{n-1}=1$.
- $\binom{n}{k}=\binom{n}{n-k}$.
- $\binom{n}{k}=\binom{n-1}{k}+\binom{n-1}{k-1}$.