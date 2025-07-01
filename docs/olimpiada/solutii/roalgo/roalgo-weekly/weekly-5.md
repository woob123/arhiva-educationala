---
id: roalgo-weekly-4
title: Descrierea soluțiilor RoAlgo Weekly Contest 4
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
[RoAlgo Weekly Contest #5](https://kilonova.ro/contests/1669/). Acest concurs nu
ar fi putut avea loc fără sprijinul vostru și fără ajutorul testerilor
concursului, care sunt prezenți în lista de editori a concursului de pe Kilonova.

Această serie de concursuri va continua în fiecare săptămână, scopul fiind acela
de a deveni un concurs similar în popularitate cu rundele de pe LeetCode și nu
numai.

## Problema [joc](https://kilonova.ro/problems/3032)

Pentru a rezolva această problemă, vom simula operațiile conform instrucțiunilor
din enunț, afișând într-un final răspunsul cerut.

### Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <fstream>
using namespace std;

int main() {
    ifstream cin ("joc.in");
    ofstream cout ("joc.out");

    int n;
    long long x;
    cin >> n >> x;

    for (int i = 0; i < n; i++) {
        int op;
        cin >> op;
        if (op == 1) {
            x = x * 2;
        } 
        else if (op == 2) {
            x = x * 4 + 1;
        } 
        else if (op == 3) {
            x = x / 2;
        }
    }

    cout << x << "\n";
    return 0;
}
```


## Problema [conturi](https://kilonova.ro/problems/3098)

Pentru a rezolva această problemă, vom afla pentru fiecare persoană banca,
genul și suma de bani pe care o deține, iar mai apoi vom folosi aceste
informații pentru a afla persoana de gen masculin cu cei mai mulți bani.

### Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <fstream>
using namespace std;

int main() {
    ifstream cin("conturi.in");
    ofstream cout("conturi.out");
    
    int N, X;
    cin >> N >> X;
    
    int best = 0;
    for (int i = 0; i < N; i++) {
        int c;
        cin >> c;
        
        int bank = c / 100000;
        int gender = (c / 10000) % 10;
        int balance = c % 10000;
        
        if (bank == X) {
            if (gender == 1) {
                if (balance > best) {
                    best = balance;
                }
            }
        }
    }
    
    cout << best << "\n";
    return 0;
}

```

## Problema [numere](https://kilonova.ro/problems/3034)

Vom afla pentru fiecare număr câți divizori are, iar mai apoi vom ține numărul
maxim de divizori și valoarea care conține acel număr de divizori.

### Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <fstream>
using namespace std;

int main() {
    ifstream cin("numere.in");
    ofstream cout("numere.out");

    int n;
    cin >> n;

    long long best_val = 0;
    int best_divs = -1;

    for (int i = 0; i < n; i++) {
        long long a;
        cin >> a;
        int divi = 0;
        for (long long d = 1; d * d <= a; d++) {
            if (a % d == 0) {
                if (d * d == a) {
                    divi += 1;
                } 
                else {
                    divi += 2;
                }
            }
        }
        if (divi > best_divs) {
            best_divs = divi;
            best_val = a;
        }
    }

    cout << best_val << "\n";
    return 0;
}
```

## Problema [card](https://kilonova.ro/problems/3041)

Pentru a rezolva această problemă, vom procesa rezultatul cifră cu cifră,
folosindu-ne de faptul că nu trebuie să păstrăm prea multe cifre în răspuns.

### Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <fstream>
using namespace std;

int main() {
    ifstream f("card.in");
    ofstream g("card.out");
    int x, k, i, a, b, c, t;
    char s;
    f >> x >> k;
    a = 1;
    b = 1;
    if (x > 9) k = k / 2;
    do {
        c = a + b;
        a = b;
        b = c;
    } while (c != k);
    if (x <= 9)
        t = a;
    else
        t = 2 * a;
    for (i = 1; i <= t; i++) {
        f >> s;
        g << s;
        if (i % 1000 == 0) {
            g << '\n';
        }
    }
	return 0;
}
```
