---
id: OJI-2014-V-piramide
title: Soluția problemei piramide (OJI 2014, clasa a V-a)
problem_id: 843
authors:
    - traian
# prerequisites:
#    - digits-manipulation
tags:
    - OJI
    - clasa V
---

## Cerința 1

Observăm că a $i$-a piramidă are $i+1$ nivele, adică are $1 + 2 + .. + (i + 1) = \frac{(i + 1) \cdot (i + 2)}{2}$ elemente.

Prima priamidă are de la $1$ la $\frac{2 \cdot 3}{2}$, a doua de la $\frac{2 \cdot 3}{2} + 1$ la $\frac{3 \cdot 4}{2}$, și așa mai departe.

Putem să căutăm piramida brut, mereu adunând câte $\frac{i \cdot (i + 1)}{2}$ la numărul de cartonașe. Totuși, trebuie să verificăm dacă nu cumva am trecut peste limita de $n$ cartonașe.

## Cerința 2

Procedăm similar cu ce am făcut la cerința $1$. Am putea să căutăm pe ce piramidă s-ar afla cartonașul $n$ și să scădem eventual $1$ din acest număr, dacă nu va fi completă.

## Cerința 3

Vom menține numărul total de cartonașe folosite în algoritmul de la cerința $2$ și vom scădea $n$ din acest număr.

## Cerința 4

Din moment ce $1 \leq c_1 \lt c_2 \lt ... \lt c_k \leq n$, înseamnă că indicele piramidelor din care fac parte cartonașele albe vor fi crescătoare.

Dacă la elementul anterior am procesat până la piramida $i$, atunci putem începe procesarea direct de la $i$, din nou, deoarece elementele sunt strict crescătoare.

La final, trebuie doar să vedem numărul maxim de indici de piramide egali. Din moment ce acești indici sunt crescători, atunci putem să vedem numărul maxim de astfel de indici egali pe poziții consecutive. Pentru mai multe detalii vedeți codul sursă.

## Cod sursă

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <fstream>

using namespace std;

ifstream fin("piramide.in");
ofstream fout("piramide.out");

signed main() {
    int n, x, k, i = 2, pi, cnt = 0, maxcnt = 0, element = 0;
    long long s = 0;
    fin >> n >> x >> k;

    while (s + 1LL * i * (i + 1) / 2 < x) {
        s += 1LL * i * (i + 1) / 2;
        i++;
    }
    if (s + 1LL * (i - 1) * i / 2 <= n) {
        fout << i - 1 << '\n';
    } else {
        fout << "0\n";
    }

    i = 2;
    s = 0;
    while (s + 1LL * i * (i + 1) / 2 < n) {
        s += 1LL * i * (i + 1) / 2;
        i++;
    }
    fout << i - 2 << '\n' << n - s << '\n';

    int aux = i - 2;
    i = pi = 2;
    s = 0;
    for (int j = 1; j <= k; j++) {
        fin >> x;

        while (s + 1LL * i * (i + 1) / 2 < x) {
            s += 1LL * i * (i + 1) / 2;
            i++;
        }
        if (i - 1 > aux) {
            break;
        }

        if (i != pi) {
            cnt = 1;
        } else {
            cnt++;
        }
        if (cnt > maxcnt) {
            maxcnt = cnt;
            element = i - 1;
        }

        pi = i;
    }

    fout << element;

    return 0;
}
```
