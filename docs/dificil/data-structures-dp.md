---
id: data-structures-dp
authors: [stefdasca]
prerequisites:
    - segment-trees
    - intro-dp
    - deque
tags:
    - programare dinamica
    - optimizare
    - structuri de date
---

## Introducere

Programarea dinamică și structurile de date reprezintă două dintre capitolele
cele mai populare în rândul elevilor, precum și în rândul problemelor date la
competiții, de multe ori acestea formează o parte esențială a seturilor de
probleme și de cunoștințe care departajează cei mai buni concurenți la olimpiade
și concursuri.

În multe situații, atunci când ne gândim la soluții care folosesc dinamici și
alte recurențe similare, ajungem să ne gândim la optimizări care ne duc cu
gândul la probleme care se pot rezolva ușor folosind structuri de date. Astfel,
se dă naștere unui tip nou de probleme, dinamicile cu structuri de date.

În acest articol, vom prezenta câteva exemple de probleme, împreună cu modul
în care aplicăm structurile de date și utilitatea îmbinării celor două seturi
de cunoștințe. Fie că este vorba de structuri precum arbori de intervale,
arbori indexați binar, stive, cozi sau deques, aceste tipuri de dinamici
sunt foarte utile și importante de știut.

## Problema exemplu - [Projects CSES](https://cses.fi/problemset/task/1140/)

În această problemă, trebuie să alegem un număr de proiecte care nu se suprapun
și ne aduc suma profiturilor maximă. Această problemă reprezintă o dinamică
clasică, dar dat fiind faptul că avem restricții foarte mari, nu putem să
o rezolvăm folosind metodele obișnuite. Astfel, vom avea nevoie să [normalizăm
mai întâi datele](../mediu/data-normalization.md), iar mai apoi ne putem gândi
la o recurență în care avem $dp[i]$ ca fiind suma maximă a profiturilor pentru
un proiect care se termină la al i-lea moment distinct de timp.

În mod evident, pentru a calcula această dinamică, vom avea nevoie de un maxim
parțial pentru primele $j$ poziții, unde $j$ este poziția cea mai mare din
șirul de momente de timp cu proprietatea că acel moment de timp nu se suprapune
cu intervalul pe care îl alegem.

Pentru a optimiza această dinamică, vom putea folosi fie o structură de date
(arbore de intervale sau indexat binar), fie chiar un maxim parțial care ar
reduce și mai mult complexitatea soluției cerute.

Mai jos puteți găsi o soluție care obține accepted.

```cpp
--8<-- "dificil/data-structures-dp/projects.cpp"
```

## Problema [Subsequences - Codeforces](https://codeforces.com/contest/597/problem/C)

În această problemă, trebuie să numărăm câte subșiruri crescătoare cu lungimea $k+1$
are șirul dat, iar o proprietate importantă este aceea că valorile formează
o permutare a primelor $n$ numere naturale.

Deoarece lungimea căutată este mică, ne putem gândi la a păstra o dinamică pe
două dimensiuni, care să ne păstreze numărul de subșiruri de lungime $i$ care
se termină cu valoarea $j$.

Pentru a calcula această dinamică, vom vrea să știm pentru o pereche $(i, j)$
numărul de moduri corespunzătoare tuturor perechilor eligibile de forma $(i-1, x)$
cu proprietatea că $x < j$, iar $poz[x] < poz[j]$, unde $poz[x]$ reprezintă
unde apare $x$ în șirul dat.

Această recurență se poate din nou calcula relativ ușor folosind arbori de
intervale sau arbori indexați binar, mai jos puteți găsi o soluție care rezolvă
problema.

```cpp
--8<-- "dificil/data-structures-dp/subsequences.cpp"
```

## Problema [Tulip Bouquet - IIOT](https://kilonova.ro/problems/2086)

Pentru a rezolva această problemă, ne putem gândi în primul rând la dinamica
relativ simplă, dar înceată în $\mathcal{O}(n \cdot k^2)$ care are tranziții în $\mathcal{O}(k)$.

Obiectivul principal va fi să optimizăm această soluție, iar cel mai simplu
mod de a face acest lucru este să ținem o stivă cu pozițiile cele mai relevante
pentru tranzițiile noastre.

Chiar dacă acest lucru nu va fi îndeajuns, ne putem gândi un pas mai departe și
să ținem pentru fiecare poziție relevantă din stivă maximul tuturor pozițiilor
acoperite de acel interval, iar mai apoi pentru acele intervale, să ne gândim
la un maxim parțial care să ne permită să păstrăm toate valorile maxime rapid și
eficient.

Când unim două intervale, vom ține valorile maxime din fiecare din ele și apoi
vom combina noile valori pentru a putea insera din nou acel răspuns în stivă.

Mai jos puteți găsi soluția de 100 de puncte, unde sunt mai multe detalii incluse.

```cpp
--8<-- "dificil/data-structures-dp/bouquets.cpp"
```

## Problema [Pictures with Kittens - Codeforces](https://codeforces.com/problemset/problem/1077/F2)

!!! note "Observatie"
    Această problemă se poate rezolva și folosind [șmenul de la Aliens](../avansat/aliens-dp.md)
    dar aici ne vom concentra pe soluția în $\mathcal{O}(n \cdot k)$.

La o primă citire este evident faptul că putem găsi repede o dinamică de tipul
$dp[i][j]$ ca fiind suma maximă dacă pentru primele $i$ poziții, am folosit $j$
astfel de valori. Această recurență poate fi descrisă ca fiind maximul
ultimelor $k$ poziții de pe coloana precedentă, $j-1$, lucru ce poate fi optimizat
în modul cel mai eficient folosind o structură de date de tip deque, așa cum
se poate vedea și în soluția prezentată mai jos.

```cpp
--8<-- "dificil/data-structures-dp/kittens.cpp"
```

## Problema [Interstellar Intervals USACO Gold](https://usaco.org/index.php?page=viewproblem2&cpid=1450)

!!! note "Observație"
    Această problemă are o soluție video făcută de autor, care poate fi accesată
    [aici](https://www.youtube.com/watch?v=UDMUjRMl5OU). În acest articol vom
    explica doar ideile principale din spatele soluției.

Această problemă duce foarte repede cu gândul la o dinamică prin care putem
număra câte șiruri distincte există cu proprietatea că toate restricțiile
date până la un punct sunt respectate. Deoarece avem două tipuri de culori în
practică (albastru și alb, roșu este dedus din pozițiile colorate în albastru),
ne putem gândi mai întâi să păstrăm două dinamici, după cum urmează:

- dp[i] = nr de moduri de a umple primele $i$ poziții dacă pe poziția $i$ avem albastru
- dp2[i] = nr de moduri de a umple primele $i$ poziții dacă pe poziția $i$ avem alb

Soluția în $\mathcal{O}(n^2)$ este relativ simplă, pentru alb putem avea o tranziție de la
$i-1$ la $i$, iar pentru albastru, putem fixa un interval și verificăm dacă îl
putem plasa conform restricțiilor date, adăugând $dp[x-1]$ dacă am putut plasa
un interval între $x$ și $i$.

Pentru a optimiza soluția, putem observa pentru fiecare poziție $i$ cât de mult
ne putem duce la stânga cu un interval din cauza restricțiilor colorate în roșu,
precum și cât de mult ne putem duce la dreapta de la acea poziție din cauza
restricțiilor colorate cu albastru.

După calcularea acestor valori, putem ține o structură de date (în soluția de
mai jos, un AIB) care să țină suma valorilor de pe un interval pentru a putea
folosi restricțiile cu albastru în avantajul nostru.

În cele din urmă, complexitatea devine $\mathcal{O}(n \log n)$. Puteți citi soluția de mai
jos pentru mai multe detalii, precum și să vizionați videoul de mai sus.

```cpp
--8<-- "dificil/data-structures-dp/interstellar.cpp"
```

## Concluzii

Aceste probleme reprezintă o intersecție a două arii care sunt frecvent întâlnite
la concursurile de algoritmică și nu numai. Cunoașterea celor mai frecvent
întâlnite observații, împreună cu însușirea tipurilor de implementări face
acest capitol important și util pentru studiul vostru individual.

## Probleme suplimentare

- [IIOT 2024 Planning Excursions](https://kilonova.ro/problems/2287)
- [ONI 2010 Stalpi](https://kilonova.ro/problems/184)
- [RCPC 2023 Yet Another Colored Tree Problem](https://kilonova.ro/problems/1888)
- [Lot 2024 Juniori Pokemoni](https://kilonova.ro/problems/2808)
- [Codeforces WI-FI](https://codeforces.com/problemset/problem/1216/F)
- [Codeforces Pathwalks](https://codeforces.com/contest/960/problem/F)
- [ONI 2015 Baraj Seniori s2c](https://kilonova.ro/problems/288/)
- [ONI 2005 Baraj Seniori Evantai](https://kilonova.ro/problems/1869)
- [Codeforces Yunlin's Subarray Queries (hard)](https://codeforces.com/problemset/problem/2009/G2)
- [Codeforces Culture Code](https://codeforces.com/contest/1197/problem/E)
- [JOI 2022 Railway Trip 2](https://oj.uz/problem/view/JOI22_ho_t4)
- [Lot 2008 Seniori turnuri](https://kilonova.ro/problems/2317)
- [Probleme cu dinamici si structuri de date de pe Codeforces](https://codeforces.com/problemset/page/1?tags=dp%2Cdata+structures)
- [Probleme cu dinamica si deque de pe kilonova](https://kilonova.ro/problems?tags=275%2C409)
- [Alte probleme similare de pe kilonova](https://kilonova.ro/problems?tags=275%2C282)

## Resurse suplimentare

- [Data Structures in Dynamic Programming Week - Codeforces](https://codeforces.com/blog/entry/98926)
- [Monotone Queue optimization - robert1003](https://robert1003.github.io/2020/02/16/dp-opt-monotone-queue.html)
