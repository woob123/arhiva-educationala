---
id: OJI-2010-VIII-cladiri
title: Soluția problemei cladiri (OJI 2010, clasa a VIII-a)
problem_id: 801
authors: [odomsa]
prerequisites:
    - basic-math
tags:
    - OJI
    - clasa VIII
---

Articolul va fi disponibil curând în arhivă.

Până atunci, editorialul poate fi accesat în repo-ul nostru de GitHub, linkul fiind [acesta](https://github.com/roalgo-discord/Romanian-Olympiad-Solutions/blob/main/OJI%20(regional%20olympiad)/2010/08/cladiri.pdf).

## Rezolvare

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <bits/stdc++.h>
using namespace std;

ifstream fin("cladiri.in");
ofstream fout("cladiri.out");

const int MAXLEVEL = 1e4;

int affectedOnLevel[MAXLEVEL + 1];

int main() {
    int longitude, lng, latitude, lat, intensity, grad, level, numAffected, maxAffectedLevel;

    fin >> longitude >> latitude >> intensity;

    numAffected = 0;
    while(fin >> lng >> lat >> grad){
        level = max(abs(longitude - lng), abs(latitude - lat)); // The distance till the center
        if(grad + level <= intensity){ // Is the building affected?
            numAffected++;
            affectedOnLevel[level]++; 
        }
    }

    maxAffectedLevel = 0;
    for(level = 0; level <= MAXLEVEL; level++){
        if(affectedOnLevel[level] > affectedOnLevel[maxAffectedLevel]){
            maxAffectedLevel = level;
        }
    }

    fout << numAffected << '\n' << affectedOnLevel[maxAffectedLevel] << '\n';

    if(affectedOnLevel[maxAffectedLevel] != 0){
        for(level = 0; level <= MAXLEVEL; level++){
            if(affectedOnLevel[level] == affectedOnLevel[maxAffectedLevel]){
                fout << level << ' ';
            }
        }
    }
    return 0;
}
```
