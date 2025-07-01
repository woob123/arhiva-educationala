---
id: roalgo-weekly-3
title: Descrierea soluțiilor RoAlgo Weekly Contest 3
authors: [stefdasca]
prerequisites:
    - basic-math
    - arrays
    - matrices
    - strings
tags:
    - clasa
    - bacalaureat
    - clasa IX
---

Vă mulțumim că ați luat parte la concursul
[RoAlgo Weekly Contest #3](https://kilonova.ro/contests/1583/). Acest concurs nu
ar fi putut avea loc fără sprijinul vostru și fără ajutorul testerilor
concursului, care sunt prezenți în lista de editori a concursului de pe Kilonova.

Această serie de concursuri va continua în fiecare săptămână, scopul fiind acela
de a deveni un concurs similar în popularitate cu rundele de pe LeetCode și nu
numai.

## Problema [sticla](https://kilonova.ro/problems/3688)

Pentru a rezolva această problemă, vom simula procesul folosind o structură
repetitivă de tip _while_, alternând cei trei copii până când nu va mai fi
suficientă apă pentru copilul care este la rând.

### Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <iostream>
using namespace std;
 
int main() {
    
    int n, a, b, c;
    cin >> n >> a >> b >> c;
    
    while (true) {
        if (n >= a) {
            n -= a;
        }
        else {
            cout << 'F';
            return 0;
        }
        if (n >= b) {
            n -= b;
        }
        else {
            cout << 'M';
            return 0;
        }
        if (n >= c) {
            n -= c;
        }
        else {
            cout << 'T';
            return 0;
        }
    }
    return 0;
}
```


## Problema [abc](https://kilonova.ro/problems/3690)

Pentru a rezolva această problemă, trebuie să verificăm dacă șirul este o
succesiune de A, urmată de o succesiune de B și de o succesiune de C.
Acest lucru se poate face folosind câteva while-uri.

### Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <iostream>
#include <cstring>
using namespace std;
 
int main() {
    
    char c[101];
    cin >> c;
    
    int n = strlen(c);
    bool ok = 1;
    
    int i = 0;
    while (i < n && c[i] == 'A') {
        i++;
    }
    while (i < n && c[i] == 'B') {
        i++;
    }
    while (i < n && c[i] == 'C') {
        i++;
    }
    if (i == n) {
        cout << "Da";
    }
    else {
        cout << "Nu";
    }
    return 0;
}
```

## Problema [oglindit](https://kilonova.ro/problems/3689)

Pentru a rezolva această problemă, vom afla pentru unul din numere oglinditul
dacă considerăm doar cifrele impare, iar pentru celălalt vom afla numărul format
din cifrele impare, în ordinea inversă, având nevoie de puterile lui $10$ pentru
a face asta.

### Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
int imog(int a, int b) {
    int p10 = 1;
    int oga = 0;
    int ogb = 0;
    while (a > 0) {
        int x = a%10;
        if (x % 2 == 1) {
            oga += p10 * x;
            p10 *= 10;
        }
        a /= 10;
    }
    while (b > 0) {
        int x = b%10;
        if (x % 2 == 1) {
            ogb = ogb * 10 + x;
        }
        b /= 10;
    }
    if (oga != 0 && oga == ogb) {
        return 1;
    }
    return 0;
}
```

## Problema [suma](https://kilonova.ro/problems/3033)

Pentru a rezolva această problemă, trebuie să aflăm pentru fiecare număr câte
cifre are, iar dacă numărul are un număr impar de cifre, să tăiem $x/2$ cifre
din el, unde $x$ este acest număr de cifre.

### Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <iostream>
using namespace std;
 
int main() {
    
    int n;
    cin >> n;
    
    int sum = 0;
    while (n > 0) {
        int x;
        cin >> x;
        
        int cntcif = 0;
        int x2 = x;
        do {
            cntcif++;
            x2 /= 10;
        } while (x2 > 0);
        
        if (cntcif % 2 == 1) {
            cntcif /= 2;
            while (cntcif) {
                x /= 10;
                cntcif--;
            }
            sum += x%10;
        }
        n--;
    }
    
    cout << sum << '\n';
    return 0;
}
```
