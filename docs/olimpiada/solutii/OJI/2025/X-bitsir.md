---
id: OJI-2025-X-bitsir
title: Soluția problemei Bitsir (OJI 2025, clasa a X-a)
problem_id: 3633
authors: [dobleaga]
prerequisites:
    - bitwise-ops
tags:
    - OJI
    - clasa X
---

## Subtask 1

Când $X = 0$, șirul $A_i$ trebuie să fie plin de 0 ($A_i = 0,\,\forall i \in {1,
2, \dots , N }$). Fiindcă $Y = 0$, iar $M_i = 0,\,\forall i \in {1, 2, \dots ,
N}$, răspunsul va fi mereu 1.

## Subtask 2

Când 𝑋 = 1, șirul $A_i$ trebuie să conțină cel puțin o valoare de 1, restul
fiind egale cu 0. Din moment ce $Y = 0$, numărul de valori de 1 trebuie să fie
par. Astfel, răspunsul va fi $C_N^2 + C_N^4 + ... = 2^{N-1} - 1$.

## Subtask 3

Observație: șirul $A_i$ nu poate avea elemente mai mari decât $X$. Pentru acest
subtask, se creează un for pentru fiecare valoare de la 0 la $X$ și se verifică
toate condițiile.

## Subtask 4

O generalizare a subtaskului 1, trebuie verificat dacă șirul plin de 0 respectă
proprietățile 2 și 3.

## Subtask 5

Pe baza observației anterioare și a mărimii șirului ($N \leq 4$), soluția pentru
acest subtask este iterarea prin toate posibilitățile și verificarea celor 3
condiții.

Complexitate: $\mathcal{O}(X^4)$ ca timp.

## Subtask 6

!!! note "Observație"

    Operația de XOR ($\oplus$) are următoarea proprietate: $a \oplus b = c
    \Rightarrow a = b \oplus c$.

Se iterează prin toate valorile posibile ale lui $A_1$ ($\{0, 1, \dots X\}$), se
află valoarea lui $A_2 = Y \oplus A_1$ și se verifică celelalte două condiții.

Complexitate: $\mathcal{O}(X)$.

## Subtask 7

În acest caz, cele 3 condiții pot fi reformulate astfel:

- Dacă $X = 1$, atunci există cel puțin o valoare de 1 în șir, altfel tot șirul
  este complet 0.
- Dacă $Y = 1$, atunci există un număr impar de 1 în șir, altfel există un număr
  par de 1.
- Dacă $M_i = 1$, atunci $A_i$ = 1, altfel $A_i$ poate fi 0 sau 1.

Dacă $X = 0$, atunci problema se reduce la Subtaskul 4. Altfel, în funcție de
valoarea lui $Y$ și câte valori de 1 există în șirul $M$, se determină paritatea
numărului de valori care pot fi alese. Mai exact, dacă se notează cu $F$ numărul
de valori de 1 din șirul $M$ și cu $R$ răspunsul, există următoarele cazuri:

- $F$ impar și $Y = 0$: $R = C_1^{N-F} + C_3^{N-F} + \dots = 2^{N - F - 1}$
- $F$ impar și $Y = 1$: $R = C_0^{N-F} + C_2^{N-F} + \dots = 2^{N - F - 1}$
- $F$ par și $Y = 0$: $R = C_0^{N-F} + C_2^{N-F} + \dots = 2^{N - F - 1}$
- $F$ par și $Y = 1$: $R = C_1^{N-F} + C_3^{N-F} + \dots = 2^{N - F - 1}$

Dacă $X = 1$, $F = 0$ și $Y = 0$, atunci la formulă se scade 1 deoarece se
înnumără și șirul plin de 0.

Complexitate: $\mathcal{O}(N)$

## Subtask 8

Din cauza naturii operațiilor pe biți, problema poate fi rezolvată bit cu bit,
fiind împărțită astfel în 30 de subprobleme de tipul subtaskului 7. Mai exact,
pentru bitul $b$, subproblema este următoarea:

- $X_b = 1$, dacă $X$ conține bitul $b$, altfel $X_b = 0$.
- $Y_b = 1$, dacă $Y$ conține bitul $b$, altfel $Y_b = 0$.
- $M_i = 1$, dacă $M_i$ conține bitul $b$, altfel $M_b = 0$.

Se rezolvă toate subproblemele, iar răspunsul final va fi produsul tuturor
răspunsurilor subproblemelor.

## Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <bits/stdc++.h>

using namespace std;

ifstream f("bitsir.in");
ofstream g("bitsir.out");

typedef long long LL;
constexpr int NMAX = 1e5 + 5;
constexpr int MOD = 1e9 + 7;

int N;
LL X, Y;
LL Mask[NMAX];

int fact[NMAX];
int inv_fact[NMAX];

int LgPut(int a, int b) {
    if (b == 1) {
        return a;
    }

    if (b == 0) {
        return 1;
    }

    int val = LgPut(a, b / 2);

    val = (1LL * val * val) % MOD;

    if (b % 2 == 1) {
        return (1LL * val * a) % MOD;
    }

    return val;
}

void Precalculare() {
    fact[0] = 1;

    for (int i = 1; i <= N; ++i) {
        fact[i] = (1LL * fact[i - 1] * i) % MOD;
    }
    inv_fact[N] = LgPut(fact[N], MOD - 2);

    for (int i = N - 1; i >= 0; --i) {
        inv_fact[i] = (1LL * inv_fact[i + 1] * (i + 1)) % MOD;
    }
}

int Comb(int n, int k) {
    int ans = (1LL * inv_fact[k] * inv_fact[n - k]) % MOD;

    return (1LL * ans * fact[n]) % MOD;
}

int main() {
    f >> N >> X >> Y;

    for (int i = 1; i <= N; ++i) {
        f >> Mask[i];
    }

    if (Y > X) {
        g << "NU\n" << 0;
        return 0;
    }
    Precalculare();

    int answer = 1;
    for (int b = 0; (1LL << b) <= X; ++b) {
        if ((X & (1LL << b)) == 0) {
            for (int i = 1; i <= N; ++i) {
                if ((Mask[i] & (1LL << b))) {
                    g << "NU\n" << 0;
                    return 0;
                }
            }

            if ((Y & (1LL << b))) {
                g << "NU\n" << 0;
                return 0;
            }

            continue;
        }

        int start = 0;
        if ((Y & (1LL << b))) {
            start = 1;
        }

        int cnt_forced_one = 0;
        for (int i = 1; i <= N; ++i) {
            if ((Mask[i] & (1LL << b))) {
                cnt_forced_one++;
            }
        }

        if (cnt_forced_one % 2 == 1) {
            start = (1 - start);
        }

        int n = N - cnt_forced_one;
        bool exists_one = (cnt_forced_one > 0);

        if (!exists_one && start == 0) {
            start += 2;
        }

        int coef = 0;
        for (int i = start; i <= n; i += 2) {
            coef = (1LL * coef + 1LL * Comb(n, i)) % MOD;
        }

        if (coef == 0) {
            g << "NU\n" << 0;
            return 0;
        }
        answer = (1LL * answer * coef) % MOD;
    }

    g << "DA\n" << answer;

    return 0;
}
```
