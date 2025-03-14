---
id: OJI-2002-V-balaur
title: Soluția problemei balaur (OJI 2002, clasa a V-a)
problem_id: 701
authors:
    - stefdasca
prerequisites:
    - basic-math
tags:
    - OJI
    - clasa V
---


Această problemă poate fi rezolvată observând modul în care evoluează numărul de
capete al balaurului.

- după prima zi, avem 5 capete (erau 6 iar Făt-Frumos taie unul din ele).
- după cea de-a doua zi, balaurul va ajunge la 11 capete iar Făt-Frumos taie
  unul din ele, deci rămân 10.

Se poate observa că în fiecare zi, balaurul va avea 5 capete mai mult decât în
ziua precedentă, deci putem defini răspunsul ca fiind egal cu $5 \cdot n$, unde
$n$ este numărul de zile dat în datele de intrare.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <fstream>
using namespace std;

ifstream cin("balaur.in");
ofstream cout("balaur.out");

int main() {
    int n;
    cin >> n;

    cout << 5 * n << '\n';
    return 0;
}
```
