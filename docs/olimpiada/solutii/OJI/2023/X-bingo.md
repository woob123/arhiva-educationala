---
id: OJI-2023-X-bingo
title: Soluția problemei bingo (OJI 2023, clasa a X-a)
problem_id: 503
authors: [spopa]
prerequisites:
    - simulating-solution
    - ad-hoc
    - stl
tags:
    - OJI
    - clasa X
---

Problema ne cere să aflăm numărul minim de interschimbări de elemente
adiacente ce trebuie efectuate pentru a obține o subsecvență egală cu
bingo într-un șir dat.

## Subtask 1

În acest subtask, fiecare șir primit are $5$ caractere, dar cum se
garantează faptul că fiecare element din mulțimea $\{ b, i, n, g, o \}$ apare
cel puțin o dată, rezultă că toate șirurile date sunt, de fapt,
anagrame ale șirului țintă bingo, deci trebuie doar să rearanjăm
literele date. Considerând șirul bingo permutarea identică de lungime
5, iar șirul S permutarea asociată lui $(b \rightarrow 1, i \rightarrow 2, \dots)$,
rezultatul este numărul de inversiuni din această permutare.

Exemplu: $biong \rightarrow (1, 2, 3, 4, 5) \rightarrow 2$ inversiuni $\rightarrow 2$
interschimbări necesare.

## Subtask 2

Șirurile conțin tot o singură anagramă a șirului $bingo$, dar de data
aceasta pozițiile caracterelor ce o formează nu mai sunt consecutive.
Am rezolvat mai sus cazul în care pozițiile sunt consecutive, deci ar
fi folositor să reducem și acest caz la problema anterioară. Pentru
orice șir, există cel puțin o soluție optimă în care, mai întâi,
caracterele ce duc la soluția optimă sunt aduse pe poziții consecutive
(fără a se schimba ordinea relativă a lor), după care se rezolvă
anagrama obținută ca la subtask-ul $1$. 

De asemenea, se poate demonstra
că este optim să aducem caracterele lângă cel din mijloc, iar pe acesta
să nu îl mutăm. Considerând că pozițiile celor $5$ caractere formează un
vector (sortat strict crescător), interschimbările dintre elemente pot
fi simulate cu adăugarea sau scăderea unui $1$ la un element din șir
atâta timp cât NU se interschimbă între ele două caractere dintre cele
$5$ selectate. Ne-am dori ca vectorul să fie format din numere
consecutive, însă este mai ușor să vedem cum facem vectorul să fie
format doar din elemente egale.

Așadar, adăugăm un $2$ la primul element,
un $1$ la al doilea, scădem un $1$ din penultimul și scădem un $2$ din
ultimul. Astfel, ținem cont de faptul că nu trebuie să aducem toate
elementele pe poziția din mijloc (ci fiecare pe pozițiile adiacente
corespunzătoare), iar șirul rămâne sortat crescător.

Acum, numărul
minim de interschimbări necesare pentru a aduce cele $5$ caractere pe
poziții consecutive este egal cu numărul minim de incrementări/
decrementări necesare pentru a face toate elementele egale. Știm că
este optim să le facem egale cu valoarea mediană, iar șirul nostru
fiind sortat, aceasta este cea din mijloc.

Exemplu: $hhnhbingog \rightarrow \{3, 5, 6, 8, 9 \} \rightarrow \{5, 6, 6, 7, 7 \} \rightarrow 3$
incrementări/decrementări necesare pentru a aduce toate elementele la
aceeași valoare $(6) \rightarrow 3$ interschimbări necesare pentru a
ajunge la $hhhnbiogh \rightarrow$ continuăm cu rezolvarea anagramei ca
la subtask-ul 1.

## Subtask 3

Întrucât singurele caractere ce ne interesează, de fapt, sunt cele din
mulțimea $\{b, i, n, g, o\}$, acest subtask ne permite să verificăm pentru
fiecare variantă de a alege caracterele cu care să formăm subsecvența
bingo, câte interschimbări sunt necesare. Pentru un $5$-tuplu (format din
pozițiile celor $5$ caractere) fixat, aplicăm raționamentul de la
subtask-ul $2$. Complexitatea unei astfel de abordări este de ordinul
$O(N^5)$, unde $N$ este lungimea șirului de caractere.

Exemplu: $xboting \rightarrow \{2, 3, 4, 6, 7\} \rightarrow subtask
\ 2 \rightarrow \{2, 3, 5, 6, 7\} \rightarrow subtask \ 2$.

## Soluția completă

Pentru fiecare permutare (dintre cele $120 = 5!$) a lui $bingo$, încercăm
să aplicăm raționamentul de la subtask-ul $3$ într-un mod mai eficient.

Pentru o permutare fixată, ne interesează doar $5$-tuplurile care
păstrează ordinea relativă a caracterelor dată de permutare. Dintre
acestea, majoritatea nu pot duce la soluția optimă, fiind depășite de
$5$-tuplurile ale căror termeni au pozițiile mai apropiate între ele.

Pentru o permutare fixată, este de ajuns să considerăm doar aparițiile
caracterului din mijloc (poziția $2$, indexând de la $0$) și să calculăm
pozițiile primului caracter valid din stânga (cel cu indicele $1$ din
permutare), respectiv primul valid din dreapta (indicele $3$). Folosim
același raționament pentru caracterele cu indicii $0$ și $4$.

Observație: Considerăm aparițiile caracterului din mijloc, deoarece
ulterior lângă acela trebuie aduse și celelalte, deci de la început și
celelalte trebuie alese cât mai aproape de acesta.

Exemplu: Dacă anagrama fixată este $nibog$, pentru fiecare apariție a lui
b din șir, căutăm primul $i$ din stânga lui $b$, respectiv primul $o$ din
dreapta. Analog, căutăm primul $g$ din dreapta lui $o$ și primul $n$ din
stânga lui $i$. Pentru indicii găsiți, aplicăm rezolvarea de la
subtask-ul $2$. Putem precalcula aceste poziții succesor/predecesor ale
caracterelor $b, i, n, g, o$ pentru fiecare poziție din șir sau le putem
căuta binar la fiecare pas. Pentru găsirea rezultatului pentru un șir
de lungime $N$, complexitatea este $O(N \cdot \log N)$ sau $O(N)$. Trebuie
menționat că avem un factor constant aproximativ egal cu $120-130$.

## Soluție alternativă (Vlad-Mihai Bogdan, Universitatea din București)

Pentru fiecare poziție $i$, vom încerca să formăm șirul bingo pe
pozițiile $i, i + 1, \dots, i + 4$. Pentru fiecare literă $l$, care ar
urma să fie pe poziția i + k în șirul cu subsecvența bingo pe pozițiile
$i, i + 1, \dots, i + 4$, avem cel mult trei opțiuni:

- Să luăm litera $l$ din stânga poziției $i + k$, dacă este posibil;
- Să luăm litera $l$ din dreapta poziției $i + k$, dacă este posibil;
- Să luăm litera $l$ chiar de pe poziția $i + k$, dacă este posibil.

Astfel, pentru fiecare poziție $i$, avem cel mult $3^5$ configurații
posibile de alegeri pentru cele $5$ litere. Vom calcula numărul de
swap-uri necesare pentru a aduce literele pe pozițiile $i, i + 1, \dots, i + 4$
în ordinea pozițiilor lor din șirul inițial (spre exemplu, dacă
decidem să luăm litera n de pe o poziție de dinaintea poziției literei
i, ordinea în care vom aduce literele este $n$, iar mai apoi $i$). La
final, trebuie să adăugăm la răspuns numărul de swap-uri necesare
pentru a transforma anagrama rezultată în șirul bingo. După
precalcularea numărului de swap-uri necesare pentru fiecare anagramă a
cuvântului bingo, complexitatea devine $O(N)$, dar cu o constantă
aproximativ egală cu $243$.

## Rezolvare

Mai jos puteți găsi soluția autorului care ia punctajul maxim.

```cpp
// Popa Sebastian, Universitatea din Bucuresti
#include <bits/stdc++.h>
using namespace std;
ifstream f("bingo.in");
ofstream g("bingo.out");
const string bingo = "bingo";
void get_pre();
int solve(const string &);
int solve2(const string &, const string &);
int solve3(const vector<int>);
int letind[130];
vector<int> indpre[5], indsuf[5], pre;
vector<string> allp;
int main() {
    get_pre();
    string s;
    int t;
    for (f >> t; t; t--) f >> s, g << solve(s) << '\n';
    return 0;
}
int solve(const string &s) {
    // precalculam pt fiecare pozitie prima aparitie din stanga si prima
    // din dreapta a caracterelor {b,i,n,g,o}
    for (int i = 0; i < 5; i++)
        indpre[i].assign(s.size(), -1), indsuf[i].assign(s.size(), -1);
    for (int i = 0; i < (int)s.size(); i++)
        for (int j = 0; j < 5; j++)
            if (s[i] == bingo[j])
                indpre[j][i] = i;
            else if (i)
                indpre[j][i] = indpre[j][i - 1];
    // indpre[j][i] = primul caracter egal cu bingo[j] din stanga lui i
    // (inclusiv)
    for (int i = s.size() - 1; i >= 0; i--)
        for (int j = 0; j < 5; j++)
            if (s[i] == bingo[j])
                indsuf[j][i] = i;
            else if (i != s.size() - 1)
                indsuf[j][i] = indsuf[j][i + 1];
    // indsuf[j][i] = priul caracter egal cu bingo[j] din dreapta lui i
    // (inclusiv)

    static int minim;
    minim = (1 << 28);
    // pentru fiecare anagrama vedem cum o putem obtine optim drept substring
    // dupa care adaugam costul de transformare a ei in bingo
    for (int i = 0; i < 120; i++)
        minim = min(minim, solve2(s, allp[i]) + pre[i]);
    return minim;
}
int solve2(const string &s,
           const string &p)  // pentru fiecare caracter egal cu cel din mijloc
// gasim cele mai apropiate pozitii pentru celelalte caractere
{
    static int minim;
    minim = (1 << 28);
    for (int i = 0, p0, p1, p2, p3, p4; i < (int)s.size(); i++)
        if (s[i] == p[2]) {
            p2 = i;
            p1 = indpre[letind[p[1]]][p2];
            if (p1 == -1) continue;
            p0 = indpre[letind[p[0]]][p1];
            if (p0 == -1) continue;
            p3 = indsuf[letind[p[3]]][p2];
            if (p3 == -1) continue;
            p4 = indsuf[letind[p[4]]][p3];
            if (p4 == -1) continue;
            minim = min(minim, solve3({p0, p1, p2, p3, p4}));
        }
    return minim;
}
int solve3(const vector<int> v)  // cate mutari trebuie facute sa aducem
                                 // caracterele langa cel din mijloc
{
    static int s;
    s = 0;
    for (int j = 0; j < 5; j++)
        if (j < 2)
            s += v[2] - v[j] + j - 2;
        else
            s += v[j] - v[2] + 2 - j;
    return s;
}
void get_pre()  // precalculam toate anagramele sirului bingo (allp[]) si cate
                // inversiuni are fiecare   (pre[])
{
    string s;
    int cnt, i0, i1, i2, i3, i4;
    for (int i = 0; i < 5; i++) letind[bingo[i]] = i;
    for (i0 = 0; i0 < 5; i0++)  // instead of next_permutation()
        for (i1 = 0; i1 < 5; i1++)
            if (i1 != i0)
                for (i2 = 0; i2 < 5; i2++)
                    if (i2 != i0 && i2 != i1)
                        for (i3 = 0; i3 < 5; i3++)
                            if (i3 != i0 && i3 != i1 && i3 != i2) {
                                i4 = 10 - i0 - i1 - i2 - i3;
                                s = "", cnt = 0;
                                s += bingo[i0], s += bingo[i1], s += bingo[i2];
                                s += bingo[i3], s += bingo[i4];
                                for (int i = 0; i < 4; i++)
                                    for (int j = i + 1; j < 5; j++)
                                        if (letind[s[i]] > letind[s[j]]) cnt++;
                                allp.push_back(s), pre.push_back(cnt);
                            }
}
```
