- [Ottimizzazione non vincolata](Ottimizzazione%20non%20vincolata.md)
- [Ottimizzazione vincolata](Ottimizzazione%20vincolata.md)
---
Lo scopo è trovare $x^*=\displaystyle\min_{x\in S}f(x)$.
- $f$ è detta funzione obbiettivo. Se il problema è non differenziabile abbiamo solo $f$; se il problema è differenziabile abbiamo anche $\nabla f$ e $\nabla^2 f$.
- $S$ è l'insieme ammissibile. Nel caso [non vincolato](Ottimizzazione%20non%20vincolata.md) $S=\mathcal{R}^n$; nel caso [vincolato](Ottimizzazione%20vincolata.md) $S\subset\mathcal{R}^n$.
---
# Compatti
Sia $f:S\subseteq\mathcal{R^n}\to\mathcal{R}$.
- Weiestrass
	Se $f$ è continua con $S$ [compatto](Ottimizzazione.md#Definizioni#Insieme%20compatto) $\Rightarrow$ $f$ ammette minimo globale.
- Se $f$ ha un [insieme di livello](Ottimizzazione.md#Insieme%20di%20livello) compatto $\Rightarrow$ $f$ ammette minimo globale.
- $f$ [coerciva](Ottimizzazione.md#Funzione%20coerciva) $\Leftrightarrow$ Ha tutti gli insieme di livello compatti.
	Da questa si ha $f$ coerciva $\Rightarrow$ $f$ ammette minimo globale.
# Convessi
Sia $f:S\subseteq\mathcal{R^n}\to\mathcal{R}$.
- $f$ [convessa](Definizioni.md#Funzione%20convessa). Se $x^*$ è minimo locale $\Rightarrow x^*$ è minimo globale.
- $f$ [strettamente convessa](Definizioni.md#Funzione%20strettamente%20convessa). Se $\exists x^*$ minimo locale $\Rightarrow x^*$ è unico.
- Se f [fortemente convessa](Definizioni.md#Funzione%20fortemente%20convessa) $\Rightarrow f$ strettamente convessa e coerciva
	Da questa si ha $f$ fortemente convessa $\Rightarrow$ $\exists$ minimo globale unico.
# Quadratici
$f$ [quadratica](Definizioni.md#Funzione%20quadratica).
- $\exists x^*$ minimo globale $\Leftrightarrow Q\succeq0$ e $\exists\bar{x}|Q\bar{X}+c=0$.
- $x^*$ minimo blogale unico $\Leftrightarrow$ $Q\succ 0$.
# Direzioni
- Se $x^*$ minimo locale$\Rightarrow\nexists d\in\mathcal{R^n}$ direzione di discesa.
- Se $x^*$ minimo locale$\Rightarrow\nabla f(x^*)=0$.
	- $f$ [convessa](Definizioni.md#Funzione%20convessa). $x^*$ minimo locale$\Leftrightarrow\nabla f(x^*)=0$.
- Se $x^*$ minimo locale$\Rightarrow\begin{cases}\nabla f(x^*)=0\\$\nexists d\in\mathcal{R^n}\text{ direzione di curvatura negativa}\end{cases}$.
- Se $x^*$ minimo locale $\Rightarrow\begin{cases}\nabla f(x^*)=0\\\nabla^2f(x^*)\succeq0\end{cases}$.
- Se $\nabla f(x^*)=0$ e vale una di $\begin{cases}\nabla^2f(x^*)\succeq0\\\end{cases}$.