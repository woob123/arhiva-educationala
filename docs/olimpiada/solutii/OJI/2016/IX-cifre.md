---
tags:
    - OJI
    - clasa IX
---

# Soluția problemei cifre (OJI 2016, clasa a IX-a)

!!! example "Cunoștințe necesare"
    - [Prelucrarea cifrelor](../../../../usor/digits-manipulation.md)
    - [Operatori și expresii. Cunoștințe matematice de bază](../../../../cppintro/basic-math.md)
    - [Maxime și minime](../../../../usor/digits-manipulation.md)

**Autor soluție**: Ștefan-Cosmin Dăscălescu

!!! note "Link problemă"
    Această problemă poate fi accesată [aici](https://kilonova.ro/problems/864/). 

De-a lungul acestei abordări, va fi foarte important să ne gândim la folosirea
cifrelor pentru a codifica numărul de poziții pe care s-a plasat un anumit
segment. Deoarece ni se dau segmentele, putem deja să codificăm cifrele care
sunt incluse una în alta, sau pur și simplu să numărăm într-o listă câte cifre
mai mari sunt incluse în alta.

Pentru prima cerință, nu trebuie decât să numărăm segmentele care apar în numere.

Pentru cea de-a doua cerință, ne putem gândi să fixăm cifra care va determina
gradul de comparare între numere, iar pentru acea cifră să aflăm la câte cifre
mai mari putem ajunge. Ulterior, pentru toate celelalte cifre vom afla câte
numere pot fi obținute, indiferent că sunt mai mari sau mai mici decât ea.

Pentru a afla formula specifică fiecărei poziții, ne putem gândi la regula
produsului, deoarece știm câte variante avem pentru fiecare poziție și le putem
înmulți pentru a afla contribuția la răspuns a poziției curente.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
--8<-- "olimpiada/IX/2016-cifre.cpp"
```