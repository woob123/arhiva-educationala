---
id: OJI-2025-XI-XII-cromatic
title: Soluția problemei Cromatic (OJI 2025, clasele XI-XII)
problem_id: 3616
authors: [zoltan]
prerequisites:
    - intro-combinatorics
tags:
    - OJI
    - clasa XI-XII
    - combinatorica
---

Să presupunem că șirul $a = (a_1, a_2, \dots, a_n)$ este cromatic.
Condiția ca șirul de intervale închise minmax:

$$[min_1, max_1], [min_2, max_2], \dots, [min_n, max_n]$$

să aibă toate elementele distincte implică faptul că orice element
$a_k$, dacă este comparat cu intervalul de pe poziția anterioară lui
$k$: $[min_{k-1}, max_{k-1}]$, atunci $a_k < min_{k-1}$ sau $a_k > max_{k-1}$.

De aici putem deduce că, față de elementul $a_1$, elementele următoare
din șir formează două subșiruri strict monotone. Elementele mai mari
decât $a_1$ formează un șir strict crescător, iar elementele mai mici
decât $a_1$ formează un șir strict descrescător. Dacă aceste două
subșiruri le interclasăm în toate modurile posibile, fiecare șir nou
obținut va fi un șir cromatic.

Considerând că avem un șir $a = (a_1, a_2, \dots, a_n)$, nu neapărat
cromatic, pentru a obține prin rearanjarea elementelor toate
permutările cromatice, vom rearanja elementele șirului în ordine
crescătoare.

În continuare vom considera că elementele șirului $a$ sunt ordonate crescător.

Se poate demonstra ușor că dacă în șir există două elemente egale,
atunci șirul nu este cromatic și nu există nicio altă permutare
cromatică, deci NSC = 0.

Pentru a calcula numărul șirurilor cromatice NSC, în continuare vom accepta că
șirul $a$ este strict crescător: $a_1 < a_2 < \dots < a_n$.

Acest șir este cromatic și în lista șirurilor cromatice este primul în ordine lexicografică.

Observăm că în lista ordonată lexicografic a tuturor șirurilor
cromatice elementele respectă următoarele proprietăți:

- (Grupa 1) $a_1, a_2, a_3, \dots, a_n$
- (Grupa 2) $a_2$ urmat de $(a_3, a_4, \dots, a_n)$ și $(a_1)$
interclasate în toate modurile posibile
- (Grupa 3) $a_3$ urmat de $(a_4, a_5, \dots, a_n)$ și $(a_2, a_1)$
interclasate în toate modurile posibile
- $\dots$
- (Grupa $k$) $a_k$ urmat de $(a_{k+1}, a_{k+2}, \dots, a_n)$ și
$(a_{k-1}, a_{k-2}, \dots, a_1)$ interclasate în toate modurile posibile
- $\dots$
- (Grupa $n$) $a_n, a_{n-1}, \dots, a_1$

Observăm că în fiecare grupă $k$ $(1 \leq k \leq n)$, numărul șirurilor
cromatice distincte este egal cu $C_{n-1}^{k-1}$.

De aici deducem că:

$$NSC = C_{n-1}^{0} + C_{n-1}^{1} + \dots + C_{n-1}^{n-1} = 2^{n-1}$$

În cadrul unei grupe $k$, ordinea lexicografică a șirurilor este
determinată de elementele subșirului descrescător $(a_{k-1}, a_{k-2},
\dots, a_1)$. Numărul de modalități de aranjare a celor $k - 1$
elemente pe cele $n - 1$ poziții este egal cu $C_{n-1}^{k-1}$. Folosind
această proprietate vom putea calcula poziția $p$ a unui șir în lista
permutărilor cromatice, sau pentru un șir cromatic dat, poziția lui $q$
în ordine lexicografică, folosind suma de combinări de ordin tot mai mic.

## Subtaskurile 1 și 2

Cazul $c = 1$, numărul șirurilor cromatice va fi egal cu $NSC = 0$, dacă
avem elemente egale în șir, respectiv $NSC = 2^{n−1}$ modulo $1 \ 000 \ 000 \ 007$.

## Subtaskurile 3, 4, 5, 6

Cazul $c = 2$. În subtaskul 3 este vorba despre o poziție $p$ dintre primele $n$
soluții în ordine lexicografică. Această cerință se poate rezolva cu un
ciclu simplu în complexitate $O(n)$.

În subtaskul 4 este vorba despre o poziție $p$ dintre ultimele $n$
soluții în ordine lexicografică. Și această cerință se poate rezolva cu
un ciclu simplu în complexitate **O(n)**.

Următoarele subtaskuri tratează cazuri generale:

- Subtaskul 5, având cazul $n \leq 20$, se poate rezolva cu un algoritm
backtracking cu complexitate $O(2^{n-1})$.
- Subtaskul 6 se rezolvă în complexitate $O(n)$ cu ajutorul sumelor de combinări.

## Subtaskurile 7, 8, 9, 10

Cazul $c = 3$. Similar cu subtaskurile 3, 4, 5 și 6, observăm că:

- Subtaskul 7, fiind caz particular, se rezolvă în complexitate $O(n)$.
- Subtaskul 8, fiind caz particular, se rezolvă în complexitate $O(n)$.
- Subtaskul 9 se poate rezolva cu un algoritm backtracking cu complexitate $O(2^{n-1})$.
- Subtaskul 10 se rezolvă în complexitate $O(n)$ cu ajutorul sumelor de combinări.

## Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
// Zoltan Szabo ISJ Mures
#include <algorithm>
#include <fstream>
using namespace std;
ifstream fin("cromatic.in");
ofstream fout("cromatic.out");
long long comb(int n, int k) {
    long long p = 1;
    for (int i = 1; i <= k; i++) {
        p = p * n / i;
        n--;
    }
    return p;
}
int main() {
    long long p, nr, nrsol = 0;
    int c, n, v[300005];
    fin >> c >> n;
    if (c == 3) {
        fin >> p;
    }
    for (int i = 1; i <= n; i++) {
        fin >> v[i];
    }
    if (c == 1) {
        sort(v + 1, v + n + 1);
        for (int i = 1; i < n; i++) {
            if (v[i] == v[i + 1]) {
                fout << 0;
                return 0;
            }
        }
        p = 1;
        for (int i = 1; i <= n - 1; i++) {
            p = (p * 2) % 1000000007;
        }
        fout << p;
        return 0;
    } else if (c == 2) {
        nr = 0;
        for (int i = 2; i <= n; i++) {
            if (v[1] > v[i]) {
                nr++;
            }
        }
        if (nr == 0) {
            fout << 1;
            return 0;
        }
        nrsol = 0;
        int N = n - 1;
        for (int i = 0; i <= nr - 1; i++) {
            nrsol = nrsol + comb(N, i);
        }
        for (int i = 2; i <= n; i++) {
            if (v[i - 1] > v[i]) {
                nr--;
                N--;
                if (nr == 0) {
                    nrsol++;
                    break;
                }
            } else {
                nrsol = nrsol + comb(N - 1, nr - 1);
                N--;
            }
        }
        fout << nrsol;
    } else if (c == 3) {
        sort(v + 1, v + n + 1);
        nr = 0;
        long long C = comb(n - 1, nr);
        while (p > C) {
            p = p - C;
            nr++;
            C = comb(n - 1, nr);
        }
        fout << v[nr + 1];
        int mare = nr + 2;
        for (int i = 2; i <= n; i++) {
            long long C = comb(n - i, nr - 1);
            if (p <= C) {
                fout << " " << v[nr--];
                if (nr == 0) {
                    break;
                }
            } else {
                fout << " " << v[mare++];
                p = p - C;
            }
        }
        while (mare <= n) {
            fout << " " << v[mare++];
        }
    }
    return 0;
}
```
