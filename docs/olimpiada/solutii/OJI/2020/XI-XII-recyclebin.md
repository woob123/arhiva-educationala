---
id: OJI-2020-XI-XII-recyclebin
author:
    - Ștefan-Cosmin Dăscălescu
prerequisites:
    - bitmask-dp
    - sequences
tags:
    - OJI
    - clasa XI-XII
    - secvente
    - programare dinamica
---
# Soluția problemei recyclebin (OJI 2020, clasele XI-XII)

!!! note "Link problemă"
    Această problemă poate fi accesată [aici](https://kilonova.ro/problems/19/).

Această problemă poate fi reformulată în felul următor: Se dă un șir de lungime
$n$ și se scot secvențe de lungimi puteri ale lui 2, cu condiția ca lungimile
secvențelor să fie distincte. Să se afle subsecvența de sumă maximă pe care o
putem obține după efectuarea unui set de operații optim.

După această reformulare, ne putem gândi mai întâi la o abordare de tip greedy
în care am putea alege într-un mod sau altul secvențele pe care să le scoatem,
iar mai apoi să rulăm algoritmul pentru subsecvența de sumă maximă pentru șirul
rămas.

Din păcate, avem mult prea multe cazuri din care putem alege, așa că este mai
bine să ne gândim la o idee bazată pe o dinamică. Aici putem recurge la
restricția menționată în enunt, prin care suntem constrânși să nu folosim aceeași
lungime de două ori, iar dat fiind că lungimile sunt puteri ale lui 2, ne
putem gândi la o dinamică pe măști pe biți, în care să avem $dp[i][msk]$
ca fiind suma maximă a unui șir care se termină la poziția $i$ și de unde
am scos deja secvențe cu suma lungimilor $msk$. 

Dintr-o asemenea stare,
putem fie să avansăm la poziția următoare, actualizând suma maximă cu
$max(dp[i][msk] + v[i+1], 0)$, fie să alegem lungimea unei secvențe pe care
să o ștergem, actualizând stări de tip $dp[i + L][msk + L]$, unde $L$ este
o putere a lui $2$ ce nu apare în masca curentă.

Acest mod de abordare al dinamicii este inspirat din algoritmul lui Kadane pentru
subsecvența de sumă maximă a unui șir, condițiile fiind identice cu cele
folosite pentru acest algoritm.

Astfel, complexitatea algoritmului va deveni $O(n^2)$, deoarece suma maximă
a unor măști folosite este $n$.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
--8<-- "olimpiada/XI-XII/2020-recyclebin.cpp"
```