---
tags:
    - OJI
    - clasa IX
---

# Soluția problemei Nestemate (OJI 2024, clasele XI-XII)

!!! example "Cunoștințe necesare"
    - [Introducere în STL](../../../../cppintro/stl.md)
    - [Introducere în teoria grafurilor](../../../../usor/graphs.md)

**Autor soluție**: Mihaela Cismaru

!!! note "Link problemă"
    Această problemă poate fi accesată [aici](https://kilonova.ro/problems/2508/).

Subtask 1 (11 puncte). Se verifică dacă cele două nestemate au cel puțin o proprietate ce coincide, caz în care declarăm că este necesară o singură transformare, în caz contrar se va afișa $−1$.

Subtask 2 (13 puncte). Se încearcă toate nestematele pentru a vedea dacă pot fi folosite ca stare intermediară sau dacă nestemata $A$ poate fi transformată direct in nestemata $B$. Dacă nu este posibila transformarea in niciunul din aceste moduri se va afișa $−1$.

Subtask 3 (16 puncte). Se încearcă toate combinațiile de 2 nestemate intermediare, se încearcă doar cu o piatră intermediară și se încearcă și dacă este posibilă transformarea directă. Se afișează numărul minim de transformări iar în caz că nu este posibilă transformarea în niciunul din aceste moduri se va afișa $−1$.

Subtask 4 (10 puncte). Folosind o abordare de tip backtracking, încercăm să luăm toate posibilitățile de transformări succesive. Reținem lungimea minimă a unui lanț de transformări valid cât și dacă s-a reușit găsirea a cel puțin un astfel de lanț.

Subtask 5 (10 puncte). Modelăm relațiile dintre nestemate ca un graf. Pentru fiecare două nestemate se stabilește dacă au o proprietate în comun și prin urmare fiecare din aceste pietre poate fi transformată în cealaltă (transformarea este mereu bidirecțională). Se va crea o matrice de adiacență și se va căuta drumul minim de la nestemata $A$ la nestemata $B$. Drumul minim poate fi găsit printr-un algoritm precum parcurgerea în lățime (BFS). Pentru această cerință este necesară o rezolvare în complexitatea $O(N^2)$.

Subtask 6 (13 puncte). Se vor genera muchiile într-o complexitate mai bună folosindu-ne de valorile prezente în nestemate. Pentru fiecare valoare se va genera o listă de nestemate ce conțin valoarea respectivă în configurație. Pentru fiecare valoare, toate nestematele din cadrul unei liste vor fi unite fiecare cu fiecare printr-o muchie. Se va avea grijă să nu se genereze de mai multe ori o muchie între aceleași două nestemate. Se va căuta drumul minim de la nestemata $A$ la nestemata $B$ printr-un algoritm precum parcurgerea în lățime (BFS). Deoarece la această cerință se garantează că o valoare apare la maxim 3 nestemate distincte, la un pas nu vor fi generate mai mult de 3 muchii. Dimensiunea grafului va fi rezonabilă pentru o complexitate de $O(N + MaxVal)$ unde $MaxVal$ este cea mai mare valoare dintre proprietățile nestematelor.

Subtask 7 (27 de puncte). Pentru soluția finală nu se va genera deloc graful deoarece numărul de muchii poate deveni foarte mare. Pentru fiecare valoare vom ține liste cu toate nestematele ce conțin valoarea respectivă. Vom folosi o abordare de parcurgere în lățime (BFS) pentru a găsi distanța minimă. La fiecare pas pornind de la o nestemată luăm toate proprietățile acesteia și explorăm doar listele proprietăților ce nu au mai fost întâlnite până în acel moment. Fiecare listă a unei proprietăți va fi parcursă exact o dată pe parcursul algoritmului. Parcurgem listele si adăugăm în coadă doar nestematele ce nu au mai fost parcurse până în acel moment. Această abordare se încadrează în complexitatea optimă $O(N + MaxVal)$ unde $MaxVal$ este cea mai mare valoare dintre proprietățile nestematelor.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <bits/stdc++.h>
using namespace std;

struct data {
    int v[3];
};
int main() {
    ifstream cin("nestemate.in");
    ofstream cout("nestemate.out");

    int t;
    cin >> t;

    for (; t; t--) {
        int n, a, b;
        cin >> n >> a >> b;

        vector<data> nodes(n + 1);
        vector<int> cost(n + 1, (1 << 30)), colors(500001, (1 << 30)), passed(500001, 0);
        cost[a] = 0;

        vector<vector<int>> who(500001);
        for (int i = 1; i <= n; i++) {
            cin >> nodes[i].v[0] >> nodes[i].v[1] >> nodes[i].v[2];
            who[nodes[i].v[0]].push_back(i);
            who[nodes[i].v[1]].push_back(i);
            who[nodes[i].v[2]].push_back(i);
        }

        queue<int> q;
        q.push(a);

        while (!q.empty()) {
            int nod = q.front();
            q.pop();

            for (int i = 0; i <= 2; i++)
                if (colors[nodes[nod].v[i]] > cost[nod]) {
                    colors[nodes[nod].v[i]] = cost[nod];
                    for (auto x : who[nodes[nod].v[i]]) {
                        if (cost[x] > cost[nod] + 1) {
                            cost[x] = cost[nod] + 1;
                            q.push(x);
                        }
                    }
                }
        }

        cout << (cost[b] == (1 << 30) ? -1 : cost[b]) << '\n';
    }
    return 0;
}
```