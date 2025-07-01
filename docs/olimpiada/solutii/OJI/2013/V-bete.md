---
id: OJI-2013-V-bete
title: Soluția problemei bete (OJI 2013, clasa a V-a)
problem_id: 831
authors:
    - traian
# prerequisites:
#    - placeholder
tags:
    - OJI
    - clasa V
---

## Cerința 1

La început, suma totală era $l \cdot n$, unde $l$ era lungimea inițială. Rezultă că $l$ este suma totală împărțită la $n$.

## Cerința 2

Lungimea celui mai lung băț este egală cu suma lungimilor maxime din ambele șiruri.

## Cerința 3

Suma lungimilor maxime din ambele șiruri se poate obține doar alegând câte un maxim din fiecare șir. Vom calcula câte maxime sunt în fiecare șir, iar răspunsul nostru va fi minimul dintre cele două numere.

## Cod sursă

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <fstream>

using namespace std;

ifstream fin("bete.in");
ofstream fout("bete.out");

int main() {
    int n, suma = 0;
    fin >> n;
    
    int max1 = 0, cnt1 = 0;
    for (int i = 1; i <= n; i++) {
        int bat;
        fin >> bat;
        if (bat > max1) {
            max1 = bat;
            cnt1 = 1;
        } else if (bat == max1) {
            cnt1++;
        }
        suma += bat;
    }
    
    int max2 = 0, cnt2 = 0;
    for (int i = 1; i <= n; i++) {
        int bat;
        fin >> bat;
        if (bat > max2) {
            max2 = bat;
            cnt2 = 1;
        } else if (bat == max2) {
            cnt2++;
        }
        suma += bat;
    }
    
    int minim = cnt1;
    if (cnt2 < minim) {
        minim = cnt2;
    }
    fout << suma / n << "\n" << max1 + max2 << "\n" << minim << "\n";
    return 0;
}
```
