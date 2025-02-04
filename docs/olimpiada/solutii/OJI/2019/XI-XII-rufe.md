---
id: OJI-2019-XI-XII-rufe
author:
    - Ștefan-Cosmin Dăscălescu
prerequisites:
    - binary-search
    - basic-math
tags:
    - OJI
    - clasa XI-XII
    - cautare binara
    - matematica
---

# Soluția problemei rufe (OJI 2019, clasele XI-XII)

!!! note "Link problemă"
    Această problemă poate fi accesată [aici](https://kilonova.ro/problems/21/).

Pentru a rezolva această problemă, ne putem gândi mai întâi la o soluție înceată,
care va calcula pentru fiecare punct distanța de la punctul central la toate
celelalte puncte, le ordonează crescător și ia cele mai mari $k$, afișând
distanța corespunzătoare.

Totuși, această abordare este foarte înceată și nu ne va aduce punctajul maxim.
Astfel, vom vrea să ne gândim la optimizări. În primul rând, o idee pe care o
putem explora este aceea că dacă ne gândim la o linie sau o coloană, distanța
de la origine la punctele de pe acea linie/coloană sunt mai întâi descrescătoare,
iar mai apoi crescătoare, cu punctul paralel cu originea fiind cel mai apropiat
de aceasta.

Această distribuție a distanțelor ne duce cu gândul la a reprezenta aceste
regiuni ale matricii, împărțind astfel suprafața în patru zone, pentru care putem
simplifica răspunsurile foarte mult, după modelul de mai jos.

![](../../../../images/olimpiada/oji/2019-rufe-1.png)

Apoi, deoarece cu cât ne apropiem mai mult de origine, vom acoperi tot mai multe
puncte, ne putem gândi la o căutare binară pe răspuns, scopul nostru este să
aflăm numărul de puncte aflate la o distanță mai mare decât valoarea țintă.

Deoarece am împărțit în patru regiuni, este îndeajuns să aflăm aria
suprafețelor acoperite, care se reduce la aria unor triunghiuri cu puncte
în jurul colțurilor cele mai îndepărtate, un exemplu fiind arătat mai jos.

![](../../../../images/olimpiada/oji/2019-rufe-2.png)

Folosind formulele pentru aria unui triunghi dreptunghic (mai multe detalii
puteți găsi în implementare, numărul de cazuri necesare devine mult mai mic,
implementarea devenind mult mai facilă).

Astfel, soluția voastră va rula în timp logaritmic, fiind îndeajuns de rapidă
pentru obținerea celor 100 de puncte.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
--8<-- "olimpiada/XI-XII/2019-rufe.cpp"
```