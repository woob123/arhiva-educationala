---
id: OJI-2015-V-cuart
title: Soluția problemei cuart (OJI 2015, clasa a V-a)
problem_id: 854
authors:
    - traian
prerequisites:
   - digits-manipulation
tags:
    - OJI
    - clasa V
    - cifre
---

## Cerința 1

Putem să calculăm, folosind puteri de $10$, numerele scrise pe primul rând. Este important să păstrăm o copie a numărului inițial pentru a afla răspunsul efectiv.

## Cerința 2

Pentru a afla dacă un număr este *cuarț*, vom adăuga brut numărul curent la o sumă și îl vom crește cu $4$, până când suma va fi mai mare sau egală cu numărul.

Vom păstra primul număr de pe cele fiecare dintre cele două șiruri ale celor doi copii. La final, vom face o comparație.

## Cerința 3

Similar cu cerința 2, trebuie doar să afișăm unul dintre cele $4$ numere păstrate în funcție de caz.

## Cod sursă

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <fstream>

using namespace std;

ifstream fin("cuart.in");
ofstream fout("cuart.out");

int main() {
    short cerinta, n, cuart1 = 0, cuart2 = 0;
    int x, maxim = 0, primul1 = 0, primul2 = 0;
    fin >> cerinta >> n;

    for (int i = 0; i < n; i++) {
        fin >> x;
        int put = 1, y = 0, copie = x;
        while (x) {
            if (x % 2 == 1) {
                y += x % 10 * put;
                put *= 10;
            }
            x /= 10;
        }
        x = copie;
        if (y == 0) {
            if (cerinta == 1) {
                maxim = max(maxim, x);
            }
        } else {
            int k = 1, sum = 0;
            while (sum < y) {
                sum += k;
                k += 4;
            }
            if (sum == y) {
                cuart1++;
            }
            if (i == 0) {
                primul1 = y;
            }
        }
    }

    for (int i = 0; i < n; i++) {
        fin >> x;
        int put = 1, y = 0, copie = x;
        while (x) {
            if (x % 2 == 0) {
                y += x % 10 * put;
                put *= 10;
            }
            x /= 10;
        }
        x = copie;
        if (y == 0) {
            if (cerinta == 1) {
                maxim = max(maxim, x);
            }
        } else {
            int k = 1, sum = 0;
            while (sum < y) {
                sum += k;
                k += 4;
            }
            if (sum == y) {
                cuart2++;
            }
            if (i == 0) {
                primul2 = y;
            }
        }
    }

    if (cerinta == 1) {
        fout << maxim;
    } else if (cerinta == 2) {
        if (cuart1 > cuart2) {
            fout << 1;
        } else if (cuart1 < cuart2) {
            fout << 2;
        } else {
            if (primul1 > primul2) {
                fout << 1;
            } else if (primul1 < primul2) {
                fout << 2;
            } else {
                fout << 0;
            }
        }
    } else {
        if (cuart1 > cuart2) {
            fout << cuart1;
        } else if (cuart1 < cuart2) {
            fout << cuart2;
        } else {
            if (primul1 > primul2) {
                fout << primul1;
            } else if (primul1 < primul2) {
                fout << primul2;
            } else {
                fout << 0;
            }
        }
    }
    return 0;
}
```
