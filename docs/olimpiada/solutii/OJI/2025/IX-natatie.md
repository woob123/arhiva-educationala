---
id: OJI-2025-IX-natatie
title: Soluția problemei Natație (OJI 2025, clasa a IX-a)
problem_id: 3629
authors: [ignat]
prerequisites:
    - simulating-solution
    - sorting
tags:
    - OJI
    - clasa IX
---

!!! note "Observație"

    Pentru soluțiile ce folosesc sortări, complexitatea pentru fiecare subtask
    în parte depinde de sortarea folosită, însă orice sortare cu complexitate
    cel mult pătratică $\mathcal{O} (N^2)$ obține punctaj maxim pentru
    restricțiile date. Soluțiile prezentate nu iau în calcul și această
    complexitate, deși aceasta poate fi mai semnificativă decât complexitatea
    soluției propriu-zise în care se află timpul minim al cursei.

## Subtask 1 (17p)

Dacă toate vitezele sunt egale, putem alege oricare $M$ rațe din cele $N$. Toate
configurațiile în care se respectă ordinea crescătoare a rezistențelor sunt
valide și dau același timp pentru cursă. Răspunsul va fi $(2 \cdot d_M )/v$,
unde $v$ reprezintă o viteză a oricărei rațe. Complexitate: $\mathcal{O} (1)$.

## Subtask 2 (16p)

Dacă toate rezistențele sunt egale, cea mai optimă variantă este să luăm cele
$M$ rațe cu cele mai mari viteze. Putem sorta rațele crescător după viteză
punându-le în ordinea crescătoare a vitezelor pe culoarele de la 1 la $M$. Apoi,
simulăm cursa, răspunsul fiind cel mai mare timp în care o rață se întoarce.
Complexitate: $\mathcal{O}(M)$.

## Subtask 3 (15p)

Din moment ce avem cel mult $3 \cdot 10^3$ rațe și trebuie să folosim de fiecare
dată $M$ dintre cele $N$ și $M = N − 1$, înseamnă că doar o rață nu va fi
folosită. Deci, putem să eliminăm fiecare rață în parte și să simulăm cursa cu
cele rămase. Observația cheie este că pentru o configurație aleasă cu $M$ rațe,
cea mai optimă așezare a lor pe cele $M$ culoare este cea în care le sortăm
crescător după rezistență, iar în caz de egalitate crescător după viteză. Se
sortează inițial toate rațele după acest criteriu, eliminăm fiecare în parte și
simulăm cursa cu cele rămase, determinând cel mai bun timp dintre curse.
Complexitate: $\mathcal{O}(M \cdot N)$.

## Subtask 4 (18p)

O primă observație este faptul că răspunsul aparține mulțimii $\{t \mid t = (2
\cdot d_j )/v_i,\,1 ≤ i ≤ N,\,1 ≤ j ≤ M\}$. Pe scurt, timpul cursei optime este
cu siguranță unul dintre timpii unei perechi (rață, baliză). Putem calcula toți
timpii posibili, îl luăm pe fiecare în parte, iar apoi verificăm dacă există
vreo configurație prin care să alegem $M$ rațe și să obținem un timp cel mult
egal cu cel ales. În continuare, trebuie să găsim o strategie prin care putem
determina dacă $M$ dintre cele $N$ rațe pot fi alese încât să obținem un timp
mai mic sau egal decât un timp fixat. În primă fază, toate rațele se sortează
după rezistență, iar în caz de egalitate după viteză. Apoi, parcurgem în ordine
toate rațele, încercând la fiecare pas să punem rața curentă pe cel mai mic
culoar neocupat. Acest lucru este corect deoarece dacă o rață cu rezistența $r$
este folosită pe un culoar, rațele cu rezistențe mai mici nu mai pot fi
folosite, deci este optim să o luăm pe cea cu rezistență minimă dacă se întoarce
în timp util. Legat de viteză (la egalitate de rezistențe), este clar că dacă o
rață cu viteza $v$ se întoarce înapoi în timp util, este optim să păstrăm rațele
cu viteze mai mari pentru culoarele cu balize mai îndepărtate, deci sortarea și
simularea cursei sunt corecte. Dacă găsim $M$ rațe care ne obțin un timp mai mic
sau egal decât cel fixat, reținem acel timp, la final răspunsul fiind cel mai
mic astfel de timp. Complexitate: $\mathcal{O} (M \cdot N^2)$.

## Subtask 5 (11p)

Din moment ce distanțele balizelor sunt cel mult $3 \cdot 10^3$ și vitezele sunt
numere naturale, înseamnă că timpul maxim al unei curse este $6 \cdot 10^3$.
Știind că rezultatul este un timp întreg și folosind ideea anterioară, putem lua
toți timpii naturali în ordine crescătoare și să verificăm pentru fiecare în
parte dacă există o cursă posibilă. Primul timp găsit este răspunsul căutat.
Complexitate: $\mathcal{O} (N \cdot d_M)$.

## Subtask 6 (13p)

O observație cheie este că dacă pentru un timp $x$ nu reușim (folosind ideile
anterioare) să alegem $M$ rațe astfel încât să obținem un timp mai mic sau egal
pentru cursa noastră, înseamnă că nu vom reuși nici pentru alți timpi mai mici.
Deci, putem face o căutare binară pe rezultat, în care luăm la fiecare pas
mijlocul ca fiind timpul maxim pe care îl poate avea cursa noastră. Încercăm să
căutăm o configurație de $M$ rațe prin care timpul cursei este mai mic sau egal
decât cel fixat. Dacă găsim, atunci vom căuta un timp mai mic sau egal (dr =
mij). Dacă nu, atunci avem nevoie de un timp strict mai mare (st = mij +
1). La finalul căutării binare, timpul găsit este chiar răspunsul.
Complexitate: $\mathcal{O} (N \cdot \log d_M )$.

## Subtask 7 (10p)

Dacă rezultatul nu este natural, ideea anterioară este validă în continuare,
însă trebuie modificată căutarea binară. Din moment ce trebuie să afișăm
rezultatul cu o eroare maximă de $10^{−3}$, vom căuta binar răspunsul până când
diferența dintre capete este mai mică decât această eroare. Deoarece căutăm
binar pe numere reale, se modifică atribuirile pentru capetele din stânga și
dreapta. Dacă găsim o configurație, atunci vom căuta un timp mai mic sau egal
(dr = mij). Dacă nu, atunci avem nevoie de un timp strict mai mare, însă
căutarea fiind pe numere reale, atribuirea e asemănătoare cu cea din cazul
anterior (st = mij). Pentru un punctaj maxim este necesară și afișarea cu
precizie, astfel încât să se afișeze cel puțin 3 cifre zecimale. Complexitate:
$\mathcal{O} (N \cdot \log d_M)$.

## Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <algorithm>
#include <fstream>
#include <iomanip>
#define lol long long
#define N 3001
using namespace std;
ifstream f("natatie.in");
ofstream g("natatie.out");
int m, n, i;
long double rez;
int v[N], r[N], ind[N], d[N];
long long K, sol;

bool cmp(int st, int dr) {
    if (r[st] == r[dr]) {
        return v[st] < v[dr];
    }
    return r[st] < r[dr];
}

void solve(lol st, lol dr) {
    if (st < dr) {
        lol mij = (st + dr) / 2;
        int nr = 0, h, j, b = 1;
        long double t, t1;

        for (h = 1; h <= n; h++) {
            j = ind[h];
            if (b <= m) {
                t = (mij * 1.0) / 1000000000;
                t1 = (2.0 * d[b]) / v[j];
                if (t1 <= t) {
                    nr++;
                    b++;
                }
            } else {
                h = n + 1;
            }
        }
        if (b == m + 1) {
            if (mij <= rez) {
                rez = mij;
            }
            solve(st, mij);
        } else {
            solve(mij + 1, dr);
        }
    }
}

int main() {
    K = 1000000000000000;
    f >> n >> m;
    for (i = 1; i <= n; i++) {
        f >> v[i];
        ind[i] = i;
    }
    for (i = 1; i <= n; i++) {
        f >> r[i];
    }
    for (i = 1; i <= m; i++) {
        f >> d[i];
    }
    sort(ind + 1, ind + n + 1, cmp);
    rez = K;
    solve(0, K);
    rez = (rez * 1.0) / 1000000000;
    g << setprecision(9) << rez << "\n";

    return 0;
}
```
