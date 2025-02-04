---
tags:
    - OJI
    - clasa X
---

# Soluția problemei Poseidon (OJI 2024, clasa a X-a)

!!! example "Cunoștințe necesare"
    - [Algoritmul lui Lee. Flood Fill](../../../../mediu/lee.md)
    - [Introducere în STL](../../../../cppintro/stl.md)
    - [Introducere în combinatorică](../../../../mediu/intro-combinatorics.md)

**Autor soluție**: Ioana Gabor

!!! note "Link problemă"
    Această problemă poate fi accesată [aici](https://kilonova.ro/problems/2506/).

## Cerința 1

Subtask 1. Cazul unei singure insule.

Se va afișa numărul de valori diferite de 0 și $-1$ din matrice.

Subtask 2. Cazul $N = 1$.

Pentru acest caz, se poate parcurge șirul către stânga, începând cu celula de start, până la întâlnirea primei valori de $-1$, apoi către dreapta, într-un mod similar. Se vor număra valorile diferite de 0 și $-1$.

Subtask 3. Restul punctajului.

Pentru restul punctajului, se va aplica algoritmul de flood fill, pornind din celula de start.

## Cerința 2

Se vor procesa individual toate insulele din matrice, folosind algoritmul de flood fill,
și se vor număra comorile din fiecare. Notăm cu $D(n)$ numărul de permutări de $n$ elemente, fără puncte fixe. Rezultatul va consta în produsul valorilor $D(x_i)$, unde $x_i$ este numărul de comori de pe o insulă. Diferențele de punctaj sunt date de modul de calcul al valorilor $D(n)$. Notăm cu $nc$ numărul maxim de comori dintr-o insulă.

Subtask 4. Cazul $nc \leq 4$.

Se pot calcula “pe hârtie” valorile $D(x_i)$, fiind numere mici.

Subtask 5. Cazul $nc \leq 8$.

Se pot genera toate permutările de câte $k$ elemente, $k \leq nc$ care respectă condiția dată, cu ajutorul metodei backtracking.

Subtask 6. Restul punctajului.

Este necesară calcularea șirului $D$ într-un mod mai eficient. Pentru ilustrarea metodei, ne imaginăm că vrem să “dezordonăm” șirul format din elementele $1, 2, 3, \dots, n$, în această ordine. Pe fiecare poziție, se poate amplasa orice număr diferit de numărul de ordine al poziției respective.

Notăm cu $k$ numărul care se va amplasa pe poziția 1. Problema se împarte în două cazuri, în funcție de elementul de pe poziția $k$.

- Amplasăm numărul 1 pe poziția $k$. Problema se reduce la a calcula $D(n−2)$, pentru că
pe poziția 1 avem valoarea $k$, iar pe poziția $k$ avem valoare 1 și restul de $n −2$ valori se pot permuta independent de cele două cu respectarea cerinței inițiale.
- Amplasăm un număr diferit de 1 pe poziția $k$. Pentru fiecare poziție din cele cuprinse între 2 și $n$, există $n − 2$ elemente care pot fi amplasate pe poziția respecitvă. Pentru pozițiile diferite de $k$, nu se pot amplasa $k$ (pentru că apare deja pe prima poziție) sau elementul cu indicele poziției respective. Pentru poziția $k$, nu se pot amplasa $k$ sau 1 (pentru că suntem în cazul 2). Problema se reduce la a calcula $D(n−1)$, fiindcă trebuie să amplasăm valorile pe pozițiile cuprinse între 2 și $n$ (adică $n − 1$ poziții în total) și avem $n − 2$ variante pentru fiecare. 

Datorită faptului că numărul $k$ se poate alege în $n − 1$ moduri, formula finală este: $D(n) = (n − 1) \cdot (D(n − 2) + D(n − 1))$, calculele se vor efectua modulo $10^9 + 7$.

O altă variantă de abordare este să observăm, că $D(n)$ este egal cu numărul total de permutări de $n$ elemente, minus numărul de permutări care conțin puncte fixe. Să notăm cu $F(n, k)$ numărul de permutări de $n$ elemente, care conțin cel puțin $k$ puncte fixe. Cele $k$ poziții se pot alege în $C_n^k$ feluri, iar restul elementelor se pot permuta oricum, așadar: $F(n, k) = C_n^k \cdot (n − k)!$

Aplicând principiul includerii și excluderii avem: $D(n) = n! − F(n, 1) + F(n, 2) − F(n, 3) + \dots F(n, n)$.

Precalculând valorile factorialelor și a inverșilor modulari a factorialelor modulo $10^9 +7$ până la $10^6$ (valoarea maximă pentru $nc$), putem obține fiecare termen al formulei de mai sus în $O(1)$.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <bits/stdc++.h>
using namespace std;

const int mod = 1000000007;

int c, n, m, grid[1001][1001], viz[1001][1001];
long long ans[1000002];

int ox[] = {-1, 0, 1, 0};
int oy[] = {0, 1, 0, -1};

int main() {
    ifstream cin("poseidon.in");
    ofstream cout("poseidon.out");

    cin >> c >> n >> m;

    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            cin >> grid[i][j];

    int xp, yp;
    if (c == 1)
        cin >> xp >> yp;

    ans[0] = 1, ans[1] = 0, ans[2] = 1;
    for (int i = 3; i <= n * m; i++)
        ans[i] = (1LL * (i - 1) * (ans[i - 1] + ans[i - 2])) % mod;

    int ans1 = 0;
    long long ans2 = 1;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            if (grid[i][j] >= 0 && !viz[i][j]) {
                queue<pair<int, int>> q;
                viz[i][j] = 1;
                set<int> s;
                q.push({i, j});
                bool isP = 0;
                while (!q.empty()) {
                    pair<int, int> nod = q.front();
                    if (nod.first == xp && nod.second == yp)
                        isP = 1;
                    q.pop();

                    s.insert(grid[nod.first][nod.second]);
                    for (int dir = 0; dir <= 3; dir++) {
                        int nxtx = nod.first + ox[dir];
                        int nxty = nod.second + oy[dir];
                        if (nxtx >= 1 && nxtx <= n && nxty >= 1 && nxty <= m) {
                            if (grid[nxtx][nxty] == -1 || viz[nxtx][nxty])
                                continue;
                            viz[nxtx][nxty] = 1;
                            q.push({nxtx, nxty});
                        }
                    }
                }
                if (s.find(0) != s.end())
                    s.erase(0);
                if (isP)
                    ans1 = s.size();
                ans2 = (1LL * ans2 * ans[s.size()]) % mod;
            }

    if (c == 1)
        cout << ans1;
    else
        cout << ans2;
    return 0;
}
```