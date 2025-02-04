---
id: OJI-2020-XI-XII-partit
author:
    - Ștefan-Cosmin Dăscălescu
prerequisites:
    - intro-combinatorics
    - intro-dp
tags:
    - OJI
    - clasa XI-XII
    - combinatorica
    - partitii
    - programare dinamica
---

# Soluția problemei partit (OJI 2020, clasele XI-XII)

!!! note "Link problemă"
    Această problemă poate fi accesată [aici](https://kilonova.ro/problems/18/).

Pentru a rezolva această problemă, va trebui să observăm mai întâi faptul că
pentru un număr dat $n$, există $2^{n-1} - 1$ partiții cu suma $n$, lucru ce
se poate demonstra destul de ușor folosind inducție, ideea de bază fiind că
dacă știm numărul de partiții pentru $1, 2, \dots, n-1$, vom putea extinde
partiția cu diferența dintre $n$ și acea sumă.

Pentru a afla cea de-a $k$-a partiție în ordine lexicografică, vom putea merge
din aproape în aproape să vedem câte partiții încep cu fiecare număr de la $1$
la $n$ (acest lucru se poate face fie folosind formule matematice, fie folosind
o recurență foarte simplă), iar implementarea se poate face fără probleme recursiv.

Pentru a afla numărul de ordine al unei partiții, vom proceda similar, ținând
cont de numărul de partiții care încep cu un anumit număr și au o anumită sumă.
Din nou, se poate proceda din aproape în aproape, iar deoarece valorile pentru
$n$ și numerele de ordine ale partițiilor sunt destul de mici, complexitatea
va fi una foarte bună, permițând chiar și unor soluții mai incete să ruleze
în timp bun.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
--8<-- "olimpiada/XI-XII/2020-partit.cpp"
```