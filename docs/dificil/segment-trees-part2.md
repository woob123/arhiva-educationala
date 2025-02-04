---
id: segment-trees-part2
author:
    - Ștefan-Cosmin Dăscălescu
prerequisites:
    - segment-trees
tags:
    - structuri de date
    - arbori de intervale
    - lazy propagation
---

## Introducere

Așa cum ați văzut și în
[articolul anterior](./segment-trees.md) pe această
temă, arborii de intervale se dovedesc a fi o structură de date foarte
puternică, care ne ajută în foarte multe tipuri de probleme. În cele ce urmează,
vom prezenta un set de operații adiționale pe care le putem face folosind
arborii de intervale, precum căutarea binară pe un arbore de intervale, precum
și actualizări pe interval, folosind metoda lazy propagation.

## Căutarea binară pe un arbore de intervale

Să presupunem că avem de rezolvat următoarea problemă:

Se da un șir de $n$ valori și $q$ queryuri în care fie schimbăm o valoare din
șir, fie trebuie să aflăm prima poziție care are o valoare mai mare sau egală cu
un $x$ dat.

O soluție care este foarte simplu de implementat ar fi să folosim o căutare
binară pe rezultat, iar la un pas al căutării binare, să folosim un arbore de
intervale care să ne răspundă rapid la un query de tipul care este valoarea
maximă în intervalul $[1, mid]$.

Deși această soluție rulează în $O(q \log^2 n)$, poate fi prea înceată în
anumite situații. De aceea, se impune găsirea unor optimizări care să ne permită
să nu mai avem nevoie de o altă căutare binară.

Dacă ne gândim la un interval de tipul $[a, b]$, noi vom putea efectua
următoarea operație fără probleme, până când ajungem la un interval de lungime
$1$:

- aflăm primul interval inclus în intervalul $[a, b]$.
- dacă acest interval are un maxim mai mare sau egal ca $x$, atunci căutăm în
  acest subinterval
- altfel, vom căuta în subintervalele de la dreapta.

Astfel, am demonstrat că putem folosi o strategie de tip divide et impera pentru
a aborda această problemă, așa cum se poate vedea în segmentul de cod de mai
jos:

```cpp
--8<-- "dificil/segtree/binsearch.cpp"
```

### Problemă exemplu: [Hotel Queries - CSES](https://cses.fi/problemset/task/1143)

Pentru a rezolva această problemă, va trebui să folosim această tehnică pentru a
afla prima poziție din șir cu o valoare mai mare decât valoarea dată, iar dacă o
găsim, vom scădea $x$ din valoarea acelui număr pentru a simula procesul prin
care oamenii se cazează la hotel.

Mai jos puteți vedea codul sursă pentru problema dată.

```cpp
--8<-- "dificil/segtree/hotelqueries.cpp"
```

## Actualizări pe interval - Lazy Propagation

Am văzut până acum în probleme faptul că putem folosi arborii de intervale în
mod eficient pentru a putea efectua actualizări pe unele valori precum și
interogări pe intervale. Totuși, aceste lucruri nu ne ajută încă să rezolvăm
situațiile în care trebuie să schimbăm valorile de pe un interval și să
prelucrăm șirul conform acestor operații.

Tehnica folosită în aceste situații este una specifică și se numește lazy
propagation și vom putea folosi această tehnică pentru a rezolva problemele ce
țin de actualizarea unui interval de valori și răspunderea la aceleași tipuri de
queryuri.

Cel mai important principiu pe care îl aplicăm atunci când vom prelucra asemenea
actualizări este acela că vom vrea să le amânăm cât mai mult posibil (de aici
vine și denumirea de lazy propagation), iar pentru a face asta, în loc să
actualizăm fiecare valoare individuală, vom ține evidența existenței acelei
actualizări într-un vector auxiliar, _lazy_, care va avea drept rol păstrarea
unei cantități minime de informație care să ne permită să reconstituim
actualizările care s-au făcut la un moment dat.

### Problemă introductivă: [Range Update Queries - CSES](https://cses.fi/problemset/task/1651)

Aici va trebui să creștem valorile dintr-un interval cu o valoare dată și să
aflăm valoarea de la o anumită poziție. Deși această problemă are și alte
soluții (precum cea cu arbori indexați binari), aici ne vom concentra pe
învățarea acestei noi tehnici.

În cazul acestei probleme, deoarece avem de făcut actualizări pe interval în
care creștem valori, asta ne dă motivația și pentru ce vom stoca în vectorul
_lazy_, și anume suma valorilor din queryurile care au atins nodul respectiv,
aceasta fiind singura diferență majoră între codul unei probleme obișnuite ce
necesită arbori de intervale și problemele cu lazy propagation.

Ulterior, când procesăm o interogare, vom parcurge în jos drumul de la rădăcină
la nodul curent și vom procesa actualizările relevante, împărțind în două
conținutul queryurilor, eventual unind conținutul queryului cu alte queryuri
anterioare.

!!! note "Observație"

    Va fi foarte important atunci când trecem într-un nod să procesăm imediat
    conținutul din lazy pentru a putea asigura păstrarea corectă a valorilor
    reale din fiecare nod.

Mai jos puteți vedea codul sursă, unde operațiile specifice metodei lazy
propagation sunt explicate în detaliu. Se remarcă atenția la detalii atunci când
transferăm conținutul unui query de la un nod la unul din fii, precum și faptul
că realizăm actualizările imediat ce vizităm un nod.

```cpp
--8<-- "dificil/segtree/rangeupdatequeries.cpp"
```

### Problema [Simple - Info1Cup 2019](https://kilonova.ro/problems/3424)

Pentru a rezolva această problemă, vom începe prin a precalcula pentru fiecare
interval valoarea maximă pară, valoarea maximă impară, valoarea minimă pară și
valoarea minimă impară, deoarece o observație importantă este faptul că
adăugarea unei valori pe un interval nu va schimba ordinea relativă a acestor
valori.

Astfel, vom păstra pentru fiecare nod în lazy un contor care ține cont de suma
actualizărilor efectuate la acea poziție, pentru a stabili dacă trebuie să luăm
valorile originale sau cele inverse (minim par/maxim impar) sau (maxim par/minim
impar).

Apoi, vom avea grijă la implementarea soluției finale să includem aceste date în
răspunsul final.

```cpp
--8<-- "dificil/segtree/simple.cpp"
```

### Problema [Range Updates and Sums CSES](https://cses.fi/problemset/task/1735)

Pentru a rezolva această problemă, va trebui să fim atenți la diferitele tipuri
de operații pe care le primim, precum și cum codificăm schimbările (sumă sau
schimbarea unei valori), lucruri ce se pot face doar cu o implementare clară a
structurii de date folosite.

Pentru a nu folosi mai mulți vectori de tip lazy, am codificat schimbările de
valori cu numere negative și adăugările cu numere pozitive, pentru a ști tipul
de operație folosit. Ulterior, queryurile de sumă devin asemănătoare celor
obișnuite.

Mai jos găsiți soluția care ia accepted.

```cpp
--8<-- "dificil/segtree/rangeupdateandsums.cpp"
```

## Concluzii

Căutarea binară în arborele de intervale, precum și lazy propagation sunt
tehnici foarte des utilizate în aceste probleme, fiind la baza multor aplicații,
așa cum ați putut observa în acest articol. O recomandare pe care o oferim este
aceea de a ști foarte bine cum să implementați lazy propagation deoarece această
variație a arborilor de intervale poate pune dificultăți dacă nu este abordată
cum trebuie.

Recomandăm abordarea unui număr cât mai mare de probleme dintre cele prezentate
aici, precum și a problemelor de pe CSES de la secțiunea Range Queries.

## Probleme suplimentare

- [IIOT Predictor Machine](https://kilonova.ro/problems/953)
- [CSES Prefix Sum Queries](https://cses.fi/problemset/task/2166)
- [USACO Platinum Counting Haybales](http://www.usaco.org/index.php?page=viewproblem2&cpid=578)
- [Infoarena fibo4](https://www.infoarena.ro/problema/fibo4)
- [Codeforces DZY Loves Fibonacci Numbers](https://codeforces.com/contest/446/problem/C)
- [ONI 2023 Baraj Seniori sirbun](https://kilonova.ro/problems/556)
- [IOI 2014 Wall](https://oj.uz/problem/view/IOI14_wall)
- [RoAlgo Contest #12 - Nice Apple Tree](https://kilonova.ro/problems/3389)
- [ONI 2024 Baraj Seniori balama](https://kilonova.ro/problems/2666)
- [Biscuiti infoarena](https://infoarena.ro/problema/biscuiti)
- [Luffpar infoarena](https://www.infoarena.ro/problema/luffpar)
- [USACO Platinum Bessie's Snow Cow](http://www.usaco.org/index.php?page=viewproblem2&cpid=973)
- [JOI 2018 Bubblesort 2](https://oj.uz/problem/view/JOI18_bubblesort2)
- [Problemele cu lazy propagation de pe kilonova](https://kilonova.ro/tags/286)

## Resurse suplimentare

- [Range Update Range Query - USACO Guide](https://usaco.guide/plat/RURQ)
- [CP Algorithms - Range updates (Lazy Propagation)](https://cp-algorithms.com/data_structures/segment_tree.html#range-updates-lazy-propagation)
- [Historic Sums and Range Add Range Sum](https://codeforces.com/blog/entry/99895)