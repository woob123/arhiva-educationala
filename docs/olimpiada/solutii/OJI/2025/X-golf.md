---
id: OJI-2025-X-golf
title: Soluția problemei Golf (OJI 2025, clasa a X-a)
problem_id: 3632
authors: [onut]
prerequisites:
    - lee
    - partial-sums
tags:
    - OJI
    - clasa X
---

## Subtask 1

Pentru a determina numărul de celule din golf ce sunt umplute cu pămânțse pot
citi caracterele (de 0 sau 1) ce reprezintă elementele matricei $A$, unul câte
unul. Așadar, într-o variabilă $E$, se poate reține răspunsul pentru prima
cerință: astfel, pentru fiecare caracter de 1 întâlnit în $A$, se incrementează
valoarea lui $E$. La final, după ce toate cele $n \times m$ elemente din matrice
au fost citite, se afișează valoarea lui $E$. Complexitatea totală de timp este
de $\mathcal{O} (n \cdot m)$, iar spațiul utilizat se încadrează în
$\mathcal{O}(1)$ (nu este nevoie să stocăm niciun tablou/vector — totul se
efectuează cu ajutorul unui număr mic, constant de variabile).

## Subtask 2

De vreme ce se garantează (prin tabelul cu Restricții și precizări din enunț) că
există o singură insulă în golf, înseamnă că numărul de insule (din [Golful
Biscayne](https://en.wikipedia.org/wiki/Biscayne_Bay)) ce conțin un număr maxim
de insule (adică răspunsul la cerința de rezolvat) este chiar 1.

## Subtask 3

Pentru a determina numărul de celule ce fac parte din fiecare insulă a golfului,
se poate folosi un algoritm ce adaugă, în timpul descoperirii unei insule,
fiecare element de 1 într-o structură de date similară cu o coadă exact o
singură dată (spre exemplu, folosind un tablou auxiliar în care se marchează
vizitarea elementelor în cadrul unei iterații anterioare), cum ar fi Algoritmul
lui Lee; dacă dorim să folosim o metodă recursivă, recomandăm și Algoritmul de
tip Flood Fill. Pentru fiecare insulă $\alpha$, se poate stoca și o variabilă
$\mathrm{num}_\alpha$ (de tip `#!cpp int`) care reține numărul de celule umplute
cu pământ ce intră în alcătuirea sa. Apoi, printr-o parcurgere a acestei
structuri de date (de exemplu, `#!cpp struct`, în C/C++) ce reține informații
despre insule (structura conține, în total, cel mult $\mathcal{O}(n \times m)$
elemente, corespunzătoare insulelor), se calculează numărul elementelor
$\mathrm{num}_\alpha$ maximale. Astfel, atât complexitatea de timp, cât și cea
de spațiu, sunt de $\mathcal{O}(n \times m)$.

## Subtask 4

Pentru fiecare interogare dintre cele $Q$, se pot găsi insulele ce
_influențează_ (altfel spus, modifică) rezultatul, într-o manieră similară cu
cea pentru al treilea subtask. Astfel, să presupunem că avem de rezolvat o
interogare de tipul $(C, p)$ (unde $1 ≤ p ≤ m$). Fie $I_\alpha =
{(𝑥_{\alpha,1}, 𝑦_{\alpha,1}), (𝑥_{\alpha,2}, 𝑦_{\alpha,2}), . . . ,
(𝑥_{\alpha,k_\alpha} , 𝑦_{\alpha,k_\alpha} )}$ mulțimea ce conține **toate**
cele $k_\alpha$ celule (reprezentate prin perechi formate din linia
$x_{\alpha,i}$, respectiv coloana $y_{\alpha,i}$ — coordonatele celulelor în
matricea $A$) ce intră în alcătuirea unei insule $\alpha$; spunem că insula
$\alpha$ are dimensiunea egală cu $k_\alpha$.

Dacă $\max(y_{\alpha,1}, y_{\alpha,2}, ..., y_{\alpha,k_\alpha}) < p$,
înseamnă că insula $\alpha$ este situată la stânga coloanei $p$, așa că valoarea
lui $b$ (adică, conform enunțului, numărul de celule din toate insulele ce se
află la stânga coloanei $p$, în cadrul interogării curente) crește cu
$k_\alpha$; altfel, dacă $\min(y_{\alpha,1}, y_{\alpha,2}, ...,
y_{\alpha,k_\alpha}) > p$, înseamnă că insula $\alpha$ este situată la dreapta
coloanei $p$, așa că valoarea lui $b$ (numărul de celule din toate insulele ce
se află la dreapta coloanei $p$) crește cu $k_\alpha$. În orice alt caz, valorile
lui $a$ și $b$ nu se modifică (cu ocazia procesării insulei curente $\alpha$).

În mod similar, putem raționa pentru o interogare de tipul $(L, p)$ (unde $1 ≤ p
≤ n$) – în loc să folosim coordonatele de tip coloană $y_{\alpha,i}$, le vom
folosi pe cele de tip linie $x_{\alpha,i}$ .

Prin urmare, complexitatea totală de timp este de $\mathcal{O}(Q \cdot n \cdot
m)$, iar cea de spațiu este de $\mathcal{O}(n \cdot m)$.

## Subtask 5

De vreme ce interog ările de tip $(L, p)$ pot fi rezolvate într-un mod analog cu
cele de tip $(C, p)$ (de exemplu, prin _înlocuirea_ liniilor matricei cu
coloanele acesteia și viceversa, sau prin folosirea unei structuri de date de
forma `#!cpp std::pair<int, int>`, în C++), putem să presupunem, pentru
simplicitate, că avem de răspuns doar la interogări de tipul $(C, p)$.

Determinarea tuturor insulelor $\alpha$ (adică a coordonatelor celulelor ce
intră în alcătuirea unei insule și a dimensiunii acesteia) se poate efectua în
complexitatea (totală) de timp $\mathcal{O}(n \cdot m)$, așa cum a fost descris
anterior.

În cadrul unei insule $\alpha$, să introducem următoarele două notații:

-   $\mu_{y,\alpha} = \max(y_{\alpha,1}, y_{\alpha,2}, ..., y_{\alpha,k_\alpha})$
-   $\omega_{y,\alpha} = \min(y_{\alpha,1}, y_{\alpha,2}, ..., y_{\alpha,k_\alpha})$

Mai mult, introducem și următoarele două tablouri unidimensionale
$l[1, ..., m]$ și $r [1, ..., m]$, definite astfel (pentru fiecare $1 ≤ i ≤ m$):

$$
l[i] = \sum_{\substack{\alpha \\ \mu_{y,\alpha} = i}} k_\alpha;
\qquad r[i] = \sum_{\substack{\alpha \\ \omega_{y,\alpha} = i}} k_\alpha
$$

Cu alte cuvinte, $l[i]$ reprezintă suma dimensiunilor insulelor din golf ce au
valoarea maximală a unei coloane (a unei celule din componență) egală cu $i$.
Similar, $r[i]$ reprezintă suma dimensiunilor insulelor din golf ce au valoarea
minimală a unei coloane egală cu $i$.

Așadar, răspunsul pentru o interogare de tipul $(C, p)$ este dat de valoarea
expresiei $a \cdot b$, unde:

$$
a
= \sum_{\substack{\alpha \\ \mu_{y,\alpha} < p}} k_\alpha
= \sum_{\substack{\alpha \\ \mu_{y,\alpha} \in \{1, ..., p-1\}}} k_\alpha
= \sum_{i = 1}^{p - 1} l[i]
$$

și

$$
b
= \sum_{\substack{\alpha \\ \omega_{y,\alpha} < p}} k_\alpha
= \sum_{\substack{\alpha \\ \omega_{y,\alpha} \in \{p + 1, ..., m\}}} k_\alpha
= \sum_{i = p + 1}^{m} r[i]
$$

Atât $a$, cât și $b$, pot fi calculate printr-o parcurgere (liniară) a
tablourilor $l[1, ..., m]$ și $r[1, ..., m]$. În funcție de elementul $i$ de
analizat (dacă $i < p$ sau $i > p$), se actualizează valoarea lui $a$ sau
valoarea lui $b$.

Astfel, complexitatea de timp necesară pentru a rezolva o interogare de tipul
$(C, p)$ este de $\mathcal{O}(m)$. Similar, complexitatea de timp necesară
pentru a rezolva o interogare de tipul $(L, p)$ este de $\mathcal{O}(n)$.

Prin urmare, complexitatea totală a acestui subtask este de $\mathcal{O} (n
\cdot m + Q \cdot \max (n, m))$.

## Soluție completă pentru $T = 3$

Aplicăm o metodă de rezolvare similară cu cea pentru al cincilea subtask.

Observăm că, în cadrul unei interogări, pentru a determina, de exemplu, valoarea
lui $a$, putem utiliza o tehnică de sume parțiale, pe prefixele șirului $l
[1, ..., m]$; de asemenea, valoarea lui $b$ poate fi calculată tot cu sume
parțiale, pe sufixele șirului $r [1, ..., m]$. Procesarea sumelor parțiale se
poate efectua în complexitatea de timp (cât și de spațiu) de $\mathcal{O}(m)$
(pentru coloane) sau de $\mathcal{O}(n)$ (pentru linii). Apoi, rezolvarea
oricărei actualizări se poate efectua în $\mathcal{O}(1)$. Complexitatea de timp
totală a acestei soluții este de $\mathcal{O}(n \cdot m + Q)$. Spațiul utilizat
se încadrează în $\mathcal{O}(n \cdot m)$.

## Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
/*
    Autor: Andrei Onut (Yale University)
    Problema: "Golf" (OJI 2025, clasa a X-a)
*/

#include <algorithm>
#include <fstream>
#include <queue>
#include <string>
#include <vector>

int my_min(const int a, const int b) { return ((a < b) ? a : b); }
int my_max(const int a, const int b) { return ((a > b) ? a : b); }

struct island {
    int first_line, first_column;
    int last_line, last_column;
    int size;

    island(const int x, const int y)
        : first_line(x),
          first_column(y),
          last_line(x),
          last_column(y),
          size(1) {}

    void update(const int x, const int y) {
        first_line = my_min(first_line, x),
        first_column = my_min(first_column, y);
        last_line = my_max(last_line, x), last_column = my_max(last_column, y);
        ++size;

        return;
    }
};

template <typename T>
std::string T_to_string(T x) {
    if (x == (T)0) {
        return "0";
    }

    std::string answer;
    while (x) {
        answer += ((char)((x % (T)10) + '0')), x /= (T)10;
    }

    reverse(answer.begin(), answer.end());
    return answer;
}

class golf_solver {
    const std::string problem_name = "golf", dot = ".", input_format = "in",
                      output_format = "out";

    const int dx[4] = {(-1), 0, (+1), 0}, dy[4] = {0, (+1), 0, (-1)};

    int T, n, m, q;

    std::vector<std::string> A;

    std::vector<std::pair<char, int>> queries;
    std::vector<long long> answer;

    std::vector<island> islands_arr;

    int answer_1, answer_2;

    island find_island(const int i, const int j,
                       std::vector<std::vector<bool>> &used) {
        island answer(i, j);

        std::queue<std::pair<int, int>> Q;

        used[i][j] = true, Q.push({i, j});
        while (!Q.empty()) {
            for (int dir = 0; dir < 4; ++dir) {
                int x(Q.front().first + dx[dir]), y(Q.front().second + dy[dir]);

                if ((x < 0) || (x >= n) || (y < 0) || (y >= m)) {
                    continue;
                }

                if (A[x][y] == '0') {
                    continue;
                }
                if (used[x][y] == true) {
                    continue;
                }

                used[x][y] = true, Q.push({x, y});
                answer.update(x, y);
            }

            Q.pop();
        }

        return answer;
    }

    void solve_T_1() {
        for (const island &it : islands_arr) {
            answer_1 += it.size;
        }

        return;
    }

    void solve_T_2() {
        if (islands_arr.empty()) {
            return;
        }

        int max_size(-1);
        for (const island &it : islands_arr) {
            if (it.size > max_size) {
                max_size = it.size, answer_2 = 1;
            } else if (it.size == max_size) {
                ++answer_2;
            }
        }

        return;
    }

    std::vector<std::pair<std::pair<int, int>, int>> extract_segments(
        const int axis) {
        std::vector<std::pair<std::pair<int, int>, int>> answer;

        for (const island &it : islands_arr) {
            if (axis == 0) {
                answer.push_back({
                    {it.first_column, it.last_column},
                    it.size
                });
            } else {
                answer.push_back({
                    {it.first_line, it.last_line},
                    it.size
                });
            }
        }

        return answer;
    }

    std::pair<std::vector<int>, std::vector<int>> get_sums(const int axis,
                                                           const int elements) {
        std::vector<int> prefix(elements, 0), suffix(elements, 0);

        std::vector<std::pair<std::pair<int, int>, int>> segments =
            extract_segments(axis);

        std::vector<int> end_num(elements, 0), start_num(elements, 0);
        for (const std::pair<std::pair<int, int>, int> &it : segments) {
            end_num[it.first.second] += it.second,
                start_num[it.first.first] += it.second;
        }

        prefix[0] = end_num[0],
        suffix[(elements - 1)] = start_num[(elements - 1)];
        for (int i = 1; i < elements; ++i) {
            prefix[i] = (prefix[(i - 1)] + end_num[i]),
            suffix[(elements - 1 - i)] =
                (suffix[(elements - i)] + start_num[(elements - 1 - i)]);
        }

        return {prefix, suffix};
    }

    long long evaluate(const int q_idx, const int elements,
                       const std::pair<std::vector<int>, std::vector<int>> &V) {
        if ((q_idx == 1) || (q_idx == elements)) {
            return 0LL;
        }
        return (1LL * V.first[(q_idx - 2)] * V.second[q_idx]);
    }

    void solve_T_3() {
        std::pair<std::vector<int>, std::vector<int>> columns = get_sums(0, m),
                                                      lines = get_sums(1, n);

        int i = 0;
        for (const std::pair<char, int> &it : queries) {
            answer[(i++)] = ((it.first == 'C') ? evaluate(it.second, m, columns)
                                               : evaluate(it.second, n, lines));
        }

        return;
    }

public:
    golf_solver() : T(0), n(0), m(0), q(0), answer_1(0), answer_2(0) {}

    void read() {
        std::ifstream f(problem_name + dot + input_format);

        f >> T >> n >> m;

        A = std::vector<std::string>(n, std::string(m, ' '));
        for (int i(0); i < n; ++i) {
            f >> A[i];
        }

        if (T == 3) {
            f >> q;

            queries = std::vector<std::pair<char, int>>(q),
            answer = std::vector<long long>(q);
            for (int i(0); i < q; ++i) {
                f >> queries[i].first >> queries[i].second;
            }
        }

        f.close();
        return;
    }

    void pre_compute() {
        std::vector<std::vector<bool>> used(n, std::vector<bool>(m, false));

        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if ((A[i][j] == '1') && (used[i][j] == false)) {
                    islands_arr.push_back(find_island(i, j, used));
                }
            }
        }

        return;
    }

    void solve() {
        ((T == 1) ? solve_T_1() : ((T == 2) ? solve_T_2() : solve_T_3()));
        return;
    }

    void print() {
        std::ofstream g(problem_name + dot + output_format);

        std::vector<std::string> output_lines(((T <= 2) ? 1 : q));

        if (T == 1) {
            output_lines[0] = T_to_string<int>(answer_1);
        } else if (T == 2) {
            output_lines[0] = T_to_string<int>(answer_2);
        } else {
            for (int i = 0; i < q; ++i) {
                output_lines[i] = T_to_string<long long>(answer[i]);
            }
        }

        for (const std::string &line : output_lines) {
            g << line << '\n';
        }

        g.close();
        return;
    }
};

int main() {
    golf_solver S;

    S.read();
    S.pre_compute();
    S.solve();
    S.print();

    return 0;
}
```
