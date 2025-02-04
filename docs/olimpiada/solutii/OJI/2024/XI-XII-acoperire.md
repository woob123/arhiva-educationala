---
tags:
    - OJI
    - clasa XI-XII
---

# Soluția problemei Acoperire (OJI 2024, clasele XI-XII)

!!! example "Cunoștințe necesare"
    - [Introducere în Metoda Greedy](../../../../usor/greedy.md)
    - [Căutarea binară](../../../../usor/binary-search.md)


**Autor soluție**: Mihai Ciucu

!!! note "Link problemă"
    Această problemă poate fi accesată [aici](https://kilonova.ro/problems/2509/).

Subtask 1 (10 puncte). Pentru $N = 1$, doar trebuie afișat intervalul de la capătul stânga la mijlocul intervalului.

Subtask 2 (10 puncte). Când sunt două intervale disjuncte, pentru $K = 2$ doar se face [start,mijloc] pentru fiecare, iar pentru $K = 1$ sunt două cazuri, ca la subtaskul 3.

Subtask 3 (20 puncte). Când $K = 1$ soluția tot timpul este ori $[primulMijloc, ultimulMijloc]$, ori se termină în ultimul mijloc și trebuie văzut pentru fiecare interval în parte unde poate începe cel mai târziu.

Subtask 4 (20 puncte). Când toate intervalele nu se intersectează, ele pot fi sortate oricum, căci bănuim că unii concurenți instinctiv le sortează după unul din capete și apoi încearcă un greedy. Problemele de genul de obicei implică o ordonare și parcurgere a evenimentelor, doar aici trebuia văzut că evenimentele interesante sunt mijloacele.

Observația de bază a problemei este că dacă un segment nou acoperă un segment inițial, atunci trebuie să acopere mijlocul segmentului inițial. Dacă intervalul de acoperiri are lungimea minim jumătate din intervalul initial, este și garantat suficient. Echivalent ar fi să spunem că orice interval de lungime minim $L/2$ acoperă un interval inițial de lungime $L$ dacă și numai dacă conține mijlocul intervalului inițial.

Ca să ajungem la concluzia de mai sus, considerăm un interval oarecare $[A, B]$. Dacă ne gândim care este segmentul minim cel mai din stânga care îl acoperă, care ar fi intervalul $[A, (A+B)/2]$, și cel mai din dreapta, adică $[(A+ B)/2, B]$, vedem că doar elementul din mijloc este intersecția lor.

De aici, putem vedea că un interval de acoperire dintr-o soluție validă poate fi translatat fără a-i afecta corectitudinea până când unul din capete coincide cu unul din mijloacele intervalelor acoperite inițial. Facem asta pentru a putea genera toate intervalele de acoperire pornind de la puncte definite de segmentele inițiale.

Pentru soluția finală căutăm binar lungimea minimă a celui mai lung interval, folosind întrebări de forma: ”Dacă toate intervalele de acoperire au lungima $L$, am putea acoperi cu cel mult $K$ intervale mulțimea celor initiale?”

Întrebarea nouă este mai simplă, și o răspundem printr-o parcurgere a intervalelor inițiale, ordonate după mijloacele lor: Începem un interval de lungime $L$ în primul mijloc și considerăm acoperite toate segmentele cu mijlocul conținut în acesta. Când întâlnim un segment nou care nu este acoperit de ultimul interval adăugat, înseamnă că trebuie să începem un nou interval în mijlocul intervalului inițial. Complexitatea pentru pasul acesta este $O(N)$ odată ce intervalele sunt sortate la început după mijloc.

Pentru soluția minimă lexicografic tot ce trebuie făcut e să vedem că dacă intervalele trebuie să înceapă cât mai din stânga, este echivalent cu a vrea ca intervalele să se termine cât mai în stânga, și deci doar facem algoritmul în sens invers după mijloace (pornim ultimul interval în ultimul mijloc, etc.).

Pentru a nu fi erori de precizie, se dublează în implementare capetele intervalelor, astfel încât mijloacele să fie întregi. La afișare se poate face cu: `cout « setprecision(2) « (x / 2.0)` (fără fixed) sau tratând explicit numere pare sau impare, cu caz particular pentru afișarea lui $-1$.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <bits/stdc++.h>
using namespace std;

bool cmp(pair<int, int> a, pair<int, int> b) { 
    return (a.first + a.second) < (b.first + b.second); 
}
int main() {
    ifstream cin("acoperire.in");
    ofstream cout("acoperire.out");

    int n;
    cin >> n;

    vector<pair<int, int>> vp(n + 1);
    int minlength = 0;
    for (int i = 1; i <= n; i++) {
        cin >> vp[i].first >> vp[i].second;
        minlength = max(minlength, vp[i].second - vp[i].first);
    }
    sort(vp.begin() + 1, vp.begin() + n + 1, cmp);

    int q;
    cin >> q;

    for (; q; q--) {
        int k;
        cin >> k;

        int st = minlength;
        int dr = 2000000000;
        int ans = dr;
        vector<pair<double, double>> vans;
        while (st <= dr) {
            int mid = (st + dr) / 2;
            double actualsize = 0.5000 * mid;
            vector<pair<double, double>> intervals;
            double lstend = -1;
            for (int i = 1; i <= n; i++) {
                double midd = (vp[i].first + vp[i].second) * 0.500;
                if (lstend < midd) {
                    intervals.push_back({midd, midd + actualsize});
                    lstend = midd + actualsize;
                }
            }
            if (intervals.size() <= k) {
                vans = intervals;
                ans = mid;
                dr = mid - 1;
            } else
                st = mid + 1;
        }

        double actualsize = 0.5000 * ans;
        vector<pair<double, double>> intervals;
        double lstend = 1e9;
        for (int i = n; i >= 1; i--) {
            double midd = (vp[i].first + vp[i].second) * 0.500;
            if (lstend > midd) {
                intervals.push_back({midd - actualsize, midd});
                lstend = midd - actualsize;
            }
        }
        vans = intervals;

        cout << ans / 2 << (ans % 2 == 0 ? "\n" : ".5\n");
        cout << vans.size() << '\n';
        sort(vans.begin(), vans.end());
        for (auto x : vans) {
            int tval = (2.00 * x.first);
            int tval2 = (2.00 * x.second);
            if (tval == -1)
                cout << "-";
            cout << tval / 2 << (tval % 2 == 0 ? " " : ".5 ");
            if (tval2 == -1)
                cout << "-";
            cout << tval2 / 2 << (tval2 % 2 == 0 ? "\n" : ".5\n");
        }
    }
    return 0;
}
```