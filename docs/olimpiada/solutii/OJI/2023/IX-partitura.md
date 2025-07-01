---
id: OJI-2023-IX-partitura
title: Soluția problemei partitura (OJI 2023, clasa a IX-a)
problem_id: 507
authors: [cismaru]
prerequisites:
    - greedy
    - sorting
    - basic-math
tags:
    - OJI
    - clasa IX
draft: true
---

!!! note "Observație"
    Toate demonstrațiile lemelor folosite în demonstrarea
    corectitudinii soluției acestei probleme se pot găsi la finalul
    acestui articol în secțiunea Demonstrații problema partitură.

## Cazul $n = 4$ și $x = 1$

Să presupunem că avem 4 note muzicale de înălțimi $a \geq b \geq c \geq d$ și durata $\frac{1}{2}$.

Lema 1: Suma maximă se va obține prin împerecherea notelor astfel: $(a, b)$ și $(c, d)$.

## Cazul $x = 1$

Extindem soluția anterioară și considerăm că avem șirul de note
muzicale cu înălțimi $a_1 \geq a_2 \geq a_3 \geq \dots \geq a_n$, toate
având durata $\frac{1}{2}$.

Lema 2. Suma maximă se va obține prin împerecherea notelor astfel:

$(a_1, a_2), (a_3, a_4), \dots, (a_{2i+1}, a_{2i+2}), \dots, (a_{n−1}, a_n)$.

Acest caz se poate rezolva prin sortarea șirului de note muzicale
descrescător în funcție de înălțime și însumând pătratul sumei
perechilor de două câte două elemente consecutive.

Complexitate temporală: $O(n \log n)$. 
Complexitate spațială: $O(n)$.

## Caz general

Lema 3. Se împerechează mai întâi toate notele muzicale cu durată
$\frac{1}{2}$ conform Lema 2, apoi se împerechează toate notele cu
durată $\frac{1}{4}$ s.a.m.d. până se consumă toate notele.

Acest caz se poate implementa prin sortarea notelor muzicale crescător
după durata $\frac{1}{2^x}$, iar la egalitate după înălțimea $y$. Se împarte
acest șir în $x_max$ șiruri în funcție de durată și se împerechează
toate notele de pe fiecare strat în ordine descrescătoare a duratelor
(de la $\frac{1}{2^{x_{max}}}$ la $\frac{1}{2}$).

Pentru a implementa eficient, este necesară o singură sortare inițială,
ulterior adăugarea notelor de durată $1/2^x$ obținute prin împerecherea
celor de durată $\frac{1}{2^{x−1}}$ la cele inițiale de durată
$\frac{1}{2^x}$ putând fi făcută prin interclasare.

Complexitate temporală: $O(n \log n)$.
Complexitate spațială: $O(n)$.

## Rezolvare

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<long long> v[20];
int main() {
    ifstream cin("partitura.in");
    ofstream cout("partitura.out");
    int n, x, y, i, j;
    long long ans = 0;
    cin >> n;
    for (i = 1; i <= n; i++) {
        cin >> x >> y;
        v[x].push_back(y);
    }
    for (i = 18; i > 0; i--) {
        sort(v[i].begin(), v[i].end());
        for (j = 0; j + 1 < v[i].size(); j += 2)
            v[i - 1].push_back(v[i][j] + v[i][j + 1]);
    }
    for (i = 0; i < v[0].size(); i++) 
        ans += 1ll * v[0][i] * v[0][i];
    cout << ans << '\n';
    return 0;
}
```

## Demonstrații problema partitură

**Lema 1.** Vrem să demonstrăm că pentru patru numere $a \geq b \geq c \geq d$,
suma pătratelor sumei a două câte două numere este maximizată de
soluția: $(a + b)^2 + (c + d)^2$.

Demonstrație: Vom începe prin a calcula cele trei sume posibile:

1. $(a + b)^2 + (c + d)^2 = a^2 + b^2 + c^2 + d^2 + 2ab + 2cd$
2. $(a + c)^2 + (b + d)^2 = a^2 + b^2 + c^2 + d^2 + 2ac + 2bd$
3. $(a + d)^2 + (b + c)^2 = a^2 + b^2 + c^2 + d^2 + 2ad + 2bc$

Vom arăta că diferența $(1) - (2)$ este pozitivă:

$a^2 + b^2 + c^2 + d^2 + 2ab + 2cd − a^2 − b^2 − c^2 − d^2 − 2ac − 2bd$

$2ab + 2cd − 2ac − 2bd$

$ab + cd − ac − bd$

$a(b − c) − d(b − c)$

$(b − c) ⋅ (a − d)$

Cum $b \geq c$ și $a \geq d$, rezultă că diferența $(1) - (2)$ este
pozitivă. Similar se demonstrează că diferența $(1) - (3)$ este și ea
pozitivă.

Astfel, dintre toate cele trei combinații, suma $(a + b)^2 + (c + d)^2$
este cea mai mare.

Definiție 1. Numim inversiune, oricare 2 perechi de numere $(x, z)$,
$(y, t)$ sau $(x, t)$, $(y, z)$, unde $x \geq y \geq z \geq t$.

Lema 2. Vrem să demonstrăm că pentru un șir de numere
$a_1 \geq a_2 \geq \dots \geq a_n$, suma pătratelor sumei a două câte
două numere este maximizată de soluția $(a_1, a_2), (a_3, a_4), \dots,
(a_{2i+1}, a_{2i+2}), \dots, (a_{n−1}, a_n)$.

Demonstrație: Presupunem că soluția ce maximizează suma pătratelor
sumei de două câte două numere conține cel puțin o inversiune. Conform
Lema 1, suma cu care contribuie această inversiune la suma totală nu
este optimă, ci poate fi îmbunătățită prin eliminarea inversiunii.

Astfel, soluția nu este optimă, deci am ajuns la o contradicție – o
soluție nu poate conține nicio inversiune. Singura configurație ce nu
conține nicio inversiune este:

$(a_1, a_2), (a_3, a_4), \dots, (a_{2i+1}, a_{2i+2}), \dots, (a_{n−1}, a_n)$

Lema 3. Vrem să demonstrăm că soluția optimă se obține prin rezolvarea
optimă a fiecărui grup de note de durate egale, începând cu cele cu
durate cât mai mari.

Demonstrație: Vom considera o soluție optimă în care știm grupele de
note de durată $\frac{1}{2}$. Considerăm toate cele $k$ grupe din
această soluție ce conțin cel puțin o notă de durată minimă ordonate
după suma $S_1 \geq S_2 \geq S_3 \geq \dots \geq S_k$.

Fie o notă de durată minimă de înălțime $x$ din grupa de sumă $S = a + x$
și o notă de durată minimă de înălțime $y$ din altă grupă $S′ = b + y$.
Dacă $x \leq y$, atunci soluția nu este optimă deoarece dacă am face o
interschimbare între $x$ și $y$, atunci suma se va îmbunătăți cu:

$(a + x)^2 + (b + y)^2 − (a + y)^2 − (b + x)^2$

$= a^2 + 2ax + x^2 + b^2 + 2by + y^2 − a^2 − 2ay − y^2 − b^2 − 2bx − x^2$

$= 2ax + 2by − 2ay − 2bx$

$= 2(ax + by − ay − bx)$

$= 2(x − y)(b − a)$

Care este un număr pozitiv.

Astfel, grupa cu cea mai mare sumă ce conține note de durată minimă va
conține sufixul de note de durată minimă ordonate după înălțimea
maximă. Deci, toate notele de durată minimă vor fi selectate în perechi
de două câte două în ordine descrescătoare.

În continuare, putem crește durata minimă a unei note considerând
aceste note împerecheate ca fiind o singură notă de lungime dublă și
deci reducem problema de la o durată minimă $\frac{1}{2^x}$ la $\frac{1}{2^{x−1}}$.
Acest proces se aplică recursiv până când obținem doar grupe de durată $\frac{1}{2}$.
