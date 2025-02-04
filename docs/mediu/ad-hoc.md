---
id: ad-hoc
author:
    - Ștefan-Cosmin Dăscălescu
tags:
    - probleme
    - meta
---

!!! note "Conținutul articolului"

    O bună parte din conținutul acestui articol este preluat din cursul creat de
    același autor pentru lotul de juniori din 2023, curs care se poate găsi
    [aici](https://cdn.sepi.ro/articole/curs%20ad-hoc.pdf)

## Introducere

De-a lungul drumului vostru de până acum spre obținerea rezultatelor care v-au
adus aici, ați rezolvat foarte multe probleme de diverse tipuri și de diverse
nivele de dificultate. Pentru o mare parte dintre ele, le putem clasifica după
algoritmul sau tehnica folosită pentru rezolvarea problemei, ceea ce face
exersarea acestor probleme și însușirea cunoștințelor necesare mai facilă, prin
faptul că putem găsi similitudini cu alte probleme rezolvate anterior, precum și
datorită faptului că există numeroase resurse utile pentru învățarea
algoritmilor, metodelor de programare și structurilor de date necesare pentru
implementarea acelor soluții.

În schimb, există un anumit tip de probleme pe care nu le putem încadra sub
umbrela unui algoritm sau a unei tehnici de programare, iar aceste probleme par
a fi complet diferite una față de cealaltă. Aceste probleme sunt cele ad-hoc și
așa cum sugerează și numele, modul în care găsim soluțiile la aceste probleme
depinde în totalitate de circumstanțele care ni se dau de la problemă la
problemă. Cu toate acestea, deși problemele ad-hoc sunt în teorie complet
originale, putem să ne folosim de câteva principii care ne vor ajuta pentru a
facilita găsirea unor soluții pentru aceste probleme

Scopul pe care acest articol îl are este acela de a prezenta viziunea mea asupra
principiilor necesare pentru a găsi idei, abordări și soluții pentru problemele
ad-hoc, precum și de a prezenta diverse probleme, aplicând aceste principii. De
asemenea, un alt scop este acela de a unifica diversele resurse prezentate
anterior pe diverse site-uri cu scopul de a extinde baza de informații în ceea
ce privește acest tip de probleme prea puțin discutat și analizat într-o manieră
mai sistematică.

## O strategie de bază pentru abordarea problemelor ad-hoc

### Tratarea cazurilor mici

Un prim pas spre a rezolva cu succes probleme ad-hoc este acela de a găsi
soluții pentru exemple mici, lucru ce se poate face fie cu ajutorul hârtiei și a
analizei manuale a acelor exemple, fie cu ajutorul unui program ce poate genera
toate soluțiile posibile folosind un algoritm de tip brute force. De exemplu,
puteți folosi un program brute force pentru a genera toate permutările unui șir
de $n$ numere, toate submulțimile mulțimii ${1, 2, ..., n}$ și asa mai departe.

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n
    cin >> n;

    vector<int> v;

    for (int i = 1; i <= n; i++) {
        v.push_back(i);
    }

    // generarea permutarilor multimii 1, 2, .., n

    do{
       // procesam permutarea
    } while(next_permutation(v.begin(), v.end()));

    // generarea submultimilor multimii 1, 2, .., n
    // avem 2^n submultimi, fiecare dintre ele putand fi reprezentata folosind
    // o masca pe biti

    for (int i = 0; i < (1<<n); i++) {
        vector<int> subset;
        for (int j = 0; j < n; j++) {
            if (i & (1<<j)) {
                subset.push_back(j);
                // procesam acel numar ca parte a submultimii gasite
            }       
        }
    }
    return 0;
}
```

### Găsirea unor patternuri sau a unor relații între diverse valori din input

De multe ori, după ce generăm răspunsuri pentru valori mici folosind o metodă
sau alta, putem să observăm diferite relații sau patternuri în funcție de
răspunsurile pe care le obținem. De asemenea, ne putem folosi de anumite
particularități pe care anumite valori din restricții le au, lucruri ce se
întâlnesc des în problemele ad-hoc. \newline

Un exemplu concret este problema [asort](https://kilonova.ro/problems/1084) de
la barajul de juniori din 2016.

!!! note "Observație"

    Soluția acestei probleme este explicată în arhiva noastră
    [aici](../usor/simulating-solution.md#problema-exemplu-asort-baraj-2016-juniori)

### Folosiți-vă de diferite restricții speciale din enunț (dacă apar asemenea restricții)

De multe ori, pot apărea anumite restricții care sunt foarte particulare
raportat la problema în sine. De obicei, o parte importantă a soluției în acest
caz constă în observarea rolului din spatele acestor restricții.

Deși acesta nu este un sfat util doar pentru problemele ad-hoc, de multe ori
analizarea atentă a acestor părți din problemă poate ușura rezolvarea problemei
sau cel puțin a unor subprobleme care duc la soluția finală a problemelor.

### Presupunerea unor soluții rezonabile

Acest pas este unul ce depinde de la problemă la problemă, dar de obicei e
important să încercăm să reducem cât se poate aspectele variabile din problemă
(reducem intervalul posibil al soluției, ignorăm informații neimportante, tratăm
cazurile evidente etc.).

### Convinge-te singur că soluția găsită anterior este corectă (sau incorectă)

Acest ultim pas este de departe cel mai dificil deoarece de multe ori se poate
întâmpla una din cele două situații:

- soluția să pară corectă dar să apară greșeli de idee și/sau submisii care
  obtin 0 sau un scor foarte mic.
- soluția să fie corectă dar să fie foarte greu de găsit cel putin o intuiție
  dacă nu o demonstrație completă a acesteia.

Indiferent de caz, este important să ne asigurăm că soluția obținută nu obține
un răspuns greșit pe un caz evident, dar și să încercăm pe cât posibil să
trimitem soluția când algoritmul și implementarea au sens.

!!! warning "Atenție"

    Deși acesta este un sfat general, este foarte important să evitați să trimiteți
    soluția imediat ce găsiți un bug pe care l-ați rezolvat (cu excepția cazului
    când concursul se apropie de final și orice secundă contează) - pe lângă faptul
    că numărul de submisii este limitat, este de asemenea important să reușiti să
    rezolvați problema din mai puține submisii, deoarece din experiența personală,
    ajută în ceea ce privește încrederea pe care o căpătați pentru celelalte
    probleme în timpul concursului.

## Problemă exemplu - [Pretty Permutations](https://codeforces.com/contest/1541/problem/A)

Putem începe prin a găsi diverse răspunsuri pentru valori mici ale lui $n$.

- $n = 2$ - [$2, 1$]
- $n = 3$ - [$3, 1, 2$]
- $n = 4$ - [$2, 1, 4, 3$]
- $n = 5$ - [$3, 1, 2, 5, 4$]

Se poate observa că în cazul acestei probleme, este îndeajuns să găsim câteva
soluții pentru valori mici ale lui $n$ pentru a observa un pattern pentru
valorile pare și impare ale lui $n$. Astfel, pentru $n > 3$, putem pleca de la
soluția pentru $n - 2$ la care adăugăm perechea [$n, n-1$]. Această soluție este
una optimă deoarece fiecare element va fi situat la o distanță de 1 de poziția
lui, cu excepția lui 3 când $n$ e impar, care va fi situat la distanța 2.

Deși această problemă este una relativ facilă, procesul folosit pentru
rezolvarea ei va fi regăsit în multe alte probleme de acest gen în viitor.

## Problema [Yinyang (OJI 2019, clasa a X-a)](https://kilonova.ro/problems/650)

La o primă citire a enunțului, putem observa faptul că noi trebuie să sortăm
matricea folosind un algoritm de interschimbare a două linii sau coloane
consecutive. De aici, putem observa faptul că putem să verificăm rezultatele pe
care le-am obține folosind un asemenea algoritm.

Cei doi algoritmi de sortare care interschimbă valori adiacente la fiecare pas
sunt bubble sort și insertion sort, deci putem verifica cei doi algoritmi și
vedem care ar oferi rezultate mai optime. Dar oare este necesar?

### Generalizarea răspunsului pe o matrice

Știm deja de la lucrul cu vectori că numărul de inversiuni al unui vector este
echivalent cu numărul de operații de interschimbare pe care îl face algoritmul
bubble sort, ceea ce ne duce cu gândul la folosirea bubble sort pentru a calcula
numărul minim de interschimbări.

De asemenea, deoarece atunci când interschimbăm două linii (sau coloane),
valorile nu își schimbă ordinea pe coloană (sau pe linie), putem trata în mod
independent sortarea liniilor și a coloanelor pentru a obține răspunsul.

După găsirea acestor observații, putem să sortăm liniile, apoi coloanele
folosind bubble sort, iar tot ceea ce avem de făcut apoi este să verificăm
condițiile din enunț.

### Problema [Preparing Boxes (ABC 134)](https://atcoder.jp/contests/abc134/tasks/abc134_d)

Mai întâi, trebuie să ne gândim la informațiile pe care le putem afla mai ușor
cu privire la valorile din vectorul $b$. De exemplu, $b[1] \ \% \ 2$ = $(a[1] +
a[2] + \dots + a[n]) \ \% \ 2$. Totuși, $b[n] \ \% \ 2$ = $a[n] \ \% \ 2$.

De asemenea, numărul total de valori pe care trebuie să le verificăm este $O(n
\log n)$, o observație ce este destul de clară pentru oricine a implementat cel
puțin o dată ciurul lui Eratostene.

Din aceste observații, putem deduce faptul că este optim să fixăm valorile din
$b$ de la dreapta la stânga, iar parcurgerea lor folosind brute force este
îndeajuns de rapidă.

### Soluția finală

Din cele două observații de mai sus, putem deduce un algoritm pentru
implementarea soluției, care trece prin valori în ordine descrescătoare a
poziției, iar pentru fiecare valoare, putem să determinăm dacă va fi 0 sau 1
în functie de restul împărțirii la 2 al sumei valorilor relevante.

[Soluție care ia
accepted](https://atcoder.jp/contests/abc134/submissions/40928822)

## Problema Karte (COCI 2018, runda 5)

Pentru multe probleme de acest gen, o sortare a datelor este foarte utilă
deoarece ne ajută să vedem lucrurile într-o perspectivă diferită. În cazul
acestei probleme, e mai simplu să presupunem că valorile mai mici ne vor duce
mai ușor la răspunsul cerut, așa că vom sorta datele mai întâi.

După ce grupăm datele, vom crea o nouă listă în care vom pune mai întâi cele mai
mici $n - k$ valori în ordine descrescătoare, iar mai apoi punem celelalte
valori, tot în ordine descrescătoare.

În cele din urmă, tot ce trebuie să mai facem este să verificăm dacă numărul de
false ipoteze este exact $k$, conform enunțului.

[Soluție care ia punctaj maxim](https://oj.uz/submission/730646)

## Problema [Game (EJOI 2017)](https://csacademy.com/contest/archive/task/game/statement/)

În primul rând, este clar că fiecare jucător va vrea să ia elementul cu valoarea
maximă la fiecare pas. Acest lucru ne duce la o soluție relativ ușor de găsit
care implică un set (la fiecare pas luăm valoarea maximă și eventual adăugăm
valoarea nouă în set). Complexitatea este $O(n * k * \log n)$, ceea ce obține
50 de puncte. Cum putem îmbunătăți această soluție?

## Observații suplimentare

Deocamdată nu am folosit încă faptul că valorile din input nu sunt foarte mari
și putem să folosim un vector de frecvență pentru a simula procesul. Totuși,
acest lucru nu este îndeajuns pentru a obține punctajul maxim, deoarece nu am
stabilit încă ce facem cu elementele pe care le adăugăm pe parcurs.

Să analizăm ce se întâmplă la un pas al jocului. După ce știm valoarea maximă
inițială, avem două cazuri în ceea ce privește noua valoare adăugată.

- Fie e mai mare decât maximul curent, iar atunci jucătorul aflat la mutare va
  lua acea valoare.
- Altfel, acea valoare va fi adăugată în vectorul de frecvență, iar calculul va
  continua ca de obicei.

Astfel, complexitatea soluției va deveni $O(n \cdot k)$.

## Alte sfaturi practice

- Folosiți tot felul de idei și analizați de ce sunt incorecte
- Aveți grijă la cazurile limită și cazurile mici.
- Verificați de două ori soluțiile la cazurile pe care le rezolvați manual sau
  cu programul brute force
- Nu vă gândiți prea mult la diferite abordări foarte sofisticate, deoarece de
  multe ori aceste probleme au soluții mai simple decât par.

## Concluzii

În general, problemele ad-hoc (și nu numai) trebuie tratate plecând de la
informațiile pe care le avem, fără să forțăm folosirea anumitor tehnici sau
algoritmi decât dacă avem certitudinea că se pot face într-un anume mod. Astfel,
putem păstra o viziune deschisă asupra tehnicilor posibile pe care o problemă le
poate impune, iar sfaturile pe care le-am adresat aici fac și rezolvarea celor
mai atipice probleme posibilă, antrenamentul fiind cu atât mai esențial pentru
această clasă de probleme.

## Probleme suplimentare

- [OJI 2019 traseu](https://kilonova.ro/problems/915)
- [infoarena palind](https://www.infoarena.ro/problema/palind)
- [COCI 2018 karte](https://oj.uz/problem/view/COCI18_karte)
- [ONI 2008 aranjare](https://kilonova.ro/problems/1303)
- [CF 1815A](https://codeforces.com/contest/1815/problem/A)
- [CF 1797C](https://codeforces.com/contest/1797/problem/C)  
- [CF 1815B](https://codeforces.com/contest/1815/problem/B)
- [Baraj juniori 2016 asort](https://kilonova.ro/problems/1084)
- [IOI 2010 quality](https://oj.uz/problem/view/IOI10_quality)
- [COCI 2013 organizator](https://dmoj.ca/problem/coci13c1p5)
- [CF 1332B](https://codeforces.com/problemset/problem/1332/B)
- [ABC134 D](https://atcoder.jp/contests/abc134/tasks/abc134_d)
- [CF 1804B](https://codeforces.com/contest/1804/problem/B)
- [ARC158 A](https://atcoder.jp/contests/arc158/tasks/arc158_a)
- [ARC153 B](https://atcoder.jp/contests/arc153/tasks/arc153_b)
- [ARC145 B](https://atcoder.jp/contests/arc145/tasks/arc145_b)
- [CF 1407C](https://codeforces.com/problemset/problem/1407/C)
- [CF 1776F](https://codeforces.com/contest/1776/problem/F)
- [ARC118 C](https://atcoder.jp/contests/arc118/tasks/arc118_c)
- [CF 1776G](https://codeforces.com/contest/1776/problem/G)
- [ARC159 C](https://atcoder.jp/contests/arc159/tasks/arc159_c)
- [CF 1736D](https://codeforces.com/contest/1736/problem/D)
- [Probleme ad-hoc de pe kilonova](https://kilonova.ro/tags/297)

## Resurse suplimentare

- [Problemele ad-hoc - USACO Guide](https://usaco.guide/bronze/ad-hoc)
- [Abordarea problemelor ad-hoc (2021) -
  SEPI](https://sepi.ro/assets/upload-file/articole/Articolul%20educational%202020-2021.pdf)
- [Abordarea problemelor ad-hoc (2023) -
  SEPI](https://sepi.ro/assets/upload-file/articole/curs%20ad-hoc.pdf)
- [Probleme constructive si interactive -
  HKOI](https://assets.hkoi.org/training2022/cast.pdf)
- [How to get started with solving Ad-Hoc tasks on codeforces -
  Codeforces](https://codeforces.com/blog/entry/85172)
- [Tips on Constructive Algorithms -
  Codeforces](https://codeforces.com/blog/entry/80317)
- [Good ad-hoc problems - Codeforces](https://codeforces.com/blog/entry/61672)
- [How to prove your solutions in Competitive Programming -
  Codeforces](https://codeforces.com/blog/entry/111849)
- [How to come up with solutions? -
  Codeforces](https://codeforces.com/blog/entry/20548)
- [On "is this greedy or DP", forcing and rubber bands -
  Codeforces](https://codeforces.com/blog/entry/106346)
