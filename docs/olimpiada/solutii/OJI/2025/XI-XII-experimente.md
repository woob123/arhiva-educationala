---
id: OJI-2025-XI-XII-experimente
title: Soluția problemei Experimente (OJI 2025, clasele XI-XII)
problem_id: 3617
authors: [grigorean]
prerequisites:
    - stl
    - stl-sorting-searching
tags:
    - OJI
    - clasa XI-XII
    - structuri de date
    - stl
---

Observație inițială: Problema poate fi reformulată astfel: dându-se un
șir circular și update-uri sub forma unor intervale continue, să se
calculeze după fiecare update cardinalul intersecției tuturor
intervalelor de până atunci.

## Subtask 1

Restricțiile pentru acest subtask implică faptul că putem considera
șirul ca fiind necircular. În acest caz intersectia unor intervale va
fi tot timpul un singur interval continuu.

Este de ajuns să reținem cel mai mare capăt de start și cel mai mic
capăt de final al intevalelor de update pentru a afla răspunsul.

## Subtaskurile 2, 3, 4, 5

Pentru celelalte subtaskuri, intersecția update-urilor va fi reprezentată
de o mulțime $M$ formată din mai multe intervale, nu doar unul singur
(inițial considerăm $M$ formată dintr-un singur interval $[1, N]$).

În momentul în care avem un update nou $U$, pentru fiecare interval $I$
din $M$ există $3$ scenarii posibile:

1. $I$ este inclus complet in $U$, trebuie păstrat în $M$;
2. $I$ nu se intersecsează cu $U$, trebuie scos din M;
3. $I$ și $U$ se intersectează fără ca $I$ să fie complet inclus în
   $U$, $I$ trebuie înlocuit.

Pentru scenariul 3, facem următoarele observații:

-   există maxim $2$ intervale $I$ care se pot afla în această situație;
-   I trebuie înlocuit fie cu un alt interval, fie cu alte două intervale.

în funcție de structura de date aleasă, se pot rezolva diferite
subtaskuri. Soluția oficială folosește un set pentru o complexitate
de $O(M \log M)$.

## Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
// Andrei Grigorean CS Academy
#include <cassert>
#include <iostream>
#include <limits>
#include <set>
using namespace std;

const int MAX_N = (int)1e9;
const int MAX_M = (int)1e5;

int n, m;
int ans;
set<pair<int, int>> s;

void addInterval(int left, int right) {
    ans += right - left + 1;
    s.insert({left, right});
}

void removeInterval(set<pair<int, int>>::iterator it) {
    ans -= it->second - it->first + 1;
    s.erase(it);
}

void splitIntersections(int pos) {
    auto entry = s.lower_bound({pos + 1, numeric_limits<int>::min()});
    if (entry != s.begin()) {
        entry = prev(entry);
    }
    if (entry == s.end()) {
        return;
    }

    int left = entry->first, right = entry->second;
    if (pos > left && pos <= right) {
        removeInterval(entry);
        addInterval(left, pos - 1);
        addInterval(pos, right);
    }
}

int main() {
    assert(freopen("experimente.in", "r", stdin));
    assert(freopen("experimente.out", "w", stdout));

    cin >> n >> m;
    assert(1 <= n && n <= MAX_N);
    assert(1 <= m && m <= MAX_M);
    addInterval(0, n - 1);

    for (int i = 1; i <= m; ++i) {
        int left, right;
        cin >> left >> right;
        assert(0 <= left && left < n);
        assert(0 <= right && right < n);
        if (i > 1) {
            left = (left + ans) % n;
            right = (right + ans) % n;
        }

        splitIntersections(left);
        splitIntersections(right + 1);

        if (left <= right) {
            while (!s.empty() && s.begin()->second < left) {
                removeInterval(s.begin());
            }
            while (!s.empty() && prev(s.end())->first > right) {
                removeInterval(prev(s.end()));
            }
            cout << ans << "\n";
        } else {
            auto startIt =
                s.lower_bound({right + 1, numeric_limits<int>::min()});
            auto endIt = startIt;
            while (endIt != s.end() && endIt->second < left) {
                ans -= endIt->second - endIt->first + 1;
                endIt = next(endIt);
            }
            s.erase(startIt, endIt);
            cout << ans << "\n";
        }
    }
    return 0;
}
```
