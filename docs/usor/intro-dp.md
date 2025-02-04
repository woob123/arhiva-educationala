---
id: intro-dp
author:
    - Teodor-Ștefan Manolea
    - Ștefan-Cosmin Dăscălescu
    - Ștefan-Iulian Alecu
prerequisites:
    - greedy
    - functions
tags:
    - programare dinamica
    - DP
---

## Introducere

Programarea dinamică (abrev. DP) este un mod de gândire care poate fi folosit
pentru rezolvarea problemelor la care trebuie să aflăm fie numărul de soluții
corecte pentru un anumit set de date, fie soluția optimă (minim, maxim, etc.).

Pentru a realiza acest lucru, vom descompune problema în mai multe subprobleme,
care, luate împreună, să ne obțină răspunsul dorit.

## De ce trebuie să știm programare dinamică?

Deși tipurile de probleme menționate anterior se pot rezolva și cu alte tehnici,
cum ar fi backtracking, greedy, formulă matematică etc., programarea dinamică
vine drept o nouă perspectivă asupra problemelor care ne permite să găsim
soluții pentru o mulțime de probleme, păstrând avantajele care ne sunt oferite
atât de greedy cât și de backtracking, iar în multe cazuri, devenind abordarea
cea mai potrivită, ceea ce face programarea dinamică să devină una dintre cele
mai populare tehnici date la concursurile de algoritmică.

!!! note "Observație"

    Aproape în fiecare an la OJI și ONI clasele XI-XII se dă o problemă care
    necesită o soluție bazată pe metoda programării dinamice, problemele de
    acest tip regăsindu-se și în foarte multe concursuri mai dificile.

## Termenii specifici ai programării dinamice

Pentru a explica părțile componente ale oricărei soluții care se bazează pe
metoda programării dinamice, vom explica un exemplu foarte simplu, folosindu-ne
de șirul lui Fibonacci.

Vom defini șirul lui Fibonacci ca fiind $F_0 = 1$, $F_1 = 1$ și $F_i = F_{i-1} +
F_{i-2}$.

După cum se poate observa, avem $F_i$ care reprezintă cel de-al $i$-lea număr
Fibonacci, acesta fiind starea pe care o vom calcula, stări pe care le vom găsi
în toate problemele ce necesită programare dinamică. Stările vor avea una sau
mai multe dimensiuni.

$F_i = F_{i-1} + F_{i-2}$ reprezintă relația de recurență dintre $F_i$ și
valorile precedente. Pasul de la $F_{i-1}$ sau $F_{i-2}$ la $F_{i}$ reprezintă
tranzițiile de la o stare la alta.

În cele din urmă, $F_0 = 1$, $F_1 = 1$ reprezintă cazurile de bază, cazuri care
nu pot fi definite folosind relația de recurență menționată mai sus.

În concluzie, pentru a putea avea o soluție care folosește programarea dinamică,
trebuie să ne asigurăm că avem următoarele părți:

- Cazuri de bază
- Stare pe care o calculăm
- Relație de recurență
- Tranzitii

Aceste aspecte pot fi și optimizate în funcție de problemă, aceste optimizări
fiind realizabile fie calculând tranzițiile mai rapid, fie reducând numărul de
stări, așa cum se va vedea de-a lungul capitolelor ulterioare care abordează
tehnica programării dinamice.

<!-- Types of DP -->
## Clasificare

În elaborarea unui algoritm care folosește metoda programării dinamice, putem
utiliza mai multe abordări.

### Tipuri de scriere

<!-- TODO adăugare linkuri pentru complete search recursiv și iterativ -->
- Recursiv

- Iterativ

|     **Recursiv**     |      **Iterativ**       |
| :------------------: | :---------------------: |
|       Mai lent       |        Mai rapid        |
| Mai ușor de elaborat | Mai dificil de elaborat |
|      Memoizare       |        Tabulare         |
|     Top-Down DP      |      Bottom-Up DP       |

### Modalități de abordare

1. Top-Down DP:

    - Această formă de DP pleacă de la starea finală a problemei, ea utilizând
      stările anterioare, până la starea inițială pe care o cunoaștem, pentru
      a-și construi parametrii ei.
    - De obicei această formă de DP este scrisă utilizând recursivitatea

2. Bottom-Up DP:
    - Această formă de DP pleacă de la starea inițială a problemei, ea
      construind parametrii stărilor următoare care la rândul lor vor face asta
      până ce ajungem la construirea parametrilor stării finale.
    - De obicei această formă de DP este scrisă utilizând Complete Search-ul

### Modalități de tranziție

- Pull-DP: Informația din starea curentă se formează bazându-se pe una sau mai
  multe stări anterioare.
- Push-DP: Informația din starea curentă se utilizează pentru calcularea unei
  stării viitoare.

<!-- Problems -->
## Probleme clasice

<!-- Beginning --> Pentru a ne dezvolta intuiția în ceea ce privește această
tehnică, este necesar să înțelegem mecanismul pe care se bazează problemele
clasice.

Este esențial să rezolvăm cât mai multe probleme de DP pentru a deveni fluenți
în elaborarea soluțiilor.

<!-- First Problem -->
### Problema [Frog 1](https://atcoder.jp/contests/dp/tasks/dp_a)

Această problemă ne cere să aflăm costul minim de care are nevoie o broască
pentru a ajunge de pe prima piatră pe ultima, dacă se poate deplasa doar pe
piatra din față sau pe piatra cu două poziții mai în față.

Deoarece restricțiile sunt foarte mari, aplicarea unei soluții care verifică
toate variantele posibile folosind Backtracking este imposibilă, iar diverse
abordări de tip Greedy, precum saltul spre cea mai apropiată piatră sau spre
piatra cu o diferență de înălțime minimă sunt greșite.

Astfel, se impune folosirea unei soluții care folosește metoda programării
dinamice, unde putem stoca $dp[i]$ care va reprezenta suma minimă a diferențelor
salturilor necesare pentru a ajunge la poziția $i$, motivul pentru care vrem să
facem asta este acela că este important să avem un răspuns optim stocat pentru
fiecare poziție în parte.

Pentru a calcula $dp[i]$, ne vom folosi de informațiile din enunț, și anume
faptul că avem voie să sărim pe o poziție aflată la cel mult două unități de
poziția curentă, astfel $dp[i]$ va fi egal cu următoarea formulă:

$$
    {dp}_{i} = \min(dp_{i-1} + |v_i - v_{i-1}|, dp_{i-2} + |v_i - v_{i-2}|)
$$

unde $|x| = \operatorname{abs}(x)$ este valoarea absolută. Cazul de bază constă
în faptul că $dp_1 = 0$ și $dp_2 = |v_1 - v_2|$, iar complexitatea
acestei soluții este $O(n)$.

Mai jos puteți găsi abordarea recursivă și cea iterativă a problemei.

=== "Recursiv"

    ```cpp
    --8<-- "usor/dp/frog1_rec.cpp"
    ```

=== "Iterativ"

    ```cpp
    --8<-- "usor/dp/frog1_iter.cpp"
    ```

<!-- Second Problem -->

### Problema [Frog 2](https://atcoder.jp/contests/dp/tasks/dp_b)

La fel ca la problema precedentă, trebuie să aflăm costul minim de care are
nevoie o broască pentru a ajunge de pe prima piatră pe ultima, dacă se poate
deplasa cu cel mult $k$ pași la un moment dat.

Spre deosebire de problema precedentă, pentru a calcula $dp[i]$, ne vom folosi
de informațiile din enunț, recurența de la Frog 1 va fi extinsă pentru a acoperi
$k$ poziții.

$$
dp_i = \min (dp_{i-x} + |v_i - v_{i-x}|),\,\forall x \leq k
$$

Cazul de bază constă în faptul că $dp_1 = 0$ și $dp_2 = |v_1 - v_2|$, iar
complexitatea acestei soluții este $O(n \cdot k)$.

=== "Recursiv"

    ```cpp
    --8<-- "usor/dp/frog2_rec.cpp"
    ```

=== "Iterativ"

    ```cpp
    --8<-- "usor/dp/frog2_iter.cpp"
    ```

<!-- Third Problem -->
### Problema [Moneda](#)

!!! info "Cerință"

    Astăzi, la ora domnului **profesor Tetris**, ți s-a pus următoarea
    întrebare: „Dacă eu îți dau $N$ tipuri de monede, având acces la o
    infinitate de monede $C$ de acele tipuri, află modalitatea optimă de a
    obține suma $S$”. Pe momentul orei tu nu ai știut cum să răspunzi, Însă
    acum, mai determinat ca niciodată, vrei să rezolvi această problem, având în
    față un document educational de 5 stele Micheline. Rezolvă problema!

    Vom defini modalitatea optimă de a obține suma $S$ ca fiind modalitatea prin
    care utilizezi cât mai puține monede per total!

    **Restricții:**

    - $ 1 \leq N \leq 500 $
    - $ 1 \leq S \leq 100000 $
    - $ 1 \leq C_i \leq 2500 $

La început, când ați citit această problemă, probabil v-ați gândit la o
rezolvare Greedy (care mai încolo o să vedeți că este Greedy Euristic), prin
care ați fi sortat descrescător șirul de monede și ați fi încercat să utilizați
denominația cea mai mare, care este mai mică ca S, cât timp puteați. După ați fi
continuat cu următoarea denominație cea mai mare care respectă condiția aceasta
pentru suma rămasă ș.a.m.d. Ca să vă dovedesc că nu funcționează această
modalitate, încercați să rezolvați această problemă, utilizând modalitatea
anterior prezentată, având aceste date de intrare ($N$ numărul de monede, apoi
$S$ suma și apoi cele $N$ monede):

```
3
31
7 2 15
```

Acum că ați încercat să rezolvați problema într-un mod cunoscut vouă, și ați
văzut că nu îți garantează un răspuns, haideți să vă prezint o soluție corectă!

Pentru această problem, o să vă prezint soluțiile utilizând ambele modalități de
abordare și scriere a sursei și modalități de tranziție.

=== "Recursiv"
    !!! note "Explicație"

        Pentru a găsi soluția optimă, noi vom avea vectorul dp care se
        utilizează pentru memoizare, el având forma următoare:
        dp[suma de bani rămasă de acoperit] = numărul de bacnote necesare pentru
        a ajunge la suma de bani rămasă de acoperit curentă.
        
        Pentru asta ne vom utiliza de o recursie care are ca parametri de stare
        suma de bani care a rămas de plătit, numărul de monede pe care l-am
        utilizat până acum și vectorul de denominații accesibile.

    ```cpp
    --8<-- "usor/dp/moneda_rec.cpp"
    ```

=== "Iterativ"
    !!! note "Explicație"

        Pentru a găsi soluția optimă, noi vom trece prin fiecare sumă de bani
        care este mai mică decât $S$, încercând, dacă putem, să continuăm să
        adăugăm bacnote astfel încât să ajungem la suma de bani dorită. Pentru
        acest lucru vom ține un vector dp de forma următoare:
        dp[sumă de bani totală] = numărul de bacnote necesare pentru a ajunge la
        această sumă de bani.

    ```cpp
    --8<-- "usor/dp/moneda_iter.cpp"
    ```

<!-- Fourth Problem -->
### Problema [Vacation](https://atcoder.jp/contests/dp/tasks/dp_c)

Pentru această problemă, trebuie să aflăm satisfacția maximă pe care o poate
obține Taro în $n$ zile, dacă nu are voie să repete o activitate de două ori la
rând.

Din nou, se poate demonstra cu ușurință că o soluție de tip Greedy nu va merge,
deoarece ne putem bloca alegeri mai bune ulterior cu alegerile curente.

Totodată, mai apare un element nou în rezolvarea acestor probleme, și anume
faptul că vom avea nevoie de o nouă dimensiune pentru a păstra informații cu
privire la ultima activitate efectuată de el, pentru a evita o situație în care
alegem de două ori aceeași activitate.

Astfel, vom defini $dp_{i,j}$ ca fiind suma maximă a satisfacției dacă am
parcurs primele $i$ zile, iar ultima activitate a fost de tipul $j$, $j$ fiind
0, 1 sau 2, în funcție de activitatea aleasă.

Pentru a calcula $dp_{i,j}$, va trebui să ne raportăm la sumele din ziua
precedentă, corespunzătoare celorlalte două activități deoarece nu avem voie să
alegem aceeași activitate iar.

$$
dp_{i,j} = \max(dp_{i-1,x} + v_j),\,\forall x \neq j
$$

Din nou ca la celelalte probleme, puteți găsi mai jos abordarea recursivă și cea
iterativă a problemei.

!!! note "Observație"

    Deoarece avem nevoie doar de valorile din ziua precedentă, nu este necesar
    să păstrăm în memorie toată matricea, ci doar ultimele două linii,
    optimizare pe care o vom prezenta în detaliu mai jos.

=== "Recursiv"

    ```cpp
    --8<-- "usor/dp/vacation_iter.cpp"
    ```

=== "Iterativ"

    ```cpp
    --8<-- "usor/dp/vacation_iter.cpp"
    ```

=== "Iterativ optimizat"

    !!! info "Explicație"
        
        Deoarece avem nevoie doar de ultimele două linii, nu vom ține toată
        matricea, mutând mereu valorile calculate pe prima linie pentru a păstra
        corectitudinea recurenței.

    ```cpp
    --8<-- "usor/dp/vacation_iter_optim.cpp"
    ```

<!-- Fifth problem -->

### Problema [Array Description](https://cses.fi/problemset/task/1746/)

Pentru această problemă, trebuie să aflăm numărul de șiruri pe care le putem
construi astfel încât să respecte condițiile din enunț cu privire la valorile
deja setate și la diferența dintre ele.

Astfel, vom defini $dp_{i,j}$ ca fiind numărul de moduri de a crea un șir cu $i$
numere, dacă valoarea de pe poziția $i$ este $j$.

Pentru a afla $dp_{i,j}$, va trebui să ne raportăm la valorile de pe poziția
precedentă, aflate la o distanță de cel mult 1, cu condiția să putem pune $j$ pe
poziția $i$.

!!! note "Observatie"

    Pentru a calcula numărul de soluții modulo $x$, vom folosi operatorul $\%$
    (mod). Dar deoarece aici avem nevoie doar de operații de adunare, putem pur
    și simplu să efectuăm operațiile de adunare și să folosim scăderi în mod
    convenabil, reușind astfel să optimizăm soluția.

```cpp
--8<-- "usor/dp/array_description.cpp"
```

<!-- Sixth problem -->

## Problema [Grid 1](https://atcoder.jp/contests/dp/tasks/dp_h)

Pentru această problemă, trebuie să aflăm numărul de moduri de a parcurge
matricea din colțul stânga-sus în colțul dreapta-jos prin mișcări în jos și la
dreapta, fără să parcurgem pătrate acoperite de ziduri.

Deoarece avem de-a face cu o matrice, putem ține $dp_{i,j}$ ca fiind numărul de
moduri de a parcurge matricea dacă am ajuns la pătratul $(i, j)$. Deoarece putem
ajunge la $(i, j)$ din pătratele de sus și stânga, acestea vor fi cele două
rezultate care contribuie la răspunsul dat.

Astfel, $dp_{i,j} = dp_{i-1,j} + dp_{i,j-1}$.

### Soluție

```cpp
--8<-- "usor/dp/grid1.cpp"
```

<!-- Seventh problem -->
## Problema [Sumtri1](https://www.pbinfo.ro/probleme/386/sumtri1)

=== "Recursiv"

    ```cpp
    --8<-- "usor/dp/sumtri1_rec.cpp"
    ```

=== "Iterativ"

    ```cpp
    --8<-- "usor/dp/sumtri1_iter.cpp"
    ```

<!-- Extra stuff -->

## Probleme suplimentare

- [Infoarena custi](https://infoarena.ro/problema/custi)
- [Codeforces Boredom](https://codeforces.com/problemset/problem/455/A)
- [AtCoder Weak Takahashi](https://atcoder.jp/contests/abc232/tasks/abc232_d)
- [IIOT 2023-24 PingPong](https://kilonova.ro/problems/1941)
- [AtCoder 1111gal password](https://atcoder.jp/contests/abc242/tasks/abc242_c)
- [AtCoder Flip Cards](https://atcoder.jp/contests/abc291/tasks/abc291_d)
- [IIOT Police](https://kilonova.ro/problems/967)
- [Counting Towers](https://cses.fi/problemset/task/2413)
- [AtCoder Index × A(Not Continuous
  ver.)](https://atcoder.jp/contests/abc267/tasks/abc267_d)
- [AtCoder Between Two
  Arrays](https://atcoder.jp/contests/abc222/tasks/abc222_d)
- [Lot Juniori Minusk](https://kilonova.ro/problems/1743)
- [AtCoder Count Bracket
  Sequences](https://atcoder.jp/contests/abc312/tasks/abc312_d)
- [AtCoder I hate Non Integer
  Number](https://atcoder.jp/contests/abc262/tasks/abc262_d)
- [Problemele cu DP de pe Kilonova](https://kilonova.ro/tags/275)
- [Problemele intre rating 500 si 1400 de
  aici](https://atcoder-tags.herokuapp.com/tag_search/Dynamic-Programming)
- [Problemele cu DP de pe
  infoarena](https://infoarena.ro/cauta-probleme?tag_id[]=58)

## Resurse suplimentare

- [DP Book](https://dp-book.com/Dynamic_Programming.pdf)
- [Programare dinamica - CPPI
  Sync](https://cppi.sync.ro/materia/probleme_diverse_dinamica_3.html)
- [Introduction to DP - USACO Guide](https://usaco.guide/gold/intro-dp?lang=cpp)
- [DP Tutorial and Problem List](https://codeforces.com/blog/entry/67679)
