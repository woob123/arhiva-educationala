---
tags:
    - OJI
    - clasa V
---

# Soluția problemei Microbist (OJI 2024, clasa a V-a)

!!! example "Cunoștințe necesare"
    - [Maxime și minime](../../../../usor/maxime-minime.md)

**Autor soluție**: Dorin-Mircea Rotar

!!! note "Link problemă"
    Această problemă poate fi accesată [aici](https://kilonova.ro/problems/2517/).

Cerința 1: Pentru a afla scorul final, vom parcurge șirul de goluri și vom număra câte goluri a marcat fiecare echipă. Pentru aceasta putem folosi două variabile contor pe care să le actualizăm în momentul citirii în funcție de echipa care marchează.

Cerința 2: Pentru a determina câte scoruri au fost egale, vom ține evidența numărului de goluri marcate de fiecare echipă, la fel ca mai sus, și vom incrementa un contor de fiecare dată când scorurile devin egale.

Cerința 3: Putem determina secvențe de elemente egale aflate pe poziții consecutive. O astfel de secvență este revenire în forță dacă atunci când ea începe, echipa pe care o reprezintă este condusă și atunci când se termină echipa pe care o reprezintă conduce cu un gol.

Detectarea și calcularea revenirii în forță pentru cerința 3:

După primul gol, se actualizează scorul echipei corespunzătoare și se determină echipa care este condusă. Pentru următoarele goluri marcate vom ține cont de următoarele aspecte:

- dacă golul curent este diferit de cel anterior (marchează altă echipă) atunci testăm dacă putem începe o nouă revenire în forță și în caz afirmativ stabilim contorul de revenire la unu.
- dacă golul curent este marcat de aceeași echipă ca și golul anterior atunci creștem contorul pentru revenirea în forță și dacă s-a schimbat echipa câștigătoare.

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <fstream>
using namespace std;

int main() {
    ifstream cin("microbist.in");
    ofstream cout("microbist.out");

    int c, n, ga = 0, gb = 0, eq = 1, strk = 0, maxstrk = 0, curr = 0;
    cin >> c >> n;
    
    for (int i = 1; i <= n; i++) {
        int nr;
        cin >> nr;
        if (nr == 1)
            ga++;
        else
            gb++;
        if (ga == gb)
            eq++;
        if (i == 1)
            strk = 1;
        else
            if (nr == curr)
                strk++;
            else
                strk = 1;
        curr = nr;
        if (curr == 1) {
            if (ga > gb && ga - strk < gb && ga - gb == 1) {
                maxstrk = max(maxstrk, strk);
            }
        }
        else {
            if (gb > ga && gb - strk < ga && gb - ga == 1) {
                maxstrk = max(maxstrk, strk);
            }
        }
    }
    if (c == 1) {
        cout << ga << " " << gb << '\n';
        return 0;
    }
    
    if (c == 2) {
        cout << eq << '\n';
        return 0;
    }
    
    if (c == 3) {
        cout << maxstrk << '\n';
    }
    return 0;
}
```