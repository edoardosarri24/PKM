È la classe che implementa le stringhe nella [STL](STL.md).
# View
Sono oggetti leggerissimi (16 Byte) che permettono di passare una stringa senza allorare memoria heap.
Si usano soprattutto per costanti.
Contengono solo:
- Puntatore all'inizio di una stringa esistente altrove. 
- Dimensione che dice quanti caratteri leggere.