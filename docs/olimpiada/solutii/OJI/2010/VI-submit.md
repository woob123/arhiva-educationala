---
id: OJI-2010-VI-submit
title: Soluția problemei submit (OJI 2010, clasa a VI-a)
problem_id: 798
authors: [cerchez]
prerequisites:
    - simulating-solution
tags:
    - OJI
    - clasa VI
---

Articolul va fi disponibil curând în arhivă.

Până atunci, editorialul poate fi accesat în repo-ul nostru de GitHub, linkul fiind [acesta](https://github.com/roalgo-discord/Romanian-Olympiad-Solutions/blob/main/OJI%20(regional%20olympiad)/2010/06/submit.pdf).

## Rezolvare

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <algorithm>
#include <fstream>
using namespace std;
ifstream f("submit.in");
ofstream g("submit.out");

int n, pct[101], bonus, m, p, i, j, complet, crt, maxim;
int main() {
    f >> n;
    for (i = 1; i <= n; i++)
        f >> pct[i];
    f >> bonus;
    f >> m;
    maxim = 0;
    for (j = 1; j <= m; j++) {
        complet = 1;
        crt = 0;
        for (i = 1; i <= n; i++) {
            f >> p;
            if (p == 0)
                complet = 0;
            else
                crt += pct[i];
        }
        if (complet) 
            crt += bonus;
        crt -= 2 * (j - 1);
        if (crt > maxim) 
            maxim = crt;
    }
    g << maxim;
    return 0;
}
```
