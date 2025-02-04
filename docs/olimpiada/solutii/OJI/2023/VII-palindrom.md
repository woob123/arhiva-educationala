---
tags:
    - OJI
    - clasa VII
---

# Soluția problemei Palindrom (OJI 2023, clasa a VII-a)

!!! example "Cunoștințe necesare"
    - [Prelucrarea cifrelor](../../../../usor/digits-manipulation.md)
    - [Simularea soluției](../../../../usor/simulating-solution.md)

**Autor soluție**: Emanuela Cerchez

!!! note "Link problemă"
    Această problemă poate fi accesată [aici](https://kilonova.ro/problems/513/).

Cerința 1. Vom citi succesiv numerele și vom determina pentru fiecare număr citit numărul minim de cifre care trebuie să fie adăugate pentru a transforma numărul respectiv în palindrom. Pentru numere mari (de maximum 50 de cifre), vom citi fiecare număr caracter cu caracter, până la întâlnirea marcajului de sfârșit de linie și vom reține cifrele numărului într-un vector.  

Pentru punctaj parțial (numere de maximum 18 cifre), se va citi numărul într-o variabilă de tip $\text{long long int}$, apoi se vor extrage cifrele numărului și se vor plasa într-un vector. Pentru a determina numărul minim de cifre care trebuie adăugate la finalul numărului pentru a transforma acest număr în palindrom, putem determina lungimea celui mai lung sufix al numărului care are proprietatea de a fi palindrom. Să notăm această lungime cu $lgs$, iar lungimea numărului cu $lg$. Numărul minim de cifre care trebuie să fie adăugate este $nr = lg - lgs$ (se adaugă la final primele $nr$ cifre ale numărului în ordine inversată). Desigur, pentru punctaj parțial este posibilă și o abordare „prin încercări”:  

- dacă numărul este deja palindrom, $nr = 0$;  
- dacă numărul nu este palindrom, adăugăm la sfârșitul lui o cifră (prima cifră a numărului) și verificăm dacă se obține un palindrom (în acest caz $nr$ va fi 1);  
- apoi încercăm cu două cifre, trei cifre, ș.a.m.d.;  
- în cel mai defavorabil caz vom adăuga $nr = lg - 1$ cifre (primele $lg - 1$ cifre în ordine inversată).  

Nu este necesar să reținem toate numerele citite, vom reține într-un vector $nr$ cu $n$ elemente numărul minim de cifre care trebuie să fie adăugate pentru a transforma fiecare număr din șir în palindrom. Rezultatul la cerința 1 este suma valorilor memorate în vectorul $nr$.  

Cerința 2. Trebuie să determinăm cea mai lungă subsecvență a vectorului $nr$, construit la cerința anterioară, care are suma elementelor mai mică sau egală cu $S$. Pentru aceasta vom parcurge vectorul $nr$ cât timp suma elementelor din secvența curentă (să o notăm $sum$) este $\leq S$. Când am ajuns la o poziție $i$ pentru care $sum + nr[i] > S$, elementul curent nu mai poate fi „înghițit” în soluție. Prin urmare:  

- comparăm lungimea secvenței curente cu lungimea maximă și o reținem dacă este mai mare;  
- eliminăm elementele de la începutul secvenței curente, actualizând corespunzător $sum$, până când este posibil să „înghițim” valoarea $nr[i]$ în soluție ($sum + nr[i] \leq S$).  

Când parcurgerea vectorului $nr$ s-a încheiat, trebuie să comparăm și lungimea ultimei secvențe cu lungimea maximă.  

Pentru punctaje parțiale sunt posibile și abordări de complexitate $O(n^2)$ sau $O(n^3)$.  


Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <bits/stdc++.h>
using namespace std;

int prevAns[50005];

int main() {
    ifstream cin("palindrom.in");
    ofstream cout("palindrom.out");

    int cer, n, i, s, j, k, ans, st, dr, sum;
    string str, aux;

    cin >> cer >> n;

    ans = 0;
    for (i = 1; i <= n; i++) {
        cin >> str;

        for (j = 0; j < (int)str.size(); j++) {
            st = j, dr = (int)str.size() - 1;
            while (st < dr && str[st] == str[dr]) {
                st++;
                dr--;
            }

            if (st >= dr) {
                break;
            }
        }

        ans += j;
        prevAns[i] = j;
    }

    if (cer == 1) {
        cout << ans;
    } 
    else {
        cin >> s;

        ans = INT_MIN;
        sum = 0, j = 1;
        for (i = 1; i <= n; i++) {
            sum += prevAns[i];
            while (sum > s) {
                sum -= prevAns[j++];
            }
            ans = max(ans, i - j + 1);
        }

        cout << ans;
    }

    return 0;
}
```