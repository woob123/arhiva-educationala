---
tags:
    - OJI
    - clasa VIII
---

# Soluția problemei Hibrid (OJI 2023, clasa a VIII-a)

!!! example "Cunoștințe necesare"
    - [Maxime și minime](../../../../usor/maxime-minime.md)
    - [Sume parțiale](../../../../usor/partial-sums.md)
    - [Șmenul lui Mars](../../../../usor/partial-sums.md#smenul-lui-mars)

**Autor soluție**: Andrei Onuț

!!! note "Link problemă"
    Această problemă poate fi accesată [aici](https://kilonova.ro/problems/515/).

Observație inițială: Pentru că cele $P$ porțiuni taxabile sunt disjuncte două câte două, iar nicio bornă dintre cele $N$ nu se poate afla la capetele sau în interiorul segmentelor ce descriu porțiunile taxabile (mai formal, nu există nicio pereche de indici $(i, j)$: $1 \leq i \leq P$ și $1 \leq j \leq N$ pentru care $s_i \leq x_j \leq d_i$), înseamnă că $d_i$ ar putea fi ignorat pentru fiecare $i: 1 \leq i \leq P$.  

Astfel, dacă ar fi nevoie să verificăm, de exemplu, de câte ori s-a trecut peste o porțiune taxabilă, ar trebui doar verificat de câte ori punctul de coordonată $s_i$ a fost vizitat în timp ce se efectua deplasarea între două borne consecutive de pe traseu; a număra de câte ori s-a trecut peste o porțiune taxabilă $i$ $(1 \leq i \leq P)$ este echivalent cu a număra numărul de indici $j$ $(1 \leq j < N)$ pentru care: $\min(x_j, x_{j+1}) < s_i < \max(x_j, x_{j+1}).$ $d_i$, observăm, nu mai influențează această abordare.  

Notații: $\min(a, b) = a \ \text{dacă} \ a < b, b \ \text{altfel.}$. $\max(a, b) = a \ \text{dacă} \ a > b, b \ \text{altfel.}$

Subtask 1: Întrucât se cunoaște faptul că pentru efectuarea traseului nu se va trece peste niciuna dintre cele $P$ porțiuni taxabile (unde este impusă folosirea motorului termic), în fișierul de ieșire se va afișa $-1$ (în cazul în care $C = 1$) sau 0 (în cazul în care $C = 2$).  

Subtask 2: Întrucât $0 \leq x_i \leq 70$, pentru fiecare $i: 1 \leq i \leq N$, înseamnă că lungimea fiecărui segment de dreaptă între două borne consecutive de pe traseu va fi de cel mult 70 ($|x_{i+1} - x_i| \leq 70$, pentru fiecare $i: 1 \leq i < N$).  

Să considerăm introducerea următorului tablou unidimensional $\text{fr}[0, \ldots, 70]$ ce conține exact 71 de elemente, cu următoarea semnificație: $\text{fr}[x] \, (0 \leq x \leq 70) = \text{de câte ori s-a trecut, în timpul efectuării traseului, prin coordonata } x \text{ (de pe axa numerelor)}$. Observăm că $\text{fr}[x] = 0$ în cazul în care nu s-a trecut niciodată prin dreptul coordonatei $x$.  

Pentru a calcula într-o complexitate de $O(71 \times N)$ care vor fi valorile elementelor din vectorul $\text{fr}[0, \ldots, 70]$, putem utiliza următoarea metodă, de tip brute-force:  

```cpp
for(int i = 1; i < N; ++i) { /// iteram prin lista coordonatelor de borne
    int p1 = min(x[i], x[i + 1]); /// capat "stanga" pe axa numerelor intregi
    int p2 = max(x[i], x[i + 1]); /// capat "dreapta" pe axa numerelor intregi
    for(int j = p1; j <= p2; ++j) /// iteram prin coordonatele de pe axa numerelor
        ++fr[j]; /// incrementam numarul de "vizite" prin dreptul coordonatei j
}
```
Apoi, pentru cerința $C = 1$, se determină care este indicele minim $i$ $(1 \leq i \leq P)$ pentru care valoarea $\text{fr}[s_i]$ este maximă. Pentru cerința $C = 2$, se calculează suma: $\sum_{i=1}^P c_i \cdot \text{fr}[s_i].$

Complexitate temporală totală: $O(P + N \cdot 71)$.  

**De remarcat:** Pentru acest subtask se știe, în plus, că $0 \leq s_i < d_i \leq 70$, pentru fiecare $i: 1 \leq i \leq P$. De asemenea, de vreme ce porțiunile taxabile sunt disjuncte două câte două, înseamnă că numărul $P$ nu poate fi prea mare. Mai exact, acesta este cel mult egal cu 35, și se poate obține, de exemplu, pentru: $s_i = (i - 1) \cdot 2$ și $d_i = s_i + 1$, pentru fiecare $i: 1 \leq i \leq P$, rezultând în intervalele: $[0, 1], [2, 3], \ldots, [68, 69]$.  

Astfel, înseamnă că și o soluție cu o complexitate de $O(P \cdot N) = O(35 \cdot N)$ va trece toate testele aferente acestui subtask.  

**Subtask 3:** Ca și în cazul precedent, avem în continuare că $|x_{i+1} - x_i| \leq 70$, pentru fiecare $i: 1 \leq i < N$, ceea ce înseamnă că am putea simula traseul descris, iterând prin fiecare coordonată prin care mașina hibridă se deplasează.  

De asemenea, valoarea absolută a coordonatelor bornelor este cel mult egală cu 300000; prin urmare, pentru fiecare deplasare între două borne consecutive am putea aplica același algoritm ca și mai sus. Trebuie avut grijă la faptul că indicii ale căror valori dorim să le modificăm într-un tablou unidimensional de frecvență/numărare ar putea deveni negativi.  

Așadar, vom aplica indicilor o translație spre dreapta cu 300000 poziții: mai exact, poziția $-300000$ va fi translatată în poziția 0, poziția $-299999$ va fi translatată în poziția 1, ..., poziția 0 va fi translatată în poziția 300000, ..., poziția 300000 va fi translatată în poziția 600000.  

```cpp
for(int i = 1; i < N; ++i) { /// iteram prin lista coordonatelor de borne
    int p1 = min(x[i], x[i + 1]); /// capat "stanga" pe axa numerelor intregi
    int p2 = max(x[i], x[i + 1]); /// capat "dreapta" pe axa numerelor intregi
    for(int j = p1; j <= p2; ++j) /// iteram prin coordonatele de pe axa numerelor
        ++fr[j + 300000]; /// incrementam numarul de "vizite" prin dreptul coordonatei j
}
```

$\text{fr}[0, \ldots, 600000]$ este tabloul unidimensional cu ajutorul căruia numărăm de câte ori s-a trecut prin dreptul fiecărei coordonate. Astfel, dacă dorim să știm, de exemplu, de câte ori am trecut prin coordonata $x$ $(-300000 \leq x \leq 300000)$,  
accesăm valoarea $\text{fr}[x + 300000]$. Complexitatea temporală totală rămâne: $O(P + N \cdot 71)$.  

**Subtask 4:**  Vom simula, în continuare, traseul mașinii pe șosea, însă, pentru a ne încadra în limita de timp dată, nu vom mai parcurge coordonate de pe axa numerelor, ci vom parcurge direct lista de intervale ce descriu cele $P$ porțiuni taxabile.  

Mai exact spus, vom parcurge fiecare segment $(i, i + 1)$ $(1 \leq i < N)$ al traseului. Vom itera, apoi, prin lista celor $P$ porțiuni, un indice $j$ $(1 \leq j \leq P)$ și dacă se întâmplă să avem: $\min(x_i, x_{i+1}) < s_j < \max(x_i, x_{i+1})$, în lumina observației inițiale, înseamnă că porțiunea taxabilă cu indicele $j$ a fost traversată în timp ce se făcea deplasarea de la coordonata bornei $i$ la cea a bornei $(i + 1)$.  

Astfel, tot într-un tablou unidimensional de frecvență (ce conține $P$ elemente), se va contoriza de câte ori s-a trecut peste fiecare porțiune taxabilă.  

```cpp
for(int i = 1; i < N; ++i) { /// iteram prin lista coordonatelor de borne
    int p1 = min(x[i], x[i + 1]); /// capat "stanga" pe axa numerelor intregi
    int p2 = max(x[i], x[i + 1]); /// capat "dreapta" pe axa numerelor intregi
    for(int j = 1; j <= P; ++j) /// iteram prin lista de coordonate taxabile
        if (p1 < st[j] && st[j] < p2) {
            ++fr2[j];
        }
}
```

În mod similar cu soluțiile descrise anterior, pentru cerința $C = 1$, se determină care este indicele minim $i$ $(1 \leq i \leq P)$ pentru care valoarea $\text{fr2}[i]$ este maximă. Pentru cerința $C = 2$, se calculează suma: $\sum_{i=1}^P c_i \cdot \text{fr2}[i]$. Complexitatea totală este: $O(P \cdot N)$.  

**Subtask 5:** Remarcăm că cele $P$ porțiuni de șosea sunt incluse complet în porțiunea reprezentată de segmentul $[-300000, 300000]$ de pe șosea. Astfel, o observație este că, dacă mașina hibrid trebuie să parcurgă un segment  $[l, r]$ $(l \leq r)$ de pe șosea, se poate modifica acest interval astfel încât să reprezinte intersecția: $[l, r] \cap [-300000, 300000];$  în cazul în care intersecția este vidă, se poate ignora segmentul $[l, r]$ de pe traseu, din moment ce parcurgerea acestuia nu va influența niciuna dintre cele $P$ porțiuni taxabile.  

De această dată, este nevoie ca marcarea unui interval $[l, r]$ să fie efectuată în timp constant, $O(1)$. Astfel, poate fi folosită, de exemplu, metoda Vectorului de Diferențe (Difference Array în Engleză); în România, această tehnică mai este cunoscută sub numele de *Șmenul lui Mars*.  

Întrucât $l$ sau $r$ pot avea valori negative, se va alege din nou o translație spre dreapta cu 300000 de poziții. În momentul actualizării intervalului, valoarea de pe poziția $(l + 300000)$ în tabloul unidimensional folosit pentru Șmenul lui Mars va crește cu o unitate, iar cea de pe poziția $(r + 300000 + 1)$ va scădea cu o unitate.  

Complexitatea totală a acestei soluții este $O(P + N + \text{maxd})$, unde $\text{maxd}$ reprezintă diferența maximă dintre două coordonate vizitate în timpul efectuării traseului (considerând intersecțiile cu segmentul $[-300000, 300000]$): $\text{maxd} \leq 600000$.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <bits/stdc++.h>
using namespace std;

const int NMAX = 300001;
int t[600005], m[600005], s2[600005], s1[600005], v[200005];
long long cost[200005], maxx[200005];
int main() {
    
    ifstream cin("hibrid.in");
    ofstream cout("hibrid.out");
    
    int cer, k, n;
    cin >> cer >> k >> n;
    for (int i = 1; i <= k; i++) {
        int a, b;
        cin >> a >> b >> cost[i];
        a += NMAX;
        b += NMAX;
        m[a] += i;
        m[b + 1] -= i;
    }
    s1[0] = m[0];
    for (int i = 1; i <= NMAX * 2; i++)
        s1[i] = s1[i - 1] + m[i];
    for (int i = 1; i <= n; i++) {
        cin >> v[i];
        if (v[i] < -NMAX)
            v[i] = -NMAX;
        if (v[i] > NMAX)
            v[i] = NMAX;
        v[i] += NMAX;
    }
    for (int i = 1; i < n; i++) {
        int a = min(v[i], v[i + 1]);
        int b = max(v[i], v[i + 1]) + 1;
        t[a]++;
        t[b]--;
    }
    s2[0] = t[0];
    for (int i = 1; i <= NMAX * 2; i++)
        s2[i] = s2[i - 1] + t[i];
    int r = 0, sol = -1;
    for (int i = 0; i <= NMAX * 2; i++) {
        if (s1[i] > 0 && s2[i] > 0) {
            if (s2[i] > r) {
                r = s2[i];
                sol = s1[i];
            } else if (s2[i] == r)
                sol = min(sol, s1[i]);
        }
    }
    if (cer == 1)
        cout << sol;
    else {
        for (int i = 0; i <= NMAX * 2; i++) {
            if (s1[i] > 0 && s2[i] > maxx[s1[i]])
                maxx[s1[i]] = s2[i];
        }
        long long r2 = 0;
        for (int i = 1; i <= k; i++)
            r2 += maxx[i] * cost[i];
        cout << r2;
    }
    return 0;
}
```