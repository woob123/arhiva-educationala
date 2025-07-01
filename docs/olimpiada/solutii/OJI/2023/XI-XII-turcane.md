---
id: OJI-2023-XI-XII-turcane
title: Soluția problemei Turcane (OJI 2023, clasele XI-XII)
problem_id: 501
authors: [bunget, acconstantinescu]
prerequisites:
    - intro-dp
    - partial-sums
tags:
    - OJI
    - clasa XI-XII
---

## Subtask 1 (26 puncte)

Avem de calculat numărul minim de sărituri pentru o matrice cu o linie.
Dacă $N - 1$ este divizibil cu $P$, rezultatul este $(N - 1) / P$, altfel
este $⌈(N - 1) / P⌉ + 1$, unde $⌈x⌉$ reprezintă partea întreagă a lui $x$.

Complexitate: $O(1)$.

## Subtask 2 (15 puncte)

Avem de calculat numărul minim de sărituri pentru o matrice cu două linii.
Pentru aceasta, se va determina minimul dintre numărul de sărituri obținut
în fiecare din următoarele situații:

- Se efectuează o săritură a calului, dacă este posibil, ajungând în
pătrățelul $(2, 3)$, după care se aplică formula de la subtask 1;
- Se efectuează o săritură pe diagonală, dacă este posibil, pentru $N = 2$;
- Se efectuează o săritură pe verticală, apoi pe orizontală, dacă este
posibil, pentru $N = 2$.

Complexitate: $O(1)$.

## Subtask 3 (7 puncte)

Avem de calculat numărul minim de sărituri pentru o matrice cu trei linii.
Pentru aceasta, se va determina minimul dintre numărul de sărituri obținut
în fiecare din următoarele situații:

- Se efectuează două sărituri ale calului, dacă este posibil, ajungând în
pătrățelul $(3, 5)$, după care se aplică formula de la subtask 1;
- Se efectuează una sau două sărituri pe diagonală, dacă este posibil,
pentru a ajunge pe linia a treia, apoi se aplică formula de la subtask 1;
- Se efectuează una sau două sărituri pe verticală, dacă este posibil,
pentru a ajunge pe linia a treia, apoi se aplică formula de la subtask 1;
- Se efectuează o săritură a calului combinată cu o săritură pe verticală
sau diagonală pentru a ajunge pe linia a treia, apoi se aplică formula de
la subtask 1.

Complexitate: $O(1)$

## Subtask 4 (7 puncte)

Se folosește programarea dinamică. Vom nota cu $d[i][j]$ numărul minim de
sărituri necesare pentru a ajunge în pătrățelul $(i, j)$. Astfel, se
inițializează $d[1][1] = 0$ și $d[i][j] = m + n$ pentru orice indici $i,
j$. Se parcurge matricea $d$ și pentru fiecare poziție $(i, j)$ se fac
actualizările:

- $d[i][j + k] = min(d[i][j + k], d[i][j] + 1)$,
pentru fiecare $1 \leq k \leq P$ cu $j + k \leq N$;
- $d[i + k][j] = min(d[i + k][j], d[i][j] + 1)$,
pentru fiecare $1 \leq k \leq Q$ cu $i + k \leq M$;
- $d[i + k][j + k] = min(d[i + k][j + k], d[i][j] + 1)$,
pentru fiecare $1 \leq k \leq R$ cu $i + k \leq M$, $j + k \leq N$;
- $d[i + 1][j + 2] = min(d[i + 1][j + 2], d[i][j] + 1)$
și $d[i + 2][j + 1] = min(d[i + 2][j + 1], d[i][j] + 1)$,
când indicii nu depășesc dimensiunile matricei.

Complexitate: $O(M \cdot N \cdot (M + N + min(M, N)))$.

## Subtask 5 (8 puncte)

Se folosește programarea dinamică. Vom nota cu $d[i][j]$ numărul minim de
sărituri necesare pentru a ajunge în pătrățelul $(i, j)$. Astfel, se
inițializează $d[1][1] = 0$ și $d[i][j] = m + n$ pentru orice indici $i,
j$. Se parcurge matricea $d$ și pentru fiecare poziție $(i, j)$ se fac
actualizările:

- $d[i][j + k] = min(d[i][j + k], d[i][j] + 1)$, pentru $k = min(P, N - j)$;
- $d[i + k][j] = min(d[i + k][j], d[i][j] + 1)$, pentru $k = min(Q, M - i)$;
- $d[i + k][j + k] = min(d[i + k][j + k], d[i][j] + 1)$,
pentru $k = min(R, M - i, N - j)$;
- $d[i + 1][j + 2] = min(d[i + 1][j + 2], d[i][j] + 1)$ și $d[i + 2][j + 1] =
min(d[i + 2][j + 1], d[i][j] + 1)$, când indicii nu depășesc dimensiunile matricei.

Complexitate: $O(M \cdot N)$.

## Soluție alternativă

Se folosește programarea dinamică. Astfel, se inițializează $d[1][1] = 1$
și $d[i][j] = m + n$ pentru orice indici $i, j$ în rest. Se parcurge
matricea $d$ și pentru fiecare poziție $(i, j)$ diferită de poziția
$(1, 1)$, se actualizează $d[i][j] = min(a, b, c, e, f)$, unde:

- $a = min(d[i][j - k] | 1 \leq k \leq P, 1 \leq j - k)$;
- $b = min(d[i - k][j] | 1 \leq k \leq Q, 1 \leq i - k)$;
- $c = min(d[i - k][j - k] | 1 \leq k \leq R, 1 \leq i - k, 1 \leq j - k)$;
- $e = d[i - 1][j - 2]$;
- $f = d[i - 2][j - 1]$.

Pentru a calcula $a$, $b$, $c$ în $O(1)$, se folosește deque pe fiecare
linie, coloană și diagonală.

Complexitate: $O(M \cdot N)$.

O soluție asemănătoare, dar care nu trece toate testele, este ca în locul
structurii deque să folosim set.

Complexitate: $O(M \cdot N \cdot \log(max(M, N)))$.

## Subtask 6 (11 puncte)

Se folosește metoda backtracking pentru a genera toate drumurile de la
pătrățelul de coordonate $(1, 1)$ la pătrățelul $(M, N)$.

Complexitate: $O(2^(M + N + min(M, N)))$.

## Subtask 7 (12 puncte)

Se folosește programarea dinamică. Vom nota cu $d[i][j]$ numărul de moduri
în care se poate ajunge în pătrățelul $(i, j)$. Se inițializează $d[1][1] =
1$, se parcurge matricea $d$ și pentru fiecare poziție $(i, j)$ se fac
actualizările:

- $d[i][j + k] += d[i][j]$, pentru fiecare $1 \leq k \leq P$ cu $j + k \leq N$;
- $d[i + k][j] += d[i][j]$, pentru fiecare $1 \leq k \leq Q$ cu $i + k \leq M$;
- $d[i + k][j + k] += d[i][j]$, pentru fiecare $1 \leq k \leq R$
cu $i + k \leq M$, $j + k \leq N$;
- $d[i + 1][j + 2] += d[i][j]$ și $d[i + 2][j + 1] += d[i][j]$,
când indicii nu depășesc dimensiunile matricei.

Complexitate: $O(M \cdot N \cdot (M + N + min(M, N)))$.

## Subtask 8 (14 puncte)

Se folosește programarea dinamică. Vom nota cu $d[i][j]$ numărul de moduri
în care se poate ajunge în pătrățelul $(i, j)$. Se inițializează $d[1][1]
= 1$, se parcurge matricea $d$ și pentru fiecare poziție $(i, j)$ se fac
actualizările:

- $d[i][j] += ∑d[i][j - k]$, pentru fiecare $1 \leq k \leq P$ cu $1 \leq j - k$;
- $d[i][j] += ∑d[i - k][j]$, pentru fiecare $1 \leq k \leq Q$ cu $1 \leq i - k$;
- $d[i][j] += ∑d[i - k][j - k]$, pentru fiecare $1 \leq k \leq R$
cu $1 \leq i - k$, $1 \leq j - k$;
- $d[i][j] += d[i - 2][j - 1]$ și $d[i][j] += d[i - 1][j - 2]$,
când indicii nu depășesc dimensiunile matricei.

Pentru aceasta, se vor calcula dinamic sumele parțiale ale elementelor
matricei $d$, pentru fiecare linie, coloană și diagonală.

Complexitate: $O(M \cdot N)$.

## Rezolvare

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <bits/stdc++.h>
using namespace std;

ifstream fin("turcane.in");
ofstream fout("turcane.out");

const int mod = 1e9 + 7;
int n, m, p, q, r;

void minSelf(int &x, int y) {
  if (y < x) {
    x = y;
  }
}

void addSelf(int &x, int y) {
  x += y;
  if (x >= mod) {
    x -= mod;
  }
}

void solveFirstTask() {
  vector<vector<int>> dp(n + 1, vector<int>(m + 1, mod));

  dp[1][1] = 0;

  for (int i = 1; i <= n; ++i) {
    for (int j = 1; j <= m; ++j) {
      minSelf(dp[i][j + min(p, m - j)], dp[i][j] + 1);
      minSelf(dp[i + min(q, n - i)][j], dp[i][j] + 1);
      minSelf(dp[i + min({r, n - i, m - j})][j + min({r, n - i, m - j})], dp[i][j] + 1);

      if (i + 1 <= n && j + 2 <= m) {
        minSelf(dp[i + 1][j + 2], dp[i][j] + 1);
      }

      if (i + 2 <= n && j + 1 <= m) {
        minSelf(dp[i + 2][j + 1], dp[i][j] + 1);
      }
    }
  }

  fout << dp[n][m] << '\n';
}

void solveSecondTask() {
  vector<vector<int>> dp(n + 1, vector<int>(m + 1));
  vector<int> lin(m + 2);
  vector<vector<int>> col(n + 2, vector<int>(m + 1));
  vector<vector<int>> diag(n + 2, vector<int>(m + 2));

  dp[1][1] = 1;

  for (int i = 1; i <= n; ++i) {
    for (int j = 1; j <= m; ++j) {
      lin[j] = 0;
    }

    for (int j = 1; j <= m; ++j) {
      addSelf(lin[j], lin[j - 1]);
      addSelf(col[i][j], col[i - 1][j]);
      addSelf(diag[i][j], diag[i - 1][j - 1]);

      addSelf(dp[i][j], lin[j]);
      addSelf(dp[i][j], col[i][j]);
      addSelf(dp[i][j], diag[i][j]);

      if (p && j + 1 <= m) {
        addSelf(lin[j + 1], dp[i][j]);
        addSelf(lin[j + min(p, m - j) + 1], mod - dp[i][j]);
      }

      if (q && i + 1 <= n) {
        addSelf(col[i + 1][j], dp[i][j]);
        addSelf(col[i + min(q, n - i) + 1][j], mod - dp[i][j]);
      }

      if (r && i + 1 <= n && j + 1 <= m) {
        addSelf(diag[i + 1][j + 1], dp[i][j]);
        addSelf(diag[i + min({r, n - i, m - j}) + 1][j + min({r, n - i, m - j}) + 1], mod - dp[i][j]);
      }

      if (i + 1 <= n && j + 2 <= m) {
        addSelf(dp[i + 1][j + 2], dp[i][j]);
      }

      if (i + 2 <= n && j + 1 <= m) {
        addSelf(dp[i + 2][j + 1], dp[i][j]);
      }
    }
  }

  fout << dp[n][m] << '\n';
}

int main() {
  int task;
  fin >> task >> n >> m >> p >> q >> r;

  if (task == 1) {
    solveFirstTask();
  } else {
    solveSecondTask();
  }
  return 0;
}
```
