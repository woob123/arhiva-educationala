---
id: OJI-2022-V-sss
title: Soluția problemei sss (OJI 2022, clasa a V-a)
problem_id: 942
authors: [nicoli]
prerequisites:
    - basic-math
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
Mai intai se afla ultima cifra nenula pentru cerinta 1. Apoi, e simplu de aflat suma ultimelor valori conform enuntului

Pentru cerinta 2, trebuie aflat numarul de grupe folosind sume gauss si apoi simulam suma fiecarei grupe
*/

#include <iostream>
#include <fstream>
using namespace std;

int main()
{
    ifstream cin("sss.in");
    ofstream cout("sss.out");
    
    int c, n;
    cin >> c >> n;
    
    int ans1 = 0, ans2 = 0;
    int lastdigit = 0, sm = 0;
    
    int grp = 1;
    while(grp * (grp+1) / 2 < n)
        grp++;
    for(int i = 1; i <= n; i++)
    {
        int x, x2;
        cin >> x;
        x2 = x;
        if(i == 1)
        {
            while(x)
            {
                if(x % 10 != 0)
                {
                    lastdigit = x%10;
                    break;
                }
                x /= 10;
            }
        }
        
        if(i >= n - lastdigit + 1)
            ans1 += x2;
        sm += x2;
        if(sm > ans2)
            ans2 = sm;
        if(n - i == grp * (grp-1) / 2)
            grp--, sm = 0;
    }
    
    if(c == 1)
        cout << ans1;
    else
        cout << ans2;
    return 0;
}
```
