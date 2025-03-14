---
id: OJI-2020-VII-wind
title: Soluția problemei wind (OJI 2020, clasa a VII-a)
problem_id: 924
authors: [stefdasca]
prerequisites:
    - partial-sums
    - divisibility
tags:
    - OJI
    - clasa VII
    - sume partiale
    - divizibilitate
---


Pentru a putea împărți cele $n$ eoliene în mod egal în $k$ orașe
trebuie să determinăm divizorii numărului $n$.

La cerința 1, se determină numărul de divizori al lui $n$. Rezultatul
va fi numărul de divizori - 1 deoarece se specifică în enunț că se vor
construi cel puțin două orașe, deci $n$ nu este un divizor valid.

La cerința 2, vom avea nevoie de sume parțiale pentru a afla suma
energiilor corespunzătoare pe primele i poziții, $suma[i]$ va
reprezenta suma valorilor energiilor generate (pierdute) de centralele
de la $1$ la $i$.

Pentru a împărți centralele în mod corect trebuie să detrminăm
divizorii numărului $n$. Pentru fiecare divizor se parcurge vectorul de
sume din divizor in divizor și se calculează diferența dintre sumele de 
pe pozițiile respective, alegându-se împărțirea optimă.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
--8<-- "olimpiada/VII/2020-wind.cpp"
```
