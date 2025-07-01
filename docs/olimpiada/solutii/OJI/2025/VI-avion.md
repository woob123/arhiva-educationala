---
id: OJI-2025-VI-avion
title: Soluția problemei avion (OJI 2025, clasa a VI-a)
problem_id: 3637
authors: [marinel]
prerequisites:
    - basic-math
tags:
    - OJI
    - clasa VI
---

## Cerința 1 - 50p

Având în vedere că numărul de rânduri $R$ este par, există același număr de
rânduri în fiecare jumătate a avionului. Este suficient să fie comparat cu
$R/2$ rândul de pe biletul fiecărui pasager. Dacă rândul $r$ este în prima
jumătate a avionului ($r ≤ R/2$) se va afișa 1, altfel se va afișa 2.

## Cerința 2 - 50p

Pentru fiecare pasager se va compara și de această dată rândul de pe bilet cu
mijlocul avionului ($R/2$). Dacă rândul $r ≤ R/2$ și pasagerul urcă în avion
pe scara 1, la suma totală se va adăuga rândul de pe bilet, iar în caz contrar,
când pasagerul intră pe scara 2, el va parcurge până la rândul de pe bilet
distanța $R − r + 1$, care se va adăuga la suma totală. Apoi, pasagerul va mai
parcurge 1, 2 sau 3 metri în funcție de locul ocupat pe rândul de pe bilet: 1
metru dacă locul este 3 sau 4, 2 metri dacă locul este 2 sau 5 și 3 metri dacă
locul este 1 sau 6. Aceste valori se adaugă și ele la suma totală. Având în
vedere că fiecare dintre cei $n$ pasageri parcurge 3 metri de la intrarea în
avion până la culoarul central, la suma totală se mai adaugă $3 \cdot n$ metri.

## Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
/// Pracsiu Dan
#include <bits/stdc++.h>
using namespace std;

ifstream fin("avion.in");
ofstream fout("avion.out");

int a[2001];

int main() {
    int total, task, NR, n, i, x, y;
    fin >> task >> NR >> n;
    if (task == 1) {
        for (i = 1; i <= n; i++) {
            fin >> x >> y;
            if (x <= NR / 2) {
                a[i] = 1;
            } else {
                a[i] = 2;
            }
        }
        for (i = 1; i <= n; i++) {
            fout << a[i] << "\n";
        }
    } else {
        total = 0;
        for (i = 1; i <= n; i++) {
            fin >> x >> y;
            if (x <= NR / 2) {
                total += (3 + x);
            } else {
                total += (3 + (NR - x + 1));
            }
            if (y <= 3) {
                total += (4 - y);
            } else {
                total += (y - 3);
            }
        }
        fout << total << "\n";
    }
    return 0;
}
```
