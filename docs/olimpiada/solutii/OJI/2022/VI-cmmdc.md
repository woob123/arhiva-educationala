---
id: OJI-2022-VI-cmmdc
title: Soluția problemei cmmdc (OJI 2022, clasa a VI-a)
problem_id: 651
authors: [pracsiu]
prerequisites:
    - divisibility
    - partial-sums
tags:
    - OJI
    - clasa VI
---

Articolul va fi disponibil curând în arhivă.

Până atunci, editorialul poate fi accesat în repo-ul nostru de GitHub, linkul fiind [acesta](https://github.com/roalgo-discord/Romanian-Olympiad-Solutions/blob/main/OJI%20(regional%20olympiad)/2022/06.pdf).

## Rezolvare

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <bits/stdc++.h>
#define ll long long
 
using namespace std;
 
long long cmmdc(long long a, long long b)
{
    long long c;
    while(b)
    {
        c = a%b;
        a = b;
        b = c;
    }
    return a;
} 
 
long long v[100002], prfs[100002], prfd[100002];
long long prf[2002][2002];
 
int main() 
{
    
    ifstream cin("cmmdc.in");
    ofstream cout("cmmdc.out");
    
    int c, n;
    cin >> c >> n;
    
    for(int i = 1; i <= n; i++)
        cin >> v[i];
        
    if(c == 1)
    {
        long long ans = v[1];
        for(int i = 2; i <= n; i++)
            ans = cmmdc(ans, v[i]);
        cout << ans;
        return 0;
    }
    else
    {
        if(c == 2)
        {
            for(int i = 1; i <= n; i++)
                prfs[i] = cmmdc(prfs[i-1], v[i]);
            for(int i = n; i >= 1; i--)
                prfd[i] = cmmdc(prfd[i+1], v[i]);
            ll ans = max(prfs[n-1], prfd[2]); // alegem 1 sau n
            for(int i = 2; i < n; i++)
                ans = max(ans, cmmdc(prfs[i-1], prfd[i+1]));
                
            cout << ans << '\n';
        }
        if(c == 3)
        {
            for(int i = 1; i <= n; i++)
            {
                prf[i][i] = v[i];
                for(int j = i+1; j <= n; j++)
                    prf[i][j] = cmmdc(prf[i][j-1], v[j]);
            }
            long long ans = 0;
            for(int poz1 = 1; poz1 <= n; poz1++)
                for(int poz2 = poz1+1; poz2 <= n; poz2++)
                {
                    long long c1 = 0;
                    long long c2 = 0;
                    long long c3 = 0;
                    if(poz1 > 1)
                        c1 = prf[1][poz1-1];
                    if(poz2 - 1 > poz1)
                        c2 = prf[poz1+1][poz2-1];
                    if(poz2 < n)
                        c3 = prf[poz2+1][n];
                    ans = max(ans, cmmdc(cmmdc(c1, c2), c3));
                }
            cout << ans << '\n';
        }
    }
    return 0;
}
```
