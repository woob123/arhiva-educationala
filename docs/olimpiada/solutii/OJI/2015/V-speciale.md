---
id: OJI-2015-V-speciale
title: Soluția problemei speciale (OJI 2015, clasa a V-a)
problem_id: 855
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

Trebuie doar să afișăm cifrele de la $9$ la $9-k+1$, fără spații, ca să constituie un număr.

## Cerința 2

Fie $x$ numărul de cifre ale lui $n$. Noi trebuie doar să testăm cele două numere speciale de pe linia $x-1$. Vom pleca de la ultima cifră. Când obținem două cifre care nu sunt egale, vom crește cu $1$ un contor și vom avansa doar în $n$. Altfel, avansăm și în $n$ și în numărul special.

## Cerința 3

Fie $x$ numărul de cifre ale lui $a$. Vom începe cu cele două numere speciale de pe linia $x$ din tabel, și vom continua cu cele de pe liniile următoare până când ajungem la numere mai mari ca $b$.

## Cod sursă

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <fstream>

using namespace std;

ifstream fin("speciale.in");
ofstream fout("speciale.out");

int main() {
    short k, cerinta, i, cifre = 0, probleme = 0, numere = 0;
    int n, a, b, nr = 0, copie, fi = 0;
    bool ok = false;
    fin >> cerinta >> k >> n >> a >> b;

    if (cerinta == 1) {
        for (i = 9; i > 9 - k; i--) {
            nr = nr * 10 + i;
        }
        fout << nr;
    } else if (cerinta == 2) {
        copie = n;
        while (copie > 0) {
            cifre++;
            copie /= 10;
        }
        cifre--;

        for (i = 1; i <= cifre; i++) {
            nr = nr * 10 + i;
        }

        copie = n;
        fi = nr;
        while (copie > 0) {
            if (copie % 10 == nr % 10) {
                nr /= 10;
            } else {
                probleme++;
            }
            copie /= 10;
        }

        if (probleme == 1) {
            fout << fi;
            ok = true;
        } else {
            nr = 0;
            for (i = 9; i > 9 - cifre; i--) {
                nr = nr * 10 + i;
            }

            probleme = 0;
            fi = nr;
            copie = n;
            while (copie > 0) {
                if (copie % 10 == nr % 10) {
                    nr /= 10;
                } else {
                    probleme++;
                }
                copie /= 10;
            }

            if (probleme == 1) {
                fout << fi;
                ok = true;
            }
        }

        if (!ok) {
            fout << 0;
        }
    } else {
        copie = a;
        while (copie > 0) {
            cifre++;
            copie /= 10;
        }

        for (i = 1; i <= cifre; i++) {
            nr = nr * 10 + i;
        }
        for (i = 9; i > 9 - cifre; i--) {
            fi = fi * 10 + i;
        }

        if (nr >= a) {
            numere++;
        }
        if (fi >= a && fi <= b) {
            numere++;
        }

        cifre++;
        copie = fi % 10 - 1;
        while (nr < b && fi < b) {
            nr = nr * 10 + cifre;
            fi = fi * 10 + copie;
            cifre++;
            copie--;

            if (nr <= b) {
                numere++;
            }
            if (fi <= b) {
                numere++;
            }
        }

        fout << numere;
    }
    return 0;
}
```
