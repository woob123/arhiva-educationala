---
id: tree-dp
author:
    - Ștefan-Cosmin Dăscălescu
prerequisites:
    - tree-1
    - intro-dp
    - rerooting
tags:
    - programare dinamica
    - arbori
---

## Introducere

În ceea ce privește studiul programării dinamice, un tip de probleme frecvent
întâlnit reprezintă dinamica pe arbori. Deși abordarea lor nu este foarte
diferită față de alte probleme care implică diverse recurențe, există câteva
sfaturi specifice care vă pot ajuta, împreună cu proprietăți specifice acestui
tip de probleme.

De obicei, atunci când abordăm aceste probleme, vrem să ne gândim la modul în
care un nod și copiii lui interacționează, de regulă starea pentru un nod $i$
(vom denumi generic această stare $dp[i]$, chiar dacă în multe situații vom avea
și alte dimensiuni) are rezultatul definit în funcție de rezultatele fiilor săi.
Modul în care găsim aceste relații, precum și diverse optimizări vor fi
prezentate pe parcursul acestui articol, prin diverse exemple.

Aceste probleme apar foarte des la olimpiadele și concursurile de informatică
românești, dinamicile pe arbore fiind un tip de problemă preferat de propunător,
dat fiind faptul că se regăsește aproape în fiecare an măcar la una din etapele
competiției, deci se impune cunoașterea celor mai importante abordări. De
asemenea, concursuri precum USACO Gold și Platinum au un număr ridicat de
asemenea probleme, de multe ori laolaltă cu diferite structuri de date.

## Problema [Tree Matching - CSES](https://cses.fi/problemset/task/1130/)

Pentru a rezolva această problemă, ne putem gândi la un caz foarte simplu, și
anume cazul când avem o rădăcină și un anumit număr de fii. În acest caz, ce
putem face este fie să nu conectăm nodul curent la un alt nod, fie să îl
conectăm la unul din acele noduri.

Astfel, putem deduce două tipuri de cazuri pe care le putem considera în
dinamica noastră, ceea ce motivează prezența unei dinamici cu două dimensiuni.
În mod formal, vom avea următoarele definiții:

- $dp[0][i]$ va reprezenta răspunsul maxim dacă nu am folosit nodul $i$ în vreo
  muchie.
- $dp[1][i]$ va reprezenta răspunsul maxim dacă am folosit nodul $i$ în vreo
  muchie.

Pentru a defini $dp[0][i]$, este simplu să ne gândim la faptul că vom vrea să
preluăm răspunsurile maxime de la fii, deci putem scrie $dp[0][i]$ ca fiind
$\sum \max(dp[0][x], dp[1][x])$ pentru toți fiii $x$ ai nodului $i$.

În mod similar, pentru a defini $dp[1][i]$, vom vrea să cuplăm nodul $i$ cu unul
din fiii săi, iar mai apoi să preluăm toate maximele de la celelalte noduri.
Astfel, $dp[1][i]$ va fi egal cu $\max (1 + dp[0][x]+ \sum \max(dp[0][x_0],
dp[1][x_0]))$, unde $x$ este fiul ales, iar $x_0$ reprezintă oricare din
ceilalți fii ai nodului $i$.

Implementarea acestei soluții se poate face folosind o parcurgere de tip DFS,
complexitatea fiind una liniară. Chiar dacă această problemă este una
introductivă, are foarte multe aspecte care sunt folosite în dinamicile pe
arbore, în special legate de combinarea subproblemelor și folosirea unei game
largi de cazuri pentru a ajunge la soluția dorită.

Mai jos puteți găsi codul sursă la această problemă.

```cpp
#include <iostream>
#include <vector>
 
using namespace std;
 
void dfs (int parent, int node, vector<vector<int>> &tree, vector<vector<int>> &dp) {
    int sum_mx = 0;
    for (auto x : tree[node]) {
        if (x != parent) {
            dfs(node, x, tree, dp);
            sum_mx += max(dp[1][x], dp[0][x]);
        }
    }
    
    dp[0][node] = sum_mx;
    for (auto x : tree[node]) {
        if (x != parent) {
            sum_mx -= max(dp[1][x], dp[0][x]);
            dp[1][node] = max(dp[1][node], 1 + dp[0][x] + sum_mx);
            sum_mx += max(dp[1][x], dp[0][x]);
        }
    }
}

int main() {
    
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    int n;
    cin >> n;
    
    vector<vector<int>> tree(n);
    for (int i = 0; i < n-1; i++) {
        int a, b;
        cin >> a >> b;
        a--, b--;
        tree[a].push_back(b);
        tree[b].push_back(a);
    }
    
    vector<vector<int>> dp(2, vector<int> (n));
    
    dfs(-1, 0, tree, dp);
    cout << max(dp[1][0], dp[0][0]) << '\n';
    return 0;
}
```

## Problema [The Fair Nut and Best Path](https://codeforces.com/problemset/problem/1083/A)

Pentru a calcula cantitatea maximă de combustibil pe care o putem avea la
finalul unui drum, vom vrea să ținem minte cantitatea maximă pe un lanț care
pleacă dintr-un nod anume spre unul din fiii săi, iar eventual să unim două
lanțuri pentru a putea considera cazul când rădăcina curentă reprezintă punctul
de intersecție a două asemenea drumuri.

Calcularea cantității maxime pentru un lanț este relativ facilă, deoarece
putem păstra o recurență de tip $dp[i]$ pentru fiecare posibil nod,
dar unirea a două lanțuri este mai dificilă deoarece trebuie să avem grijă
să evităm situațiile în care suma distanțelor este mai mare decât cantitatea
pe care o putem colecta din orașe.

Tratarea acestor cazuri se poate face într-un loop separat care pleacă
de la lanțul cel mai avantajos pe care l-am calculat, urmat de adăugarea
celui de-al doilea lanț, de preferat unul cât mai bun din punct de
vedere al benzinii rămase.

Soluția are o complexitate liniară, această problemă fiind instructivă
în contextul tratării cazurilor atunci când vine vorba de unirea a două
răspunsuri pentru a obține un răspuns final cât mai bun.

```cpp
#include <iostream>
#include <vector>

using namespace std;
int n;
long long amount[300002], maxanswer;
vector<pair<long long, long long>> v[300002];
long long dp[300002];

void dfs(int parent, int node) {
    int who = 0;
    for (int i = 0; i < v[node].size(); ++i) {
        int nxt = v[node][i].first;
        if (nxt == parent)
            continue;
        int cost = v[node][i].second;
        dfs(node, nxt);
        if (dp[nxt] - cost > dp[node])
            dp[node] = dp[nxt] - cost, who = nxt;
    }
    dp[node] += amount[node];
    maxanswer = max(maxanswer, dp[node]);
    for (int i = 0; i < v[node].size(); ++i) {
        int nxt = v[node][i].first;
        if (nxt == parent || nxt == who)
            continue;
        int cost = v[node][i].second;
        if (cost < dp[node] || cost < dp[nxt])
            maxanswer = max(maxanswer, dp[node] - cost + dp[nxt]);
    }
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    cin >> n;
    
    for (int i = 1; i <= n; ++i)
        cin >> amount[i], maxanswer = max(maxanswer, amount[i]);
        
    for (int i = 1; i < n; ++i) {
        long long a, b, c;
        cin >> a >> b >> c;
        v[a].push_back({b, c});
        v[b].push_back({a, c});
    }
    dfs(0, 1);
    cout << maxanswer;
    return 0;
}
```

## Problema [Arbore ONI 2016](https://kilonova.ro/problems/188)

Pentru a rezolva această problemă, ne vom gândi în mod similar cu prima problemă
la o soluție care va avea două cazuri pentru fiecare nod, dacă păstrăm acel nod
sau nu în arbore, abordare ce duce la calcularea atât a numărului maxim de
componente conexe, cât și a calculării numărului de moduri de a ajunge la acest
număr.

Vom avea două cazuri, dacă decidem să păstrăm nodul curent, vom
aduna răspunsurile maxime de la fiecare fiu, iar în caz contrar, vom
avea o abordare similară, diferența fiind aceea că vom aduna doar cazurile
când păstrăm nodurile.

Pentru mai multe detalii de implementare, puteți vedea soluția de mai jos.

```cpp
#include <fstream>
#include <vector>
#define mod 1000000007

using namespace std;

ifstream f("arbore.in");
ofstream g("arbore.out");

int n;
vector<int> v[100002];
int dp[3][100002];
long long ways[3][100002], sol, totalans;

void dfs(int dad, int node) {
    if (v[node].size() == 1 && v[node][0] == dad) {
        dp[1][node] = 1;
        ways[1][node] = 1;
        ways[2][node] = 1;
        return;
    }
    int max3 = 0;
    long long ways1 = 0;
    int max1 = 1;
    long long ways2 = 1;
    int max2 = 0;
    long long ways3 = 1;
    for (int i = 0; i < v[node].size(); ++i) {
        int nxt = v[node][i];
        if (nxt == dad) {
            continue;
        }
        dfs(node, nxt);
        max3 = max3;
        if (dp[1][nxt] - 1 == dp[2][nxt]) {
            max3 += dp[1][nxt] - 1;
            ways3 = (ways3 * (ways[1][nxt] + ways[2][nxt])) % mod;
        }
        else {
            if (dp[1][nxt] - 1 > dp[2][nxt]) {
                max3 += dp[1][nxt] - 1;
                ways3 = (ways3 * (ways[1][nxt])) % mod;
            }
            else {
                max3 += dp[2][nxt];
                ways3 = (ways3 * ways[2][nxt]) % mod;
            }
        }
        max1 = max1 + dp[2][nxt];
        ways1 = (ways1 * ways[2][nxt]) % mod;
        if (dp[1][nxt] == dp[2][nxt]) {
            max2 += dp[1][nxt];
            ways2 = (ways2 * (ways[1][nxt] + ways[2][nxt])) % mod;
        }
        else {
            if (dp[1][nxt] > dp[2][nxt]) {
                max2 += dp[1][nxt];
                ways2 = (ways2 * (ways[1][nxt])) % mod;
            }
            else {
                max2 += dp[2][nxt];
                ways2 = (ways2 * ways[2][nxt]) % mod;
            }
        }
    }
    max3++;
    dp[1][node] = max(max3, max1);
    dp[2][node] = max2;
    ways[2][node] = ways2;
    if (max3 == max1) {
        ways[1][node] = (ways1 + ways3) % mod;
    }
    if (max3 > max1) {
        ways[1][node] = ways3;
    }
    if (max1 > max3) {
        ways[1][node] = ways1;
    }
    if (node == 1) {
        if (dp[1][node] == dp[2][node]) {
            totalans = (ways[1][node] + ways[2][node]) % mod;
        }
        if (dp[1][node] > dp[2][node]) {
            totalans = ways[1][node];
        }
        if (dp[2][node] > dp[1][node]) {
            totalans = ways[2][node];
        }
    }
}
int main() {
    f >> n;
    for (int i = 1; i < n; ++i) {
        int a, b;
        f >> a >> b;
        v[a].push_back(b);
        v[b].push_back(a);
    }
    dfs(0, 1);
    g << max(dp[1][1], dp[2][1]) << " " << totalans << '\n';
    return 0;
}
```

## Problema [Museum CEOI 2017](https://oj.uz/problem/view/CEOI17_museum)

!!! note "Observație importantă"

    [Acest blog](https://codeforces.com/blog/entry/100910) va detalia o tehnică
    folosită pentru această problemă care ne ajută să optimizăm complexitatea cu
    un factor de $n$.

Există foarte multe dinamici pe arbore care au o soluție care după un număr de
observații, se reduc la un rucsac sau o idee similară. De regulă, vom vrea să
ordonăm nodurile după numărul de noduri din subarbore pentru a reduce numărul de
operații efectuat atunci când actualizăm răspunsurile din dinamica noastră.

Acest lucru se va observa și în cazul problemei noastre, unde vom avea o
dinamică de tip $dp[0/1][i][j]$, care reprezintă cel mai mic cost al unui traseu
ce începe în nodul $i$, vizitează nodul $j$ și se întoarce sau nu la nodul $i$
(1 - se întoarce, 0 - nu se întoarce).

Pentru a calcula această dinamică, vom avea pentru fiecare nod un rucsac
care trece prin variantele de a continua un anume drum, în funcție de
ordinea pe care o vom prefera. De notat că trebuie să fim atenți la
implementare, deoarece există destul de multe detalii care trebuie
avute în considerare.

```cpp
#include <iostream>
#include <vector>

using namespace std;

int n, k, x;
int dp[2][10002][10002];
int xtr[10002][10002];

vector<pair<int, int>> v[10002];

vector<int> dp2[2][10002];
vector<int> xtr2[2][10002];
vector<bool> viz2[2][10002];
int sts[10002];
void dfs(int parent, int node, int cost) {
    sts[node] = 1;
    for (int i = 0; i < (int) v[node].size(); ++i) {
        int vecin = v[node][i].first;
        if (vecin == parent)
            continue;
        dfs(node, vecin, v[node][i].second);
        sts[node] += sts[vecin];
    }

    dp2[0][node].resize(sts[node] + 2, 0);
    dp2[1][node].resize(sts[node] + 2, 0);

    xtr2[0][node].resize(sts[node] + 2, 0);
    xtr2[1][node].resize(sts[node] + 2, 0);

    viz2[0][node].resize(sts[node] + 2, 0);
    viz2[1][node].resize(sts[node] + 2, 0);

    viz2[0][node][1] = 1;
    sts[node] = 1;
    for (int i = 0; i < (int) v[node].size(); ++i) {
        int vecin = v[node][i].first;
        if (vecin == parent)
            continue;
        int edcost = v[node][i].second;
        for (int j = min(k, sts[node]); j >= 1; --j)
            for (int sz = 1; sz <= min(sts[vecin], k - j); ++sz) {
                if (!viz2[1][node][j + sz] || dp2[0][node][j] + dp[1][vecin][sz] + edcost < dp2[1][node][j + sz]) {
                    dp2[1][node][j + sz] = dp2[0][node][j] + dp[1][vecin][sz] + edcost;
                    viz2[1][node][j + sz] = 1;
                    xtr2[1][node][j + sz] = xtr[vecin][sz];
                } 
                else {
                    if (dp2[0][node][j] + dp[1][vecin][sz] + edcost == dp2[1][node][j + sz]) {
                        xtr2[1][node][j + sz] = min(xtr2[1][node][j + sz], xtr[vecin][sz]);
                    }
                }
                for (int trb = 0; trb <= 1; ++trb) {
                    if (!viz2[trb][node][j + sz] ||
                        dp2[trb][node][j] + dp[0][vecin][sz] + 2 * edcost < dp2[trb][node][j + sz]) {
                        dp2[trb][node][j + sz] = dp2[trb][node][j] + dp[0][vecin][sz] + 2 * edcost;
                        viz2[trb][node][j + sz] = 1;
                        xtr2[trb][node][j + sz] = xtr2[trb][node][j];
                    } 
                    else if (dp2[trb][node][j] + dp[0][vecin][sz] + 2 * edcost == dp2[trb][node][j + sz]) {
                        xtr2[trb][node][j + sz] = min(xtr2[trb][node][j + sz], xtr2[trb][node][j]);
                    }
                }
            }
        sts[node] += sts[vecin];
    }
    for (int i = 2; i <= min(k, sts[node]); ++i) {
        dp[1][node][i] = dp2[1][node][i];
        xtr[node][i] = xtr2[1][node][i];
        dp[0][node][i] = dp2[0][node][i];
    }
    for (int i = 1; i <= min(k, sts[node]); ++i)
        xtr[node][i] += cost;
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    cin >> n >> k >> x;
    for (int i = 1; i < n; ++i) {
        int a, b, c;
        cin >> a >> b >> c;
        v[a].push_back({b, c});
        v[b].push_back({a, c});
    }
    dfs(0, x, 0);
    cout << dp[1][x][k] << '\n';
    return 0;
}
```

## Problema [TreeGCD ONI 2019](https://kilonova.ro/problems/11)

Pentru a rezolva această problemă, ne vom folosi de faptul că valoarea
lui $M$ este foarte mică, ceea ce ne duce cu gândul la o dinamică care
să implice fiecare valoare posibilă a CMMDC-ului în parte.

Mai întâi, vom precalcula pentru fiecare număr $i$ de la 1 la $m$ toate
numerele cu proprietatea că cmmdc-ul dintre ele și $i$ este diferit
de 1 și care nu au niciun factor prim în descompunerea lor mai mult
de o dată.

Acest lucru ne va ajuta să prevenim numărarea de mai multe ori a acelorlași
răspunsuri, lucru specific problemelor ce folosesc principiul includerii
și al excluderii.

Apoi, vom rula un DFS din rădăcină în care vom avea o dinamică de
tipul $dp[i][j]$ ce reprezintă numărul de moduri de a avea un arbore
cu rădăcina în nodul $i$, iar în acel nod avem valoarea $j$.

Pentru a calcula această dinamică, putem avea mai întâi niște
tranziții încete care calculează pentru fiecare variantă de a avea
o valoare pe un nod, numărul de moduri de a avea o valoare pe un fiu,
dar această abordare ar avea complexitatea $O(n \cdot m^2)$, ceea
ce este prea încet.

O observație importantă pe care o facem este aceea că ne va interesa
să știm răspunsurile pentru divizorii fiecărei valori pe care o putem
avea în acel nod, iar atunci când vom înmulți răspunsurile la un anume
pas, vom ține cont de numărul de factori primi și de suma parțială
a răspunsurilor pentru numerele care sunt multiplu ale acelui divizor.

Astfel, vom ajunge să avem o complexitate echivalentă cu aplicarea
unui [PINEX](../mediu/pinex.md), deci $O(n \cdot m \cdot DMAX)$,
unde $DMAX$ este numărul maxim de divizori pe care îi are un număr conform
precalculării anterioare, iar acest număr $DMAX$ este egal cu 31 deoarece
orice număr de la 1 la $m$ are cel mult 5 factori primi distincți.

Pentru mai multe detalii de implementare, recomandăm consultarea sursei de mai
jos.

```cpp
#include <iostream>
#include <fstream>
#include <vector>
#include <algorithm>

using namespace std;

const int N = 102;
const int M = 10004;
const int MOD = 1000000007;

int n, m;
vector<int> adj[N], divs[M];
long long dp[N][M], eradp[N][M], pfcnt[M];

void dfs(int node, int parent) {
    for (auto neighbor : adj[node]) {
        if (neighbor != parent) {
            dfs(neighbor, node);
        }
    }

    for (int amt = 2; amt <= m; ++amt) {
        dp[node][amt] = 1;
        for (auto neighbor : adj[node]) {
            if (neighbor == parent) {
                continue;
            }
            long long count = 0;
            for (auto divisor : divs[amt]) {
                if (divisor == 1) {
                    continue;
                }
                count = (count + ((pfcnt[divisor] & 1 ? -1 : 1) * eradp[neighbor][divisor]) + MOD) % MOD;
            }
            dp[node][amt] = (dp[node][amt] * count) % MOD;
        }
    }

    for (int i = 1; i <= m; ++i) {
        for (int j = i; j <= m; j += i) {
            eradp[node][i] = (eradp[node][i] + dp[node][j]) % MOD;
        }
    }
}

int main() {
    
    ifstream cin("treegcd.in");
    ofstream cout("treegcd.out");
    
    cin >> n >> m;

    for (int i = 0; i < n - 1; ++i) {
        int x, y;
        cin >> x >> y;
        --x; 
        --y; 
        adj[x].push_back(y);
        adj[y].push_back(x);
    }

    for (int i = 2; i < M; ++i) {
        divs[i].push_back(1);
        int num = i;
        for (int j = 2; j * j <= num; ++j) {
            if (num % j == 0) {
                int previous_size = divs[i].size();
                for (int k = 0; k < previous_size; ++k) {
                    divs[i].push_back(divs[i][k] * j);
                }
                pfcnt[i]++;
            }
            while (num % j == 0) {
                num /= j;
            }
        }
        if (num != 1) {
            int previous_size = divs[i].size();
            for (int k = 0; k < previous_size; ++k) {
                divs[i].push_back(divs[i][k] * num);
            }
        }
    }

    dfs(0, 0);

    long long result = 0;
    for (int i = 1; i <= m; ++i) {
        result = (result + dp[0][i]) % MOD;
    }

    cout << result << '\n';
    return 0;
}
```

## Problema [Compressed Tree - Codeforces](https://codeforces.com/problemset/problem/1901/E)

Pentru această problemă, vom avea o dinamică pe două dimensiuni care va
ține cont de următoarele cazuri:

- $dp[0][node]$: cea mai mare sumă a unui subarbore cu rădăcina în $node$,
presupunând că vom păstra legătura dintre acest nod și părintele său.
- $dp[1][node]$: cea mai mare sumă a unui subarbore cu rădăcina în $node$,
fără a păstra legătura dintre $node$ și părintele său.

În funcție de aceste cazuri, vom vrea să păstrăm un set de noduri,
împreună cu sumele lor care să fie cât mai mari, ceea ce duce cu
gândul la o abordare în care sortăm sumele în ordine descrescătoare,
luând mereu termenii pozitivi, iar dacă este necesar, să completăm cu
un număr de termeni până ajungem la doi, respectiv trei copii luați.

Ulterior, în funcție de numărul de fii, se pot deduce ușor câteva cazuri care
puse laolaltă, ne duc la soluția cerută, așa cum se poate observa în codul de
mai jos. În mod alternativ, puteți consulta și [soluția
oficială](https://codeforces.com/blog/entry/122645) propusă de autorii
competiției.

```cpp
#include <bits/stdc++.h>
using namespace std;

long long ans, v[500002], dp[2][500002];
vector<vector<int>> tree;

int n;

void dfs(int parent, int node) {

    vector<long long> sums;
    for (int i = 0; i < (int)tree[node].size(); i++) {
        int nxt = tree[node][i];
        if (nxt == parent)
            continue;
        dfs(node, nxt);
        sums.push_back(dp[0][nxt]);
    }

    sort(sums.begin(), sums.end());
    reverse(sums.begin(), sums.end());

    // keep the parent

    dp[0][node] = v[node];
    if (sums.size() >= 2) {
        long long sm = 0;
        for (int i = 0; i < (int)sums.size(); i++) {
            if (sums[i] < 0 && i >= 2)
                break;
            sm += sums[i];
        }
        dp[0][node] = max(dp[0][node], v[node] + sm);
    }

    if (sums.size() >= 1)
        dp[0][node] = max(dp[0][node], sums[0]);

    // not keep the parent

    if (sums.size() >= 1)
        dp[1][node] = v[node] + sums[0];

    if (sums.size() > 2) {
        long long sm = 0;
        for (int i = 0; i < (int)sums.size(); i++) {
            if (sums[i] < 0 && i >= 3)
                break;
            sm += sums[i];
        }
        dp[1][node] = max(dp[1][node], v[node] + sm);
    }

    if (sums.size() >= 2)
        dp[1][node] = max(dp[1][node], sums[0] + sums[1]);
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int t;
    cin >> t;

    for (; t; t--) {
        cin >> n;
        ans = 0;
        for (int i = 1; i <= n; i++) {
            cin >> v[i];
            ans = max(ans, v[i]);
        }

        tree.resize(n + 1);
        for (int i = 1; i < n; i++) {
            int a, b;
            cin >> a >> b;
            tree[a].push_back(b);
            tree[b].push_back(a);
        }
        dfs(0, 1);

        for (int i = 1; i <= n; i++) {
            ans = max(ans, dp[1][i]);
            dp[0][i] = dp[1][i] = 0;
        }
        cout << ans << '\n';
        tree.clear();
    }

    return 0;
}
```

## Concluzii

Dinamicile pe arbore apar foarte des în competițiile de informatică românești și
nu numai, iar experiența căpătată cu aceste tipuri de probleme se dovedește a fi
foarte importantă pentru obținerea unor rezultate foarte bune la olimpiadă, în
special la ONI și baraj în clasele XI-XII.

Recomandăm lucratul a cât mai multe probleme, atât din această listă cât și din
celelalte articole menționate anterior, în special cel despre rerooting, care
are concepte mai simple, dar care se leagă de cunoștințele prezentate aici.

## Probleme suplimentare

- [AtCoder Independent Set](https://atcoder.jp/contests/dp/tasks/dp_p)
- [OJI 2019 Tairos](https://kilonova.ro/problems/22)
- [OJI 2008 Iepuri](https://kilonova.ro/problems/45)
- [Nastia Plays with a Tree Codeforces](https://codeforces.com/contest/1521/problem/D)
- [ONI 2015 arbvalmax](https://kilonova.ro/problems/203/)
- [USACO Gold Barn Painting](https://usaco.org/index.php?page=viewproblem2&cpid=766)
- [USACO Gold Delegation](http://www.usaco.org/index.php?page=viewproblem2&cpid=1019)
- [RMI 2023 AAtree](https://kilonova.ro/problems/1834)
- [Moisil 2024 Arborele frumos](https://kilonova.ro/problems/2568/)
- [Permutree Codeforces](https://codeforces.com/problemset/problem/1856/E2)
- [JOI 2020 Power](https://oj.uz/problem/view/JOI20_power)
- [ONI 2018 tricolor](https://kilonova.ro/problems/151)
- [ONI 2021 Baraj Seniori Arbsumpow](https://kilonova.ro/problems/67/)
- [ONI 2019 Baraj Seniori Anarhie](https://kilonova.ro/problems/409/)
- [CSES Creating Offices](https://cses.fi/problemset/task/1752)
- [Miss Punyverse Codeforces](https://codeforces.com/contest/1280/problem/D)
- [IOI 2007 Training](https://oj.uz/problem/view/IOI07_training)
- [Probleme cu DP pe arbore de pe Kilonova](https://kilonova.ro/problems?tags=275%2C284)
- [Probleme cu DP pe arbore de pe Codeforces](https://codeforces.com/problemset?tags=dp,trees)

## Resurse suplimentare

- [DP on Trees - USACO Guide](https://usaco.guide/gold/dp-trees)
- [DP on Trees tutorial - Codeforces](https://codeforces.com/blog/entry/20935)
