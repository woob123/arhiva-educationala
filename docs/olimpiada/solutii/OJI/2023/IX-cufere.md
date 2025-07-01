---
id: OJI-2023-IX-cufere
title: Soluția problemei cufere (OJI 2023, clasa a IX-a)
problem_id: 508
authors: [schiopu]
prerequisites:
    - sieve
    - simulating-solution
tags:
    - OJI
    - clasa IX
---

Se vor reține într-un tablou unidimensional $fr$ numărul de obiecte
întâlnite pentru fiecare tip de obiect (etichetă). Acesta se poate
construi în timpul citirii datelor de intrare. Când citim numărul
$xxyy$ ce codifică o celulă dintr-un cufăr, incrementăm numărul de
obiecte întâlnite cu eticheta $yy$ cu $xx$, adică
$fr[xx] = fr[xx] + yy$.

Tabloul va avea lungime maxim 100, întrucât etichetele iau valori în
intervalul $[10, 99]$.

## Cerința 1

Se afișează în ordinea crescătoare a etichetelor din intervalul $[10,99
]$ toate perechile $(x, fr[x])$ pentru care $fr[x] > 0$.

## Cerința 2

Se parcurge tabloul $fr$ în ordinea crescătoare a etichetelor, de la
$10$, până la $99$ inclusiv. Pentru fiecare etichetă $x$ se verifică
primalitatea acesteia: dacă x este număr prim, se setează dimensiunea
maximă a unui grup $g = 16$, altfel se setează $g = 64$.

Cât timp încă există obiecte cu eticheta curentă $(fr[x] > 0)$, se
adaugă pe următoarea celulă liberă din cele $n$ cufere un nou grup de
obiecte cu eticheta $x$ de dimensiune $min(g, fr[x])$ și se
scade $fr[x]$ cu $g$ (sau se setează cu $0$ în cazul în care $fr[x] < g$).
Dacă mai rămân celule libere în cufere, se completează cu 0.

Verificarea primalității unui număr poate fi precalculată într-un
tablou unidimensional $prim[i] = 1$ dacă $i$ este prim și $0$ altfel,
și poate fi calculată în diverse modalități.

Complexitatea temporală este: $O(N)$.
Complexitatea spațială este: $O(1)$, întrucât dimensiunea tablourilor
$fr$ și $prim$ este constantă.

## Rezolvare

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <fstream>

using namespace std;
ifstream fin("cufere.in");
ofstream fout("cufere.out");

int row = 3, col = 9;
int f[101], p[100], c, n;

void numar(int x) {
    int eticheta;
    eticheta = x % 100;
    f[eticheta] += x / 100;
}

void ciur() {
    for (int i = 2; i <= 99; i++) {
        if (p[i] == 0) {
            for (int j = 2 * i; j <= 99; j += i) {
                p[j] = 1;
            }
        }
    }
}

void afisarespatiu(int d) {
    if (d % 9 == 0) {
        fout << "\n";
    }
}

void ordonare() {
    int afisare = 0;
    for (int i = 10; i <= 99; i++) {
        if (f[i] != 0) {
            if (f[i] > 64 && p[i] != 0) {
                for (int j = 1; j <= f[i] / 64; j++) {
                    fout << 64 << i << " ";
                    afisare++, afisarespatiu(afisare);
                }
                if (f[i] % 64 != 0) {
                    fout << f[i] % 64 << i << " ";
                    afisare++, afisarespatiu(afisare);
                }
            } else if (f[i] < 64 && p[i] != 0) {
                if (f[i] % 64 != 0) {
                    fout << f[i] % 64 << i << " ";
                    afisare++, afisarespatiu(afisare);
                }
            } else if (f[i] > 16 && p[i] == 0) {
                for (int j = 1; j <= f[i] / 16; j++) {
                    fout << 16 << i << " ";
                    afisare++, afisarespatiu(afisare);
                }
                if (f[i] % 16 != 0) {
                    fout << f[i] % 16 << i << " ";
                    afisare++, afisarespatiu(afisare);
                }
            } else if (f[i] < 16 && p[i] == 0) {
                if (f[i] % 16 != 0) {
                    fout << f[i] % 16 << i << " ";
                    afisare++, afisarespatiu(afisare);
                }
            }
        }
    }
    for (int i = afisare; i < row * col * n; i++) {
        fout << 0 << " ";
        afisare++;
        afisarespatiu(afisare);
    }
}

int main() {
    int c, x;
    fin >> c >> n;

    for (int i = 1; i <= n * row * col; i++) {
        fin >> x;
        numar(x);
    }

    if (c == 1) {
        for (int i = 10; i <= 99; i++) {
            if (f[i] != 0) {
                fout << i << " " << f[i] << "\n";
            }
        }
    }

    if (c == 2) {
        ciur();
        ordonare();
    }
    return 0;
}
```
