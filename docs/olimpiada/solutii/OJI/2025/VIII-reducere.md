---
id: OJI-2025-VIII-reducere
title: Soluția problemei reducere (OJI 2025, clasa a VIII-a)
problem_id: 3642
authors: [cerchez]
prerequisites:
    - divisibility
tags:
    - OJI
    - clasa VIII
---

## Cerința 1

Pentru a determina valoarea de reducere, trebuie să determinăm cel mai mare
divizor comun al valorilor din secvență. Pentru a determina cmmdc pentru două
valori utilizăm algoritmul lui Euclid. Pentru a determina cmmdc pentru o
secvență de $n$ valori utilizăm asociativitatea operației cmmdc, deci determinăm
la fiecare pas cmmdc dintre cmmdc-ul curent și următoarea valoare din secvență:

$$
(a_1, a_2, ..., a_n) = (a_1, (a_2, ..., (a_{n - 1}, a_{n})))
$$

unde $(a, b)$ reprezintă cmmdc-ul lui $a$ și $b$.

## Cerința 2

Descompunem în factori primi fiecare valoare din secvență şi determinăm pe
parcurs descom punerea în factori primi a celui mai mic multiplu comun al
acestor valori (toți factorii primi care apar în descompunerile valorilor din
secvență la puterea cea mai mare). Factorii primi comuni la puterea cea mai mică
constituie cmmdc (deci îi păstrăm în valoarea de reducere). Numărul minim de
operații care trebuie să fie aplicate pentru a obține valoarea de reducere este
egal cu suma exponenților factorilor primi din descompunerea în factori primi a
cmmmc/cmmdc.

Pentru subtask-ul 2, restricțiile permit utilizarea unui vector $nr$ de $10^6$
elemente, unde $nr_i$ este puterea factorului prim $i$ în descompunerea factori
primi a cmmmc/cmmdc. Pentru a obține punctele pe acest subtask nu este necesar
să optimizăm descompunerea în factori primi utilizând pregenerarea numerelor
prime cu ciurul lui Eratostene, dar, pentru subtask-ul 3, este necesar să
descompunem în factori primi căutând divizorii, doar printre numerele prime până
la radicalul numărului.

Pentru subtask-ul 3 restricțiile nu permit declararea vectorului $nr$. Ca urmare
vom reține o descompunere în factori primi ca o listă de factori primi şi
puterile acestora, listă în care factorii primi apar în ordine crescătoare.

Pentru fiecare număr din secvență:

-   descompunem numărul în factori primi;
-   printr-un algoritm similar cu algoritmul de interclasare, actualizăm
    descompunerea în factori primi a cmmmc (în cmmmc trebuie să apară toți
    factorii primi la puterea cea mai mare).

La final simplificăm cmmmc cu cmmdc, parcurgând descompunerile în factori primi
ale acestora şi, pentru factorii primi comuni, scăzând din puterea factorului
prim din cmmmc puterea factorului prim respectiv din cmmdc.

Suma puterilor factorilor primi ai cmmmc după simplificarea cu cmmdc va fi
numărul minim de operații de reducere necesare.

## Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
// Em. Cerchez 100 puncte
#include <fstream>
#define VMAX 1000000
#define PMAX 80000
#define LGMAX 8
#define NMAX 2002
using namespace std;
ifstream fin("reducere.in");
ofstream fout("reducere.out");
struct factor {
    long long int d;
    short int p;
};
bool ciur[VMAX];
int prime[PMAX];
void eratostene();
long long int cmmdc();
int c, n, m, nrp;
long long int cmd;
factor D[LGMAX], dx[LGMAX], rez[LGMAX * NMAX], aux[LGMAX * NMAX];
int lgD, lgx, lgrez;
long long int nr[NMAX];

long long int cerinta2();
void descompunere(long long int x, factor dx[], int &lgx);

int main() {
    int i;
    fin >> c >> n;
    for (i = 0; i < n; i++) {
        fin >> nr[i];
    }
    cmd = cmmdc();
    if (c == 1) {
        fout << cmd << '\n';
    } else {
        eratostene();
        fout << cerinta2() << '\n';
    }
    return 0;
}

void eratostene() {
    int i, j;
    for (i = 3; i * i < VMAX; i += 2) {
        if (ciur[i] == 0) {
            for (j = i * i; j < VMAX; j += i) {
                ciur[j] = 1;
            }
        }
    }
    prime[0] = 2;
    nrp = 1;
    for (i = 3; i < VMAX; i += 2) {
        if (ciur[i] == 0) {
            prime[nrp++] = i;
        }
    }
}

long long int cmmdc() {
    int i;
    long long int d, x, r;
    d = nr[0];
    for (i = 1; i < n; i++) {
        x = nr[i];
        while (x) {
            r = d % x;
            d = x;
            x = r;
        }
    }
    return d;
}

void descompunere(long long int x, factor dx[], int &lgx) {
    int i, m;
    lgx = 0;
    for (i = 0; i < nrp && (long long int)prime[i] * prime[i] <= x; i++) {
        if (x % prime[i] == 0) {
            m = 0;
            while (x % prime[i] == 0) {
                m++;
                x /= prime[i];
            }
            dx[lgx].p = m;
            dx[lgx].d = prime[i];
            lgx++;
        }
    }
    if (x > 1) {
        dx[lgx].d = x;
        dx[lgx].p = 1;
        lgx++;
    }
}

void actualizeaza() {
    int i = 0, j = 0, k = 0;
    while (i < lgx && j < lgrez) {
        if (dx[i].d == rez[j].d) {
            aux[k].d = dx[i].d;
            aux[k].p = max(dx[i].p, rez[j].p);
            i++;
            j++;
            k++;
        } else if (dx[i].d < rez[j].d) {
            aux[k++] = dx[i++];
        } else {
            aux[k++] = rez[j++];
        }
    }
    while (i < lgx) {
        aux[k++] = dx[i++];
    }
    while (j < lgrez) {
        aux[k++] = rez[j++];
    }
    for (i = 0; i < k; i++) {
        rez[i] = aux[i];
    }
    lgrez = k;
}
long long int cerinta2() {
    int i, j;
    long long int nrop = 0;
    descompunere(cmd, D, lgD);
    descompunere(nr[0], rez, lgrez);
    for (i = 1; i < n; i++) {
        descompunere(nr[i], dx, lgx);
        actualizeaza();
    }
    i = 0;
    j = 0;
    while (i < lgD) {
        if (D[i].d == rez[j].d) {
            rez[j].p -= D[i].p;
            i++;
            j++;
        } else {
            j++;
        }
    }
    for (i = 0; i < lgrez; i++) {
        nrop += rez[i].p;
    }
    return nrop;
}
```
