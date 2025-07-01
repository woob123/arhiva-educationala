---
id: OJI-2010-V-tren
title: Soluția problemei tren (OJI 2010, clasa a V-a)
problem_id: 796
authors: [sichim]
prerequisites:
    - simulating-solution
tags:
    - OJI
    - clasa V
---

Articolul va fi disponibil curând în arhivă.

Până atunci, editorialul poate fi accesat în repo-ul nostru de GitHub, linkul fiind [acesta](https://github.com/roalgo-discord/Romanian-Olympiad-Solutions/blob/main/OJI%20(regional%20olympiad)/2010/05/tren.pdf).

## Rezolvare

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <fstream>
using namespace std;
ifstream fi("tren.in");
ofstream fo("tren.out");
int x, t, y, z, u, t1, l, h, m, s, i, j, vine, pleaca, g[1441], v[101], inc = 1441, sf;
int main() {
    fi >> t;
    for (i = 1; i <= t; i++) {
        fi >> l >> h >> m >> s;
        vine = h * 60 + m;
        pleaca = vine + s;
        if (i == 1) 
            inc = vine;
        if (sf < pleaca) 
            sf = pleaca;
        if (l == 1) {
            t1++;
            for (j = vine; j <= pleaca; j++) 
                g[j] = i;
        } else
            for (j = vine; j <= pleaca; j++)
                if (g[j] == 0) 
                    g[j] = i;
        // trenul de pe linia 2 este vizibil numai daca nu avem tren pe linia 1
    }
    z = (t1 > t - t1) ? t1 : t - t1;  // numărul maxim de trenuri de pe o linie
    y = 0;
    for (i = inc; i <= sf; i++) {
        v[g[i]] = 1;         // trenul care se află în momentul i în gară este vizibil
        if (g[i] == 0) u++;  // u=numărul de minute consecutive în care
        // ambele linii sunt libere
        else {
            if (u > y) y = u;
            u = 0;
        }
    }
    x = 0;
    for (i = 1; i <= t; i++) 
        x = x + v[i];
    fo << z << ' ' << x << ' ' << y;
    return 0;
}
```
