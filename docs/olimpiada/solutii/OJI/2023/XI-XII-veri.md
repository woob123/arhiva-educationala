---
id: OJI-2023-XI-XII-veri
title: Soluția problemei Veri (OJI 2023, clasele XI-XII)
problem_id: 502
authors: [ulmeanu]
prerequisites:
    - graphs
    - shortest-path
tags:
    - OJI
    - clasa XI-XII
---

## Subtask 1 (30 puncte)

Tot graful este un ciclu. Primul drum are lungimea $n$. Următorul drum are
lungimea $(n + A - S) \mod n$, analog pentru ultimul drum.

Complexitate: $O(n)$.

## Subtask 2 (50 puncte)

Observația principală a problemei implică recunoașterea formei drumului optim:

Trebuie să existe un nod $i$ astfel încât drumurile $s \rightarrow i$,
$i \rightarrow a$ și $i \rightarrow b$ au lungime minimă și ne putem folosi
de cel mai scurt ciclu care începe și se termină în $i$.

Toate constrângerile de mai sus pot fi rezolvate cu algoritmul Roy-Floyd,
cu două mențiuni:

- $d[i][j]$ va fi folosit pentru ciclul de lungime minimă care începe și se
termină în $i$, $\forall i \in {1, \dots, n}$.
- Pentru reconstruirea oricărui drum, trebuie să reținem o matrice
suplimentară $p$. Dacă am ameliorat lungimea drumului $i \rightarrow j$ cu
ajutorul nodului $z$, atunci actualizăm $p[i][j] \leftarrow z$. Când vrem
să reconstruim drumul $i \rightarrow j$ de lungime minimă, construim
recursiv și unim drumurile $i \rightarrow p[i][j]$ și $p[i][j] \rightarrow j$.

Trebuie să reconstruim 4 drumuri, fiecare în $O(n^2)$. Complexitatea totală
a algoritmului este $O(n^3)$.

## Subtask 3 (20 puncte)

Căutăm ciclul optim $(i \rightarrow i)$ astfel: oricum ar arăta ciclul,
există un arbore BFS al grafului astfel încât ciclul să fie reprezentat de
un lanț în jos din rădăcină, închis de un back edge înapoi în rădăcină.

Constrângerile ne permit să construim fiecare arbore BFS din graf (adică să
facem $n$ BFS-uri, unul pentru fiecare rădăcină posibilă), care vor lua
$O(n(n + m)) \in O(n^2)$, deoarece $m \leq 4n$.

Putem reconstrui orice drum în $O(n)$, deci soluția are complexitatea 
$O(n^2)$ pentru acest subtask. Pentru celelalte subtaskuri, soluția are
complexitatea $O(n^3)$, încadrându-se în limita de timp.

## Rezolvare

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <bits/stdc++.h>

using namespace std;

ifstream f("veri.in");
ofstream g("veri.out");

const int NMAX = 5e3 + 5, INF = 1e9;

int dist[NMAX][NMAX], dist1[NMAX];
int parent[NMAX][NMAX];

vector<int> v[NMAX];
queue<int> q;

void bfs(int src) {
    dist[src][src] = 0;
    q.push(src);

    while (!q.empty()) {
        int nod = q.front();
        q.pop();

        for (auto& i : v[nod]) {
            if (src == i && !dist1[src]) {
                dist1[src] = dist[src][nod] + 1;
                parent[src][src] = nod;
            } else if (dist[src][nod] + 1 < dist[src][i]) {
                dist[src][i] = dist[src][nod] + 1;
                parent[src][i] = nod;
                q.push(i);
            }
        }
    }
}

void inapoi(int nod, int start, bool rep) {
    if (nod == start && rep == false)
        g << start << " ";
    else 
        if (nod != start) {
            inapoi(parent[start][nod], start, rep);
            g << nod << " ";
        }
}

int main() {
    int cer;
    f >> cer;

    int n, m, s, a, b;
    f >> n >> m >> s >> a >> b;

    for (int i = 1; i <= m; i++) {
        int x, y;
        f >> x >> y;
        v[x].push_back(y);
    }

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            dist[i][j] = INF;
        }
    }

    for (int i = 1; i <= n; i++) {
        bfs(i);
    }

    int ind;
    int ans = INF;

    for (int i = 1; i <= n; i++) {
        int aux = dist[s][i] + dist1[i] + dist[i][a];
        int aux1 = dist[s][i] + dist1[i] + dist[i][b];

        if (max(aux, aux1) < ans && dist1[i] && dist[s][i] != INF && dist[i][a] != INF && dist[i][b] != INF) {
            ans = max(aux, aux1), ind = i;
        } 
        else if (max(aux, aux1) == ans && dist1[i] && dist[s][i] != INF && dist[i][a] != INF && dist[i][b] != INF &&
                   dist[s][i] + dist1[i] < dist[s][ind] + dist1[ind]) {
            ind = i;
        }
    }

    if (cer == 1) {
        g << ans;
    } 
    else {
        g << dist[s][ind] + dist1[ind] << '\n';
        inapoi(ind, s, 0);
        inapoi(parent[ind][ind], ind, 1);
        g << ind << '\n';

        g << dist[ind][a] << '\n';
        inapoi(a, ind, 0);
        g << '\n';

        g << dist[ind][b] << '\n';
        inapoi(b, ind, 0);
    }

    return 0;
}
```
