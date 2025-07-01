---
id: OJI-2023-IX-fibosnek
title: Soluția problemei fibosnek (OJI 2023, clasa a IX-a)
problem_id: 506
authors: [nodea]
prerequisites:
    - basic-math
    - matrices
    - sequences
tags:
    - OJI
    - clasa IX
---

## Observații

- Având în vedere restricțiile problemei, vom memora într-un tablou
unidimensional $fib$ doar primii $nrF = 46$ termeni ai șirului
Fibonacci, ultimul termen fiind $fib[46] = 1836311903$.

- Nu este necesară reținerea valorilor matricei inițiale, matricea de
int-uri depășind memoria maximă alocată (6MB). Se vor reține indicii
din șirul Fibonacci corespunzători valorilor citite. În cazul valorilor
care nu aparțin șirului Fibonacci, se poate reține indicele celui mai
apropiat număr din șir, cu semn negativ. Așadar, $8$ va fi reținut ca
$5$, $13$ ca $6$, $11$ ca $−6$, iar $4$ ca $−3$. Astfel, se poate
reduce semnificativ memoria, deoarece poate fi folosită o matrice de
$char$, întrucât valorile din noua matrice sunt între $−46$ și $46$.

- Matricea inițială se poate liniariza, adică se poate memora într-un
tablou unidimensional prin parcurgerea $snek$. Elementul de pe celula
$(i, j)$ din matrice va apărea în tablou pe poziția $(j − 1) \cdot n +
i$, pentru oricare $1 \leq i \leq n$, $1 \leq j \leq m$.

Astfel, în urma acestor transformări asupra problemei, se reduce la
determinarea lungimii maxime a cel mult trei secvențe consecutive,
compacte, ce alternează $fibosnek$, $non-fibosnek$, $fibosnek$.

## Cazul $c = 1$ și $n, m \leq 1000$

Se vor precalcula primii $46$ de termeni ai șirului Fibonacci în
tabloul $fib$. Pentru fiecare valoare citită din matricea inițială, se
va verifica dacă aceasta se află în vectorul $fib$ utilizând căutare
binară și se vor număra câte numere Fibonacci au fost întâlnite.

Complexitate temporală: $O(n \cdot m \cdot \log nr_F + nr_F)$.
Complexitate spațială: $O(nr_F)$.

## Cazul $c = 2$ și $n, m \leq 100$

Se parcurge matricea pe coloane, conform parcurgerii $snek$, și pentru
fiecare element al matricei se determină dacă este sau nu număr
Fibonacci parcurgând secvențial tabloul $fib$.

Pentru fiecare triplet de secvențe alternative ($fibosnek$,
$non-fibosnek$, $fibosnek$), se va calcula răspunsul astfel:

- $S_1$ — suma primei secvențe din triplet $fibosnek$ ce are $n_1$ termeni;
- $S_2$ — suma secvenței din mijloc $non-fibosnek$ ce are $n_2$ termeni;
- $S_3$ — suma ultimei secvenței $fibosnek$ ce are $n_3$ termeni.

Se pot întâlni următoarele cazuri particulare:

1. $n_1 = 0$ – secvența $fibosnek$ curentă este prima din parcurgere.
2. $n_1 = 0$, $n_3 = 0$ – nu există nicio secvență $fibosnek$ în parcurgere.
3. $n_3 = 0$ – parcurgerea se termină într-o secvență $non-fibosnek$.

Se reține suma $S_1 + S_2 + S_3$ a primului triplet întâlnit de lungime
maximă $n_1^2 + n_2^2 + n_3^2$.

Complexitate temporală: $O(n \cdot m \cdot nr_F + nr_F)$.
Complexitate spațială: $O(n \cdot m)$.

## Cazul $c = 2$ și $n, m \leq 1000$

Pentru acest caz este necesară îmbunătățirea algoritmului anterior
prezentat. Putem verifica dacă un număr este sau nu Fibonacci căutând
binar în loc de secvențial acel număr în tabloul $fib$, astfel,
reducând complexitatea pentru verificarea fiecărui număr de la $O(nr_F)$
la $O(\log nr_F)$.

Complexitate temporală: $O(n \cdot m \cdot \log nr_F)$.

## Soluția finală

Pentru a obține punctajul maxim, este necesară implementarea
artificiului de memorare a indicilor și nu a termenilor Fibonacci
propriu-ziși pentru a micșora memoria utilizată.


## Rezolvare

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<long long> fib;

int closestfibleft (long long x) {
    int L = 0;
    int R = fib.size() - 1;
    int ans = 0;
    while (L <= R) {
        int mid = (L + R) / 2;
        if (fib[mid] <= x) {
            ans = mid;
            L = mid + 1;
        }
        else {
            R = mid - 1;
        }
    }
    
    return ans;
}

int closestfibright (long long x) {
    int L = 0;
    int R = fib.size() - 1;
    int ans = 0;
    while (L <= R) {
        int mid = (L + R) / 2;
        if (fib[mid] < x) {
            L = mid + 1;
        }
        else {
            ans = mid;
            R = mid - 1;
        }
    }
    
    return ans;
}
int main() {
    
    ifstream cin("fibosnek.in");
    ofstream cout("fibosnek.out");

    int c, n, m;
    cin >> c >> n >> m;
    
    fib.push_back(0);
    fib.push_back(1); 
    fib.push_back(2); 
    
    while(1) {
        int sz = fib.size();
        long long x = fib[sz-1] + fib[sz - 2];
        fib.push_back(x); 
        if (x >= (1LL<<36)) {
            break;
        }
    }
    
    char grid[n+1][m+1];
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            long long val;
            cin >> val;
            
            int posL = closestfibleft(val);
            int posR = closestfibright(val);
            if (fib[posL] == val) {
                grid[i][j] = posL;
            }
            else {
                if (abs(val - fib[posR]) < abs(val - fib[posL])) {
                    posL = posR;
                }
                grid[i][j] = -posL;
            }
        }
    }
    
    if (c == 1) {
        int ans = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (grid[i][j] > 0) {
                    ans++;
                }
            }
        }
        
        cout << ans << '\n';
    }
    else {
        deque <pair<int, long long> > seqs;
        int streak = 0;
        long long sum = 0;
        
        int maxlen = 0;
        long long sm = 0;
        for (int j = 1; j <= m; j++) {
            for (int i = 1; i <= n; i++) {
                if (grid[i][j] > 0) {
                    if (streak >= 0) {
                        streak++;
                        sum += fib[grid[i][j]];
                    }
                    else {
                        seqs.push_back ({streak, sum});
                        streak = 1;
                        sum = fib[grid[i][j]];
                    }
                }
                else {
                    grid[i][j] *= -1;
                    if (streak < 0) {
                        streak--;
                        sum += fib[grid[i][j]];
                    }
                    else {
                        seqs.push_back ({streak, sum});
                        streak = -1;
                        sum = fib[grid[i][j]];
                    }
                }
                
                if (seqs.size() >= 4) {
                    int N = (int) seqs.size();
        
                    for (int i = 0; i < N; i++) {
                        if (seqs[i].first > maxlen) {
                            maxlen = seqs[i].first;
                            sm = seqs[i].second;
                        }
                    }
                    
                    for (int i = 0; i < N; i++) {
                        if (seqs[i].first < 0) {
                            if (abs(seqs[i].first) + (i > 0) * seqs[i-1].first + (i + 1 < N) * seqs[i+1].first > maxlen) {
                                maxlen = abs(seqs[i].first) + (i > 0) * seqs[i-1].first + (i + 1 < N) * seqs[i+1].first;
                                sm = seqs[i].second + (i > 0) * seqs[i-1].second + (i + 1 < N) * seqs[i+1].second;
                            }
                        }
                    }
                    
                    seqs.pop_front();
                }
            }
        }
        seqs.push_back({streak, sum});
        
        
        int N = (int) seqs.size();
        
        for (int i = 0; i < N; i++) {
            if (seqs[i].first > maxlen) {
                maxlen = seqs[i].first;
                sm = seqs[i].second;
            }
        }
        
        for (int i = 0; i < N; i++) {
            if (seqs[i].first < 0) {
                if (abs(seqs[i].first) + (i > 0) * seqs[i-1].first + (i + 1 < N) * seqs[i+1].first > maxlen) {
                    maxlen = abs(seqs[i].first) + (i > 0) * seqs[i-1].first + (i + 1 < N) * seqs[i+1].first;
                    sm = seqs[i].second + (i > 0) * seqs[i-1].second + (i + 1 < N) * seqs[i+1].second;
                }
            }
        }
        
        cout << sm << '\n';
    }
    return 0;
}
```