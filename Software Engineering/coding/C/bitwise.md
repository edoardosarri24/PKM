Gli operatori bitwise modificano singoli bit: il dato non è più letto interamente in byte ma bit a bit.
# Usi
Gli usi principali sono:
- Modificare bit con velocità di registro.
- Memorizzare 8 booleani in 1 byte. Di solito un $\texttt{boolean}$ occupa 1 byte.
- Leggere e aggiornare flag di stato.
# Operatori
- &
	Corrisponde all'operatore logico AND: vale 1 se entrambi gli operandi sono veri.
- |
	Corrisponde all'operatore logico OR: vale 1 se uno dei due operandi è vero.
- ^
	Corrisponde all'operatore logico XOR: vale 1 se i due operandi soo discordi.
- ~
	Corrisponde all'operatore logico NOT: inverte l'operando.
- << e >>
	Sono gli shift: spostano a destra e sinistra l'operando; nel caso di spostamento a sinistra si riempie con 0.
# Operazioni
Per le operazioni fondamentali si considera il $k$-esimo bit da destra (LSB - Less Significant Bit) partendo da 0.
La maschera di base di ottiene con $(1\ll k)$.
##### Impostare a 1
Dato input si imposta a 1 il $k$-esimo bit con $\texttt{input}|=(1\ll k)$.
##### Impostare a 0
Dato input si imposta a 0 il $k$-esimo bit con $\texttt{input}\&=$~$(1\ll k)$.
##### Invertire
Dato input si inverte il $k$-esimo bit con $\texttt{input}$^$=(1\ll k)$.
##### Testare
Dato input si testa il $k$-esimo bit con $(\texttt{input}\gg k)\&1$.