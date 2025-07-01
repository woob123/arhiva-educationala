---
id: OJI-2025-XI-XII-aventura
title: Soluția problemei Aventura (OJI 2025, clasele XI-XII)
problem_id: 3615
authors: [ivan]
prerequisites:
     - graphs
     - toposort
tags:
    - OJI
    - clasa XI-XII
    - grafuri
---

Vom nota $k_1 + k_2 + \dots + k_N$ cu $\sum K$.

## Subtask 1

Se generează toate permutările șirului $1, 2, \dots, N$, iar pentru
fiecare șir se încearcă completarea nivelelor în ordinea generată.

Complexitate $O(N! \cdot \sum K)$.

## Subtask 2

O dependență circulară de dimensiune exact $2$ reprezintă, în graful
format de restricțiile jocului, un ciclu de lungime $2$. Mai precis,
dacă fixăm două nivele $i$ și $j$ din graf și există muchii în ambele
sensuri, atunci este clar că ambele nivele nu vor putea fi completate
vreodată. Acest lucru se poate verifica ușor dacă muchiile din graf
sunt reținute într-o matrice de adiacență.

Putem porni din unul dintre cele două nivele o parcurgere DFS,
deoarece orice nivel care anterior era condiționat de unul dintre
aceste nivele clar nu va fi parcurs niciodată.

În final, restul nivelelor nemarcate vor putea fi completate în joc.

Complexitate $O(N^2)$.

## Subtask 3

Pentru fiecare nivel, vom ține minte $grad[x]$ – numărul de nivele de
care este condiționat nivelul $x$. La fiecare pas, vom căuta un nivel $i$
care are $grad[i] = 0$ și vom scădea gradul fiecărui nivel
condiționat de nivelul $i$.

Complexitate $O(N^2 + \sum K)$.

## Subtask 4

Vom folosi construcția de la subtaskul precedent, însă, în loc să
căutăm manual nivelele cu gradul $0$, vom utiliza o coadă. Mai întâi,
vom adăuga în coadă toate nivelele care au gradul inițial egal cu $0$.

Apoi, vom parcurge coada, iar în momentul în care scădem gradele
vecinilor nivelului curent, dacă unul dintre vecini ajunge la gradul
$0$, îl vom adăuga în coadă.

Complexitate $O(N + \sum K)$.

## Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
// OJI 2025 11-12 Ivan Andrei
// Aventura ASSERT 1+2+3+4
#include <bits/stdc++.h>

using namespace std;

const int max_size = 5e5 + 5;

vector<int> nxt[max_size];
int deg[max_size], sumall;
queue<int> q;

void solve() {
    int n;
    cin >> n;
    assert(n <= 500000);
    for (int i = 1; i <= n; i++) {
        nxt[i].clear();
    }
    int sumk = 0;
    for (int i = 1; i <= n; i++) {
        int k;
        cin >> k;
        sumk += k;
        if (k == 0) {
            q.push(i);
        }
        deg[i] = k;
        assert(deg[i] >= 0);
        while (k--) {
            int x;
            cin >> x;
            nxt[x].push_back(i);
            assert(x != i);
        }
    }
    assert(sumk <= 4 * n);
    int ans = 0;
    sumall += sumk;
    while (!q.empty()) {
        int nod = q.front();
        q.pop();
        ans++;
        for (auto f : nxt[nod]) {
            deg[f]--;
            if (deg[f] == 0) {
                q.push(f);
            }
        }
    }
    cout << ans;
    cout << '\n';
}

int main() {
#ifdef LOCAL
    freopen("test.in", "r", stdin);
    freopen("test.out", "w", stdout);
#else
    freopen("aventura.in", "r", stdin);
    freopen("aventura.out", "w", stdout);
#endif
    int t;
    cin >> t;
    assert(t <= 5);
    // t = 1;
    while (t--) {
        solve();
    }
    assert(sumall <= 5000000);
    return 0;
}
```
