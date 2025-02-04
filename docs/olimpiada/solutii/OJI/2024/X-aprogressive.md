---
tags:
    - OJI
    - clasa X
---

# Soluția problemei Aprogressive (OJI 2024, clasa a X-a)

!!! example "Cunoștințe necesare"
    - [Simularea soluției](../../../../usor/simulating-solution.md)
    - [Tehnica divide et impera](../../../../mediu/divide-et-impera.md)

**Autor soluție**: Gheorghe-Eugen Nodea

!!! note "Link problemă"
    Această problemă poate fi accesată [aici](https://kilonova.ro/problems/2504/).

## Cerința 1

Pentru fiecare linie a matricei $T$ se calculează suma pe linie prin adunarea elementelor aflate pe aceasta. Sumele obținute pentru fiecare dintre liniile acestei matrice formează termenii unui șir, numit șirul sumelor pe linii $S_i$ cu $1 \leq i \leq n$. Se determină pentru șirul $S$ valoarea maximă. La final, se afisează în ordine strict crescătoare indicii liniilor matricei pentru care suma $S_i$ este maximă. 

## Cerința 2

Vom privi linia unei matrici ca un șir (vector) cu $m$ elemente. Cum verificăm dacă elementele șirului pot fi reanjate astfel încât să formeze o progresie aritmetică?

- Soluția 1, brute-force $O(m^2)$ - Se determină cele mai mici două elemente din șir $Min_1$, respectiv $Min_2$, cu $Min_1 \leq Min_2$. Dacă șirul poate fi rearanjat pentru a forma o progresie aritmetică, atunci rația progresiei este $r = Min_2 − Min_1$. Dacă rația este nenulă, atunci se caută a treia cea ma mică valoare, a patra cea mai mică,
și așa mai departe. Pentru cea mai mică a $i$-a valoare din șir se verifică dacă diferența între valoarea minimă curentă și valoarea minimă determinată anterior este egală cu rația. În caz contrar, șirul nu poate fi reanjat astfel încât să formeze o progresie aritmetică.

- Soluția 2, $O(m \log m)$ -  Se sortează elementele șirului crescător. Rația progresiei este dată de diferența dintre al doilea element și primul element al șirului sortat. Se verifică dacă diferențele dintre elementele consecutive din șir se păstrează și este egală cu rația. În funcție de metoda de sortare folosită se pot obține punctaje diferite. Se afisează în ordine strict crescătoare indicii liniilor matricei pentru care elementele pot fi rearanjate astfel încât să formeze pe linia respectivă o progresie aritmetică de rație nenulă.

- Soluția 3, $O(m)$ - Se determină cele mai mici două elemente din șir $Min_1$, respectiv $Min_2$, cu $Min_1 \leq Min_2$. Calculăm rația progresiei $r = Min_2 − Min_1$. Se scade din toate elementele șirului valoarea $Min_1$. Acum ar trebui ca toate elementele obținute să se se dividă cu rația $r$, caz în care vom simplifica fiecare element cu $r$. Se obține prin simplificare un șir cu valori distincte din mulțimea $\{0, \dots, m−1\}$ pentru a putea fi reanjat să formeze o progresie aritmetică. Putem verifica această condiție folosind un vector de apariții.

## Cerința 3

Pentru împărțirea în submatrici se folosește un algoritm recursiv (divide et impera). Fie submatricea $R$ cu colțul stânga-sus $(x_1, y_1)$ și colțul dreapta-jos $(x_2, y_2)$.

- Submatricea $A$ are colțul stânga-sus în $(x_1, y_1)$, iar colțul dreapta-jos în $((x_1 + x_2)/2, (y_1 + y_2)/2)$.
- Submatricea $B$ are colțul stânga-sus în $(x_1, (y_1 + y_2)/2 + 1)$, iar colțul dreapta-jos în $((x_1 + x_2)/2, y_2)$.
- Submatricea $C$ are colțul stânga-sus în $((x_1 +x_2)/2+1, y_1)$, iar colțul dreapta-jos în $(x_2, (y_1 + y_2)/2)$.
- Submatricea $D$ are colțul stânga-sus în $((x_1 + x_2)/2+1, (y_1 + y_2)/2+1)$, iar colțul dreapta jos în $(x_2, y_2)$.

Pentru determinarea șirului $S$ al unei submatrice $R$ se precalculează o matrice a sumelor parțiale pe linie. Se procedează similar ca la cerința 2 pentru a verifica dacă o submatrice $R$ este aprogressive.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <algorithm>
#include <fstream>
#include <vector>

using namespace std;

ifstream cin("aprogressive.in");
ofstream cout("aprogressive.out");

long long c, n, m, a[1032][1032], sp[1032][1032];

long long ps(int xa, int ya, int xb, int yb) {
    return sp[xb][yb] - sp[xa - 1][yb] - sp[xb][ya - 1] + sp[xa - 1][ya - 1];
}

void solve(int xa, int ya, int xb, int yb) {
    cout << "(";
    if (xa == xb || ya == yb) {
        cout << xa << "," << ya << "," << xb << "," << yb << "," << 0;
    } else {
        vector<int> sum;
        for (int i = xa; i <= xb; i++)
            sum.push_back(ps(i, ya, i, yb));
        sort(sum.begin(), sum.end());
        int ratio = sum[1] - sum[0];
        bool ok = 1;
        for (int i = 2; i < sum.size(); i++)
            if (sum[i] - sum[i - 1] != ratio)
                ok = 0;
        if (ok && ratio != 0)
            cout << xa << "," << ya << "," << xb << "," << yb << "," << ratio;
        else {
            int midx = (xa + xb) / 2;
            int midy = (ya + yb) / 2;
            solve(xa, ya, midx, midy);
            solve(xa, midy + 1, midx, yb);
            solve(midx + 1, ya, xb, midy);
            solve(midx + 1, midy + 1, xb, yb);
        }
    }
    cout << ")";
}
int main() {

    cin >> c >> n >> m;

    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++) {
            cin >> a[i][j];
            sp[i][j] = sp[i - 1][j] + sp[i][j - 1] - sp[i - 1][j - 1] + a[i][j];
        }

    if (c == 1) {
        vector<int> maxlines;
        maxlines.push_back(1);
        for (int i = 2; i <= n; i++)
            if (ps(i, 1, i, m) > ps(maxlines[0], 1, maxlines[0], m))
                maxlines.clear(), maxlines.push_back(i);
            else if (ps(i, 1, i, m) == ps(maxlines[0], 1, maxlines[0], m))
                maxlines.push_back(i);
        for (auto x : maxlines)
            cout << x << '\n';
    }

    if (c == 2) {
        for (int i = 1; i <= n; i++) {
            vector<int> vals;
            for (int j = 1; j <= m; j++)
                vals.push_back(a[i][j]);
            sort(vals.begin(), vals.end());
            bool ok = 1;
            if (m >= 2) {
                int ratie = vals[1] - vals[0];
                for (int j = 2; j < vals.size(); j++)
                    if (vals[j] - vals[j - 1] != ratie)
                        ok = 0;
            }
            if (ok)
                cout << i << '\n';
        }
    }

    if (c == 3) {
        solve(1, 1, n, m);
    }
    return 0;
}
```