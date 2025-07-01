---
id: roalgo-weekly-0
title: Descrierea soluțiilor RoAlgo Weekly Contest 0
authors: [stefdasca]
prerequisites:
    - basic-math
    - arrays
    - strings
    - divisibility
tags:
    - clasa
    - bacalaureat
    - clasa IX
---

Vă mulțumim că ați luat parte la concursul
[RoAlgo Weekly Contest #0](https://kilonova.ro/contests/1457/). Acest concurs nu
ar fi putut avea loc fără sprijinul vostru și fără ajutorul testerilor
concursului, care sunt prezenți în lista de editori a concursului de pe Kilonova.

Această serie de concursuri va continua în fiecare săptămână, scopul fiind acela
de a deveni un concurs similar în popularitate cu rundele de pe LeetCode și nu
numai.

## Problema [parimpar](https://kilonova.ro/problems/3625)

Pentru a rezolva această problemă, trebuie să simulăm operațiile din enunț
conform algoritmului descris. Pentru a interschimba caracterele, vom folosi
metoda celor trei pahare, având o variabilă auxiliară în care vom păstra unul
dintre caractere.

### Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <iostream>
#include <cstring>
using namespace std;

int main() {
    char S[101];
    cin >> S;

    for(int i = 0; i < strlen(S); i = i + 2) {
        char auxiliar = S[i];
        S[i] = S[i+1];
        S[i+1] = auxiliar;
    }

    cout << S;
    return 0;
}
```

## Problema [sumsecv](https://kilonova.ro/problems/3626)

Pentru a rezolva această problemă, trebuie doar să parcurgem subsecvența delimitată de pozițiile $L$ și $R$ de la fiecare interogare și să păstrăm suma totală într-o variabilă numită $sum$.

### Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <iostream>
using namespace std;
 
int main(){
    
    int n, m, a[101];
    cin >> n >> m;
    for (int i = 1; i <= n; i++){
        cin >> a[i];
    }
    
    for (int q = 1; q <= m; q++){
        int l, r;
        cin >> l >> r;
        int sum = 0;
        for (int i = l; i <= r; i++){
            sum += a[i];
        }
        cout << sum << '\n';
    }
    
    return 0;
}
```

## Problema [GCDness](https://kilonova.ro/problems/3627)

Pentru a rezolva această problemă, putem observa faptul că răspunsul pe care îl
căutăm va fi mereu în intervalul $[2, 1000]$. Deoarece avem foarte puține valori
în șir, este îndeajuns să verificăm toate valorile și să vedem care ar fi
valoarea cu cel mai mare GCDness.

### Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <iostream>
using namespace std;
 
int main() {
    int n, v[101];
    cin >> n;

    for (int i = 1; i <= n; i++){
         cin >> v[i];
    }
    
    int bestK = 2, bestCount = 0;
    for (int k = 2; k <= 1000; k++){
        int cnt = 0;
        for (int i = 1; i <= n; i++) {
            if (v[i] % k == 0) {
                cnt++;
            }
        }
        if(cnt > bestCount){
            bestCount = cnt;
            bestK = k;
        }
    }
    cout << bestK;
    return 0;
}
```

## Problema [identice](https://kilonova.ro/problems/3628)

Rezolvarea acestei probleme presupune păstrarea pentru fiecare cifră a poziției
celei mai din stânga și celei mai din dreapta apariții a valorii în șir, iar
pentru fiecare cifră de la $0$ la $9$, vom verifica dacă lungimea acelei secvențe
este mai mare decât lungimea maximă aflată. Trebuie avut grijă și la criteriul de
departajare.

### Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <iostream>
using namespace std;

int main() {
    int V[100001], St[10] = {0}, Dr[10] = {0}, n, i, maxim = 0, st = 1, dr = 1;

    cin >> n;

    for(i = 1; i <= n; i++)
        cin >> V[i];

    for(i = 1; i <= n; i++) {
        if(St[V[i]] == 0)
            St[V[i]] = i;
        Dr[V[i]] = i;
    }

    for(i = 0; i <= 9; i++)
        if(Dr[i] != 0 && St[i] != 0)
            if(Dr[i] - St[i] + 1 > maxim) {
                maxim = Dr[i] - St[i] + 1;
                st = St[i];
                dr = Dr[i];
            }
            else if(Dr[i] - St[i] + 1 >= maxim && St[i] < st) {
                st = St[i];
                dr = Dr[i];
            }

    cout << st << " " << dr;

    return 0;
}
```
