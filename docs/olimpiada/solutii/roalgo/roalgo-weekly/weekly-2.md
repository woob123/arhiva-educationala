---
id: roalgo-weekly-2
title: Descrierea soluțiilor RoAlgo Weekly Contest 2
authors: [stefdasca]
prerequisites:
    - basic-math
    - arrays
    - matrices
    - strings
tags:
    - clasa
    - bacalaureat
    - clasa IX
---

Vă mulțumim că ați luat parte la concursul
[RoAlgo Weekly Contest #2](https://kilonova.ro/contests/1522/). Acest concurs nu
ar fi putut avea loc fără sprijinul vostru și fără ajutorul testerilor
concursului, care sunt prezenți în lista de editori a concursului de pe Kilonova.

Această serie de concursuri va continua în fiecare săptămână, scopul fiind acela
de a deveni un concurs similar în popularitate cu rundele de pe LeetCode și nu
numai.

## Problema [lexisir](https://kilonova.ro/problems/3687)

Pentru a rezolva această problemă, trebuie să alternăm literele a și b
pentru a avea un șir cât mai mic lexicografic.

### Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <iostream>
using namespace std;
int main() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++) {
        if (i % 2 == 1) {
            cout << 'a';
        }
        else {
            cout << 'b';
        }
    }
    return 0;
}
```

## Problema [abecedar](https://kilonova.ro/problems/3661)

Deoarece datele de intrare sunt mici, putem verifica pentru fiecare număr de la
$1$ la $10^5$ dacă este un divizor al tuturor numerelor sau măcar al unui număr.

### Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <iostream>
using namespace std;
int main() {
    int a, b, c;
    cin >> a >> b >> c;
    for (int i = 1; i <= 100000; i++) {
        if (a % i == 0 && b % i == 0 && c % i == 0) {
            cout << i << " ";
        }
    }
    cout << '\n';
    for (int i = 1; i <= 100000; i++) {
        if (a % i == 0 || b % i == 0 || c % i == 0) {
            cout << i << " ";
        }
    }
    cout << '\n';
    return 0;
}

```

## Problema [secvk](https://kilonova.ro/problems/3662)

Pentru a rezolva această problemă, este îndeajuns să verificăm fiecare
subsecvență din șir, ținând suma numerelor într-o variabilă și verificând dacă
suma este multiplu de $k$. Trebuie să aveți grijă să folosiți tipul de date
long long.

### Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <iostream>
using namespace std;
int main() {
    int n, k;
    cin >> n >> k;
    
    int v[n+1];
    for (int i = 1; i <= n; i++) {
        cin >> v[i];
    }
    int cnt = 0;
    for (int i = 1; i <= n; i++) {
        long long sm = 0;
        for (int j = i; j <= n; j++) {
            sm = sm + v[j];
            if (sm % k == 0) {
                cnt++;
            }
        }
    }
    cout << cnt << '\n';
    return 0;
}
```

## Problema [magica](https://kilonova.ro/problems/3668)

Pentru a rezolva această problemă, trebuie să avem grijă să verificăm toate
sumele posibile, fie că este vorba de suma liniilor, coloanelor sau diagonalelor.

### Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <iostream>
using namespace std;
int main() {
    int n;
    cin >> n;
    
    int v[n+1][n+1];
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            cin >> v[i][j];
        }
    }
    
    int sm = 0;
    int sm2 = 0;
    for (int i = 1; i <= n; i++) {
        sm += v[i][i];
        sm2 += v[i][n-i+1];
    }
    if (sm != sm2) {
        cout << "NU E MAGICA";
        return 0;
    }
    for (int i = 1; i <= n; i++) {
        sm2 = 0;
        for (int j = 1; j <= n; j++) {
            sm2 += v[i][j];
        }
        if (sm != sm2) {
            cout << "NU E MAGICA";
            return 0;
        }
        sm2 = 0;
        for (int j = 1; j <= n; j++) {
            sm2 += v[j][i];
        }
        if (sm != sm2) {
            cout << "NU E MAGICA";
            return 0;
        }
    }
    cout << "MAGICA" << '\n';
    return 0;
}
```
