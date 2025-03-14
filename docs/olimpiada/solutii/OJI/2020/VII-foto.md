---
id: OJI-2020-VII-foto
title: Soluția problemei foto (OJI 2020, clasa a VII-a)
problem_id: 923
authors: [stefdasca]
prerequisites:
    - partial-sums
    - matrices
tags:
    - OJI
    - clasa VII
    - sume partiale
---

Această problemă are foarte multe soluții posibile, aici voi descrie doar una dintre
ele.

Pentru a rezolva cerința 1, vom număra câte celule sunt egale cu 0, acest lucru
fiind îndeajuns.

La cerința 2, noi vrem să aflăm numărul maxim de fulgere situate consecutiv, iar
pentru a ține numărătoarea cât mai ușor, ne putem folosi de sume parțiale pentru
fiecare poziție, având grijă la respectarea condițiilor din enunț.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
--8<-- "olimpiada/VII/2020-foto.cpp"
```
