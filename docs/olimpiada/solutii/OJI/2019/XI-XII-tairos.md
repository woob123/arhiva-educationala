---
id: OJI-2019-XI-XII-tairos
author:
    - Ștefan-Cosmin Dăscălescu
prerequisites:
    - intro-dp
    - tree-1
tags:
    - OJI
    - clasa XI-XII
    - programare dinamica
    - arbori
---

# Soluția problemei tairos (OJI 2019, clasele XI-XII)

!!! note "Link problemă"
    Această problemă poate fi accesată [aici](https://kilonova.ro/problems/22/).

Pentru a rezolva această problemă, trebuie să ne gândim mai întâi la
o precalculare pe baza arborelui inițial. Astfel, putem face o parcurgere
de tip DFS să aflăm pentru arborele dat câte noduri și câte frunze (excluzând
rădăcina) se află pe fiecare nivel, acest lucru fiind realizabil fără prea mari
probleme.

Motivul pentru care este important să numărăm și frunzele de pe un anumit nivel
este acela că din frunze, putem genera noi arbori conform construcției menționate
în cerința problemei.

Ulterior, nevoia de a număra nodurile aflate la o anumită distanță face naturală
folosirea unei dinamici pentru a număra nodurile și frunzele de pe un anumit
nivel.

Astfel, ne putem gândi la o serie de stări de tipul următor:

- $dst[i]$ - numărul de noduri aflate la nivelul $i$ în arborele inițial.
- $leaves[i]$ - numărul de frunze aflate la nivelul $i$ în arborele inițial.
- $dp[i]$ - numărul de noduri aflate la nivelul $i$ în arborele infinit.
- $cntleaves[i]$ - numărul de frunze aflate la nivelul $i$ în arborele infinit.

Astfel, pentru a calcula $dp[i]$ și $cntleaves[i]$, ne putem duce înapoi pe urma
nivelelor și să adunăm produsele dintre valorile de pe nivelurile precedente și
numărul de frunze de la un anumit nivel.

Complexitatea finală a soluției va fi $O(n \cdot d)$, iar ca un detaliu suplimentar,
trebuie să fim atenți în ceea ce privește operațiile cu modulo, deoarece
răspunsurile pot deveni foarte mari.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
--8<-- "olimpiada/XI-XII/2019-tairos.cpp"
```