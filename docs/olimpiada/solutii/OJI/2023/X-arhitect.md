---
id: OJI-2023-X-arhitect
title: Soluția problemei arhitect (OJI 2023, clasa a X-a)
problem_id: 504
authors: [diac]
prerequisites:
    - stl
    - basic-geometry
tags:
    - OJI
    - clasa X
---

Segmentele sunt paralele cu axele sau bisectoarele. Atunci segmentele
au pantele cu unghiuri de 0°, 45°, 90° sau 135° și se pot număra
segmentele care sunt aliniate de la început, adică verticale deci
$x_1 = x_2$ sau orizontale deci $y_1 = y_2$, fie numărul acestora $a$. 

Toate celelalte segmente se pot alinia printr-o rotație cu 45°. Deci
răspunsul va fi $max(a, N - a)$: fie nu rotim segmentele, fie le rotim
cu 45°.

## Soluție $O(N^2)$ aproximativ $50$ puncte

Fixăm orice segment $1 \leq i \leq N$ pentru a fi aliniat. Prin rotirea
tuturor segmentelor cu unghiul necesar alinierii lui $i$, se vor alinia
și alte segmente: cele care sunt paralele și cele care sunt
perpendiculare pe $i$.

Putem evita implementarea efectivă a rotațiilor
dacă doar numărăm aceste segmente printr-o altă parcurgere. Pentru a
testa dacă două segmente sunt paralele, putem translata un segment
astfel încât segmentele să aibă un capăt comun. Segmentele sunt
paralele dacă triunghiul format are aria zero. Segmentele sunt
perpendiculare dacă în triunghiul format se verifică teorema lui Pitagora.

## Soluție O(N log N) punctaj maxim

Putem reprezenta fiecare segment printr-un singur punct dacă translatăm
toate segmentele astfel ca un capăt al lor să fie $(0, 0)$. Mai departe,
pentru simplitate, mutăm orice segment cu valoarea y negativă în
simetricul lui față de $(0, 0)$, adică $(x, y < 0)$ devine $(−x, −y)$,
iar panta este aceeași. 

Acum, pentru a trata cazul segmentelor
perpendiculare, orice segment cu $x < 0$ poate fi rotit cu 45° și astfel
vom avea toate segmentele reprezentate de puncte cu ambele coordonate
pozitive. Rotația cu 45° se poate aplica transformând punctul $(x < 0,
y)$ în punctul $(y, −x)$. 

În sfârșit, acum este suficient să calculăm
numărul maxim de segmente ce au aceeași pantă. Coordonatele fiind
naturale, putem reprezenta pantele prin perechi de coordonate
simplificate: $(x, y)$ devine $(x / cmmdc(x, y), y / cmmdc(x, y))$. Acum
trebuie să determinăm numărul maxim de segmente egale. Pentru aceasta,
putem sorta segmentele după pantă sau coordonate.

Apoi, printr-o
singură parcurgere, găsim numărul maxim de segmente de pe poziții
consecutive egale sau, alternativ, segmentele pot fi numărate printr-un
tablou asociativ, adică un map<>. În funcție de operațiile care se
folosesc pentru calcule, trebuie folosit tipul long long evitând depășirile.

## Rezolvare

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <fstream>
#include <map>

using namespace std;

ifstream fin("arhitect.in");
ofstream fout("arhitect.out");

map<pair<int, int>, int> M;

int cmmdc(int a, int b) {
    while (b != 0) {
        int r = a % b;
        a = b;
        b = r;
    }
    return a;
}

int main() {
    int n, fmax = 0;
    fin >> n;
    for (int i = 1; i <= n; i++) {
        int x1, y1, x2, y2;
        fin >> x1 >> y1 >> x2 >> y2;
        if (x1 > x2) 
            swap(x1, x2), swap(y1, y2);
        int x = x2 - x1, y = y2 - y1;
        if (y < 0)
            swap(x, y), x = -x;
        else if (x == 0)
            swap(x, y);
        if (x == 0)
            y = 1;
        else {
            int d = cmmdc(x, y);
            x /= d, y /= d;
        }
        M[{y, x}]++;
    }
    for (auto x : M)
        if (x.second > fmax) 
            fmax = x.second;
    fout << fmax;
    return 0;
}
```
