---
id: OJI-2025-VII-teren
title: Soluția problemei teren (OJI 2025, clasa a VII-a)
problem_id: 3640
authors: [dpopa]
prerequisites:
    - matrices
    - partial-sums
tags:
    - OJI
    - clasa VII
---

## Cerința 1

Se citesc coordonatele de început ale însămânțării $L_1$, $C_1$ și coordonatele
de sfârșit $L_2$, $C_2$. Dacă $L_1 = L_2$ la numărul de semințe aruncate se
adună $1 + |C_1-C_2|$, altfel se adună $1 + |L_1-L_2|$.

## Precalculări pentru cerința 2 și 3

Atât pentru cerința 2 cât și pentru cerința 3 se folosesc 5 matrici: matricea
$o$ pentru parcurgerile orizontale, $v$ pentru cele verticale, $dp$ pentru cele
paralele cu diagonala principală, $ds$ pentru parcurgerile paralele cu diagonala
secundară. Pentru fiecare zbor/parcurgere se aplică [difference
array](../../../../usor/partial-sums.md/#smenul-lui-mars). Deoarece parcurgerea
este liniară difference array se aplică exact ca la vectori: Pentru parcurgerile
orizontale (unde $L_1 = L_2$):

```cpp
o[L1][min(C1, C2)]++;
o[L1][max(C1, C2) + 1]--;
```

Pentru parcurgerile verticale (unde $C_1 = C_2$):

```cpp
v[min(L1, L2)][C1]++;
v[max(L1, L2) + 1][C1]--;
```

Pentru parcurgerile paralele cu diagonala secundară (unde $L_1 + C_1 = L_2 +
C_2$): se interschimbă capetele a.î. $L_1 < L_2$ și apoi:

```cpp
ds[L1][C1]++;
ds[L2 + 1][C2 - 1]--;
```

Pentru parcurgerile paralele cu diagonala principală: se interschimbă capetele
a.î. $L_1 < L_2$ și apoi:

```cpp
dp[L1][C1]++;
dp[L2 + 1][C2 + 1]--;
```

Se parcurg matricele și se fac adunările corespunzătoare:

```cpp
o[i][j] += o[i][j - 1];
v[i][j] += v[i - 1][j];
dp[i][j] += dp[i - 1][j - 1];
ds[i][j] += ds[i - 1][j + 1];
```

Într-o matrice rezultat se marchează acele celule care sunt nenule în cel puțin
una din matricele anterioare.

## Cerința 2

Pentru obținerea rezultatului se numără câte valori nenule sunt în matricea rezultat.

## Cerința 3

Pentru a obține rezultatul se numără pentru fiecare celulă nenulă câți vecini au
valoarea 0.

## Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
// prof Popa Daniel, Colegiul Național "Aurel Vlaicu", Orăștie
#include <cmath>
#include <fstream>
#include <iostream>
using namespace std;
ifstream fin("teren.in");
ofstream fout("teren.out");
const int nm = 1002;
int o[nm][nm], v[nm][nm], dp[nm][nm], ds[nm][nm], r[nm][nm];
int n, m, k, i, j, L1, C1, L2, C2, c;
void afis(int a[nm][nm]) {
    for (int i = 1; i <= n; i++) {
        for (j = 1; j <= n; j++) {
            cout << a[i][j] << " ";
        }
        cout << endl;
    }
    cout << endl;
}
void cerinta1() {
    int sol = 0;
    for (int i = 1; i <= m; i++) {
        fin >> L1 >> C1 >> L2 >> C2;
        if (L1 == L2) {
            sol += abs(C1 - C2) + 1;
        } else {
            sol += abs(L1 - L2) + 1;
        }
    }
    fout << sol;
}
void pentruCerinta2_3() {
    for (int i = 1; i <= m; i++) {
        fin >> L1 >> C1 >> L2 >> C2;
        if (L1 == L2) {
            o[L1][min(C1, C2)]++;
            o[L1][max(C1, C2) + 1]--;
        } else if (C1 == C2) {
            v[min(L1, L2)][C1]++;
            v[max(L1, L2) + 1][C1]--;
        } else if (L1 + C1 == L2 + C2)  /// diagonala secundara
        {
            if (L1 > L2) {
                swap(L1, L2);
                swap(C1, C2);
            }
            ds[L1][C1]++;
            ds[L2 + 1][C2 - 1]--;
        } else  /// diagonala principala
        {
            if (L1 > L2) {
                swap(L1, L2);
                swap(C1, C2);
            }
            dp[L1][C1]++;
            dp[L2 + 1][C2 + 1]--;
        }
    }
    for (i = 1; i <= n; i++) {
        for (j = 1; j <= n; j++) {
            o[i][j] += o[i][j - 1];
            v[i][j] += v[i - 1][j];
            dp[i][j] += dp[i - 1][j - 1];
            ds[i][j] += ds[i - 1][j + 1];
            r[i][j] = (o[i][j] + v[i][j] + dp[i][j] + ds[i][j]) > 0;
        }
    }
    // afis(r);
}
void cerinta2() {
    int sol = 0;
    for (int i = 1; i <= n; i++) {
        for (j = 1; j <= n; j++) {
            if (r[i][j] > 0) {
                sol++;
            }
        }
    }
    fout << sol;
}

void cerinta3() {
    int sol = 0, k;
    fin >> k;
    for (int i = 1; i <= n; i++) {
        for (j = 1; j <= n; j++) {
            if (r[i][j] != 0) {
                sol += (r[i - 1][j] == 0) + (r[i][j - 1] == 0)
                     + (r[i + 1][j] == 0) + (r[i][j + 1] == 0);
            }
        }
    }
    fout << sol;
}

int main() {
    fin >> c >> n >> m;
    if (c == 1) {
        cerinta1();
    } else {
        pentruCerinta2_3();
        if (c == 2) {
            cerinta2();
        }
        if (c == 3) {
            cerinta3();
        }
    }

    return 0;
}
```
