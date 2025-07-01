---
id: OJI-2022-V-ceas
title: Soluția problemei ceas (OJI 2022, clasa a V-a)
problem_id: 941
authors: [apintea]
prerequisites:
    - digits-manipulation
tags:
    - OJI
    - clasa V
---

Articolul va fi disponibil curând în arhivă.

Până atunci, editorialul poate fi accesat în repo-ul nostru de GitHub, linkul fiind [acesta](https://github.com/roalgo-discord/Romanian-Olympiad-Solutions/blob/main/OJI%20(regional%20olympiad)/2022/05.pdf).

## Rezolvare

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
/*

C = 1 -> se numara cifrele in mod standard, atentie la cazul x = 0

C = 2 -> se simuleaza grupele, din nou trebuie atentie la cifrele egale cu 0
raspunsul final este numarul de grupe create - n

*/

#include <fstream>
#include <iostream>
using namespace std;

int main() {
    ifstream cin("ceas.in");
    ofstream cout("ceas.out");

    int c, x, n;
    cin >> c >> x >> n;

    int ans1 = 0, ans2 = 0;
    for (int i = 1; i <= n; i++) {
        int nr;
        cin >> nr;
        int grupa = -1;
        if (nr == 0) {
            if (x == 0) ans1++;
            ans2++;
        }
        while (nr) {
            if (nr % 10 == x) ans1++;
            if (grupa == -1)
                ans2++, grupa = nr % 10;
            else if ((nr % 10) != 0 && (nr % 10) * 10 + grupa <= 12)
                grupa = -1;
            else
                ans2++, grupa = nr % 10;
            nr /= 10;
        }
    }
    if (c == 1)
        cout << ans1 << '\n';
    else
        cout << ans2 - n << '\n';
    return 0;
}
```
