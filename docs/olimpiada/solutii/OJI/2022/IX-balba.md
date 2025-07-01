---
id: OJI-2022-IX-balba
title: Soluția problemei balba (OJI 2022, clasa a IX-a)
problem_id: 641
authors: [gharmand]
prerequisites:
    - ad-hoc
    - simulating-solution
tags:
    - OJI
    - clasa IX
---

Articolul va fi disponibil curând în arhivă.

Până atunci, editorialul poate fi accesat în repo-ul nostru de GitHub, linkul fiind [acesta](https://github.com/roalgo-discord/Romanian-Olympiad-Solutions/blob/main/OJI%20(regional%20olympiad)/2022/09.pdf).

## Rezolvare

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ifstream cin("balba.in");
    ofstream cout("balba.out");
    
    int c, n;
    cin >> c >> n;
    
    vector<int> v(n+1), fr(10), fr2(10);
    for (int i = 1; i <= n; i++) {
        cin >> v[i];
        fr[v[i]]++;
        fr2[v[i]]++;
    }
    
    if (c == 1) {
        int grp = 0, consec = 0, strk = 0;
        for (int i = 1; i <= n; i++) {
            if (i == 1 || v[i] != v[i-1]) {
                grp++;
                strk = 1;
            }
            else {
                strk++;
                if (strk == 2) {
                    consec++;
                }
            }
        }
        cout << grp << '\n';
        cout << consec << '\n';
    }
    else {
        string s;
        for (int i = 9; i >= 0; i--) {
            if (i == 0 && s.empty())
                break;
            while (fr[i] >= 2) {
                s += (char) ('0' + i);
                fr[i] -= 2;
            }
        }
        
        // group all chars w/ even frequency and add the biggest odd in the middle
        string ans = s;
        
        int biggestodd = -1;
        for (int i = 9; i >= 0; i--) 
            if (fr[i] == 1) {
                biggestodd = i;
                break;
            }
            
        if (biggestodd != -1) 
            ans += (char) ('0' + biggestodd);
        
        for (int i = (int) s.size() - 1; i >= 0; i--) 
            ans += s[i];
        
        string ans2, ans3;
        
        // starting from string made of even frequency, add one more char 
        
        for (int i = 0; i < (int) s.size(); i++) 
            if (fr[s[i] - '0']) {
                for (int j = 0; j < i; j++) 
                    ans2 += s[j];
                ans2 += s[i];
                for (int j = i; j < (int) s.size(); j++) 
                    ans2 += s[j];
                for (int j = (int) s.size() - 1; j >= 0; j--) 
                    ans2 += s[j];
                break;
            }
        
        for (int i = 0; i < (int) s.size(); i++) 
            if (fr[s[i] - '0']) {
                for (int j = 0; j < (int) s.size(); j++) 
                    ans3 += s[j];
                for (int j = (int) s.size() - 1; j >= i; j--) 
                    ans3 += s[j];
                ans3 += s[i];
                for (int j = i-1; j >= 0; j--) 
                    ans3 += s[j];
                break;
            }
        
        string finalans = ans;
        if (ans2.size() > finalans.size() || (ans2.size() == finalans.size() && ans2 > finalans))
            finalans = ans2;
        if (ans3.size() > finalans.size() || (ans3.size() == finalans.size() && ans3 > finalans))
            finalans = ans3;
        
        // fix middle char 
        
        for (int digit = 0; digit <= 9; digit++) {
            if (fr2[digit])
            {
                fr2[digit]--;
                string ss;
                for (int i = 9; i >= 0; i--) {
                    if (i == 0 && ss.empty())
                        break;
                    while (fr2[i] >= 2) {
                        ss += (char) ('0' + i);
                        fr2[i] -= 2;
                    }
                }
                string variant;
                for (int j = 0; j < (int) ss.size(); j++)
                    variant += ss[j];
                variant += (char) ('0' + digit);
                
                for (int i = 0; i + 1 < (int) variant.size(); i++) {
                    if (fr2[variant[i] - '0']) {
                        string anss;
                        for (int j = 0; j < i; j++)
                            anss += variant[j];
                        anss += variant[i];
                        for (int j = i; j < (int) variant.size(); j++)
                            anss += variant[j];
                        for (int j = (int) variant.size() - 2; j >= 0; j--)
                            anss += variant[j];
                        if (anss.size() > finalans.size() || (anss.size() == finalans.size() && anss > finalans))
                            finalans = anss;
                        break;
                    }
                     
                }
            
                for (int j = 0; j < (int) ss.size(); j++)
                    fr2[ss[j] - '0'] += 2;
                fr2[digit]++;
            }
        }
        cout << finalans << '\n';
    }
    return 0;
}
```
