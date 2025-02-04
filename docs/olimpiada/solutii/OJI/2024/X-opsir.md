---
tags:
    - OJI
    - clasa X
---

# Soluția problemei Opsir (OJI 2024, clasa a X-a)

!!! example "Cunoștințe necesare"
    - [Șiruri de caractere](../../../../cppintro/strings.md)
    - [Introducere în Metoda Greedy](../../../../usor/greedy.md)
    - [Abordarea problemelor ad-hoc](../../../../mediu/ad-hoc.md)

**Autor soluție**: Ștefan-Alexandru Popescu

!!! note "Link problemă"
    Această problemă poate fi accesată [aici](https://kilonova.ro/problems/2505/).

## Cerința 1 

Subtask 1

Pentru fiecare șir se construiește un vector de frecvență ce memorează numărul de apariții al fiecărei litere. Pe baza acestora se determină apoi câte caractere distincte există în cele 2 șiruri.

Pentru fiecare caracter distinct ce apare se determină în care dintre șiruri apare de mai multe ori folosindu-se vectorii de frecvență.

## Cerința 2

Subtask 2 - $S$ e sortat

În această situație putem aplica o operație de tip 2 pe tot șirul $T$, eliminând apoi literele care apar în plus. Abordarea e echivalentă cu a verifica dacă, pentru fiecare caracter distinct, frecvența sa în $T$ e mai mare decât frecvența sa în $S$.

Subtask 3 - $S$ poate fi obținut doar prin operații de tip 1

Problema e echivalentă cu a verifica daca $S$ e subșir al lui $T$, ceea ce se poate face cu un algoritm Greedy.

Subtask 4 - fără restricții

Să presupunem că există o succesiune de lungime $L$ de operații de tip 1 și 2 care pot transforma șirul $T$ în șirul $S$ (adica e validă ca soluție): $op_1, op_2, op_3 \dots op_L$. Dacă încercăm să interschimbăm 2 operații consecutive din această succesiune (fie ele $op_i$ și $op_{i+1}$, $1 \leq i < L$) pentru a obține tot o succesiune validă, putem observa următoarele cazuri:

- $op_i$ și $op_{i+1}$ sunt operații de tip 1: echivalent cu a șterge 2 poziții din șirul $T$. Acestea pot fi șterse în orice ordine (cu o eventuală recalculare de indecși)
- $op_i$ și $op_{i+1}$ sunt operații de tip 2: din criteriul colorării din cerință știm că intervalele sunt disjuncte, deci nu contează ordinea în care se realizează aceste operații
- $op_i$ și $op_{i+1}$ sunt operații de tipuri diferite: echivalent cu a șterge o poziție și a sorta un interval, operații care se pot realiza în orice ordine (cu eventuale recalculări de indecși)

Aceste observații ne arată faptul că putem interschimba 2 operații consecutive dintr-o succesiune validă (cu anumite modificări) pentru a obține tot o succesiune validă. Considerăm în continuare că ”am adus”, prin interschimbări, operațiile de tip 1 pe primele poziții din succesiune.

De asemenea, considerăm că am efectuat deja aceste operații de tip 1. Astfel, obținem
din șirul $T$ un șir de lungime $n$ și o mulțime de intervale disjuncte ce corespund sortărilor ce urmează a fi efectuate.

Să considerăm partiționarea șirului $S$ în subsecvențe crescătoare maximale. Fie p numărul subsecvențelor din această partiționare. În acest caz, orice interval din mulțimea precizată anterior va avea capetele în aceeași subsecvență din această partiționare. Observăm că, dacă înlocuim toate operațiile de tip 2 rămase cu câte o operație de tip 2 pentru fiecare subsecvență din partiționarea șirului $S$ obținem același rezultat.

Astfel, folosindu-ne din nou de interschimbări, putem obține o succesiune de operații în care au loc mai întâi toate sortările, iar fiecare element din șirul $T$ este inclus în cel puțin o sortare.

Problema se reduce, în această situație, la a verifica dacă există o partiționare a șirului $T$ în $p$ subsecvențe, astfel încât frecvențele literelor din fiecare subsecvență a lui $T$ să fie mai mari sau egale decât cele din subsecvența corespondentă din $S$ (din partiționarea de mai sus).

Verificarea se poate face prin următorul algoritm Greedy: pentru fiecare subsecvență din partiționarea lui $S$ (subsecvențele fiind luate de la stânga la dreapta) se determina prefixul de lungime minimă a lui $T$ pentru care frecvențele literelor sale sunt mai mare sau egale decât cele din subsecvența actuală a lui $S$. Se elimină acest prefix și se continuă cu următoarea subsecvență din $S$. Dacă pentru fiecare subsecvență a lui $S$ se găsește un astfel de prefix, atunci răspunsul este DA, altfel răspunsul este NU.

Complexitate timp: $O(n \cdot dimensiune_{alfabet})$.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ifstream cin("opsir.in");
    ofstream cout("opsir.out");

    int c, t;
    cin >> c;
    cin >> t;

    for (; t; t--) {
        int n, m;
        cin >> n >> m;
        string s, t;
        cin >> s >> t;

        vector<int> frq(26), frq2(26);
        for (int i = 0; i < n; i++)
            frq[s[i] - 'a']++;
        for (int i = 0; i < m; i++)
            frq2[t[i] - 'a']++;

        if (c == 1) {
            int cnt = 0;
            for (int i = 0; i < 26; i++) {
                if (frq[i] || frq2[i])
                    cnt++;
            }
            cout << cnt << '\n';
            for (int i = 0; i < 26; i++) {
                if (frq[i] || frq2[i]) {
                    if (frq[i] > frq2[i])
                        cout << (char)('a' + i) << " " << "S\n";
                    else
                        cout << (char)('a' + i) << " " << "T\n";
                }
            }
        } else {
            int poz = 0;
            int poz2 = 0;
            bool ok = 1;
            while (poz < n && poz2 < m) {
                vector<int> frqq(26);
                vector<int> frqq2(26);
                int cntdif = 0;
                while (poz < n) {
                    frqq[s[poz] - 'a']++;
                    if (frqq[s[poz] - 'a'] == 1)
                        cntdif++;
                    poz++;
                    if (poz < n && s[poz] < s[poz - 1])
                        break;
                }
                while (poz2 < m && cntdif) {
                    frqq2[t[poz2] - 'a']++;
                    if (frqq2[t[poz2] - 'a'] == frqq[t[poz2] - 'a'])
                        cntdif--;
                    poz2++;
                }
                if (cntdif)
                    ok = 0;
            }
            if (poz == n && ok)
                cout << "DA\n";
            else
                cout << "NU\n";
        }
    }
    return 0;
}
```