---
id: OJI-2023-XI-XII-parcare
title: Soluția problemei Parcare (OJI 2023, clasele XI-XII)
problem_id: 500
authors: [bsitaru]
prerequisites:
    - greedy
    - stl
tags:
    - OJI
    - clasa XI-XII
---

## Subtask 1 (24 puncte)

Deoarece fiecare mașină stă exact o secundă, iar timpii de sosire și
plecare sunt distincți, deducem că fiecare mașină va fi singură în parcare,
deci se poate afișa locul $1$. Dacă ultima mașină are timpul de plecare cel
mult egal cu $T$, atunci se va afișa timpul său de sosire urmat de $N - 1$
de valori $-1$, altfel se vor afișa $N$ de $-1$.

Complexitate: $O(N + M)$.

## Subtask 2 (26 puncte)

Se va atribui câte un loc de parcare fiecărei mașini în ordinea sosirii și
se va afișa locul respectiv. După sosirea tuturor mașinilor, se vor elibera
locurile de parcare ale mașinilor al căror timp de plecare este cel mult $T$
și se va afișa configurația parcării.

Complexitate: $O(N + M)$.

## Subtask 3 (26 puncte)

Se va procesa fiecare mașină în ordinea sosirii. Mai întâi se vor elibera
locurile de parcare ale mașinilor care au timpul de plecare cel mult egal
cu timpul actual. Apoi se va căuta secvențial un loc de parcare pentru
mașina tocmai sosită. La final, se vor elibera locurile de parcare ale
mașinilor cu timpul de plecare mai mic sau egal cu $T$ și se va afișa
configurația parcării.

Complexitate: $O(N \cdot M)$.

## Subtask 4 (24 puncte)

Pentru a ști în fiecare moment dacă există locuri libere în parcare, se va
folosi o structură de heap (set) în care se vor reține perechi de forma
$(t, j)$, unde $t$ reprezintă secunda la care locul $j$ va fi liber.
Inițial, în heap, toate perechile vor fi de forma $(0, j)$ pentru orice $j$ de
la $1$ la $N$. Pentru fiecare mașină sosită la secunda $s_i$ cu plecare la $p_i$,
dacă perechea din heap $(t, j)$, cu $t$ minim, verifică condiția $t < s_i$,
atunci se elimină din heap această pereche, se afișează locul j disponibil
pentru mașină și se adaugă în heap perechea $(p_i, j)$. În caz contrar, se va
afișa $-1$ deoarece nu există loc de parcare pentru mașina curentă. La final,
se eliberează locurile de parcare pentru mașinile care au timpul de plecare
cel mult $T$ și se afișează configurația parcării.

Complexitate: $O(M \cdot \log N)$

## Soluție alternativă

Deoarece la fiecare moment de timp se întâmplă maxim un eveniment
(venirea/plecarea unei mașini), putem reține pentru fiecare secundă
informațiile ce descriu acest eveniment. Astfel, pentru fiecare secundă
reținem una sau două valori: $0$ - niciun eveniment, $1$ - venirea unei mașini,
$2 \ x$ - plecarea mașinii de pe locul de parcare $x$. În același timp, vom
menține locurile de parcare libere într-o stivă/coadă, pentru a putea găsi
în timp constant un loc de parcare liber sau a vedea dacă parcarea este
plină. Astfel, putem itera prin secunde de la $1$ la $T$ și procesa
evenimentele. La venirea unei mașini la timpul s_i care va sta până la
timpul $p_i$, verificăm dacă stiva este goală, caz în care afișăm $-1$. În caz
contrar, vom obține un loc liber de parcare $x$ (din vârful stivei), pe care
îl vom elimina din stivă și îl vom aloca mașinii curente, după care vom
actualiza configurația parcării și informațiile despre evenimentul de
plecare al mașinii la timpul $p_i$. La plecarea unei mașini, vom actualiza
configurația parcării și vom insera noul loc gol de parcare în vârful
stivei. După procesarea evenimentelor din intervalul de timp $[1, T]$, afișăm
configurația parcării.

Complexitate: $O(N + M)$.

## Rezolvare

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <fstream>
#include <set>
using namespace std;
ifstream f("parcare.in");
ofstream g("parcare.out");
int n, m, t, i, h, j, s, p, timp, poz;
int v[200005];
set<pair<int, int> > mul;
set<pair<int, int> >::iterator it;

int main() {
    f >> n >> m >> t;
    for (i = 1; i <= n; i++) {
        mul.insert({0, i});
        v[i] = -1;
    }
    for (i = 1; i <= m; i++) {
        f >> s >> p;
        it = mul.begin();
        timp = (*it).first;
        poz = (*it).second;
        if (timp < s) {
            g << poz << "\n";
            mul.erase(it);
            mul.insert({p, poz});
            v[poz] = s;
        } 
        else
            g << "-1\n";
    }
    while (!mul.empty()) {
        it = mul.begin();
        timp = (*it).first;
        poz = (*it).second;
        if (timp <= t)
            v[poz] = -1;
        else
            break;
        mul.erase(it);
    }
    for (i = 1; i <= n; i++) 
        g << v[i] << " ";

    return 0;
}
```
