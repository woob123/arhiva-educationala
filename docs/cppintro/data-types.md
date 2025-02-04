---
id: data-types
author:
    - Ștefan-Cosmin Dăscălescu
    - Ștefan-Iulian Alecu
prerequisites:
    - intro
tags:
  - C++
  - introducere
  - variabile
  - tipuri de date
---

Așa cum ați observat atunci când am prezentat primul program în articolul
precedent, fiecare componentă are mai multe caracteristici, una dintre cele mai
importante fiind variabilele, respectiv tipul de date al acestora.

Ulterior, voi împărți în cele ce urmează tipurile de date simple în funcție de
valorile care sunt păstrate.

Totuși, unele tipuri de date mai complexe, cum ar fi
[vectorii](../cppintro/arrays.md) sau
[matricile](../cppintro/matrices.md), vor fi discutate în
articolele lor corespunzătoare.

## Variabile

!!! info "Definiție"

    O variabilă este o locație de memorie care stochează o valoare de un anumit
    tip. Aceasta este caracterizată printr-o adresă, un identificator, un tip de
    date și un domeniu de vizibilitate.

Orice variabilă este caracterizată de:

1. **Adresa memoriei**: Este locația din memorie unde este stocată variabila. Nu
   lucrăm cu ea în timpul programelor, dar aceasta poate fi utilă în
   diagnosticarea problemelor de memorie. Adresa este exprimată în hexazecimal
   (de exemplu, `#!cpp 0x7ffe3278f424`).

2. **Identificatorul (numele)**: Reprezintă legătura între variabilă și valoarea
   sa din memorie. Un identificator trebuie să respecte următoarele reguli:

      - Poate conține litere ale alfabetului englez (a-z, A-Z), cifre (0-9) și
        caracterul underscore (bară jos, `_`).
      - Trebuie să înceapă cu o literă sau `_`, dar niciodată cu o cifră.
      - Nu poate fi un cuvânt rezervat al limbajului (ex.: `#!cpp int`, `#!cpp
        return`, `#!cpp while` etc.).
      - Identificatorul este case-sensitive, astfel încât `Varsta`, `varsta` și
        `VARSTA` sunt trei identificatori diferiți.

3. **Tipul de date**: Determină ce tip de valori poate să stocheze variabila și
   limitele acestor valori (ex.: numere întregi, numere în virgulă mobilă,
   caractere etc.).

4. **Domeniul de vizibilitate (scope)**: Definește partea programului în care
   variabila este accesibilă. Variabilele pot fi:

      - **Globale**: disponibile în întregul program, sau
      - **Locale**: disponibile doar în cadrul funcției sau blocului în care
        sunt definite.

În C/C++, este necesar să specificăm tipul variabilei la momentul declarației.
Sintaxa generală este:

```cpp
--8<-- "cppintro/data_types/datatypes1.cpp"
```

unde:

- `tip_de_date` specifică tipul valorii pe care o poate stoca variabila (ex.:
  `#!cpp int`, `#!cpp float`, `#!cpp double`, `#!cpp char`).
- `identificator1, identificator2, ...` reprezintă numele variabilelor.

!!! example

    ```cpp
    --8<-- "cppintro/data_types/datatypes2.cpp"
    ```

!!! note "Observații"

    - Dimensiunea memoriei ocupate de o variabilă depinde de
      tipul de date (ex.: `#!cpp int` ocupă 4 bytes în majoritatea sistemelor).

    - Dacă o variabilă consumă mai multă memorie decât este disponibilă, veți primi
      verdictul _limită de memorie depășită_ sau, în limba engleză, _memory limit
      exceeded_.

## Tipuri de date ce păstrează numere întregi

Tipurile de date numerice sunt folosite pentru a stoca valori întregi. Unele
dintre aceste tipuri pot avea aplicații suplimentare; de exemplu, tipul `#!cpp
char` poate fi folosit și pentru a opera cu caractere, fiecare valoare având
drept corespondent un caracter din codul ASCII (mai multe detalii vor fi oferite
în secțiunea dedicată șirurilor de caractere).

În ordinea popularității lor, tipurile de date întregi sunt următoarele:

1. `#!cpp int`
      - Cel mai frecvent folosit tip de date pentru a stoca numere întregi.
      - Stochează valori între -2<sup>31</sup> și 2<sup>31</sup> - 1 (echivalent
        cu -2,147,483,648 și 2,147,483,647).
      - Ocupă 32 de biți (4 bytes) în memorie.

2. `#!cpp long long`
      - Un tip de date folosit pentru numerele întregi mai mari.
      - Având limite între -2<sup>63</sup> și 2<sup>63</sup> - 1
        (-9,223,372,036,854,775,808 și 9,223,372,036,854,775,807).
      - Numere de maxim 19 cifre, stocate pe 64 de biți (8 bytes).

3. `#!cpp char`
      - Tipul de date folosit pentru lucrul cu caractere.
      - Având limite între -128 și 127.
      - Ocupă 1 byte.

4. `#!cpp bool`
      - Folosit pentru a păstra doar valori binare.
      - Valori posibile: 1 sau 0 (corespunzător stărilor de `#!cpp true`
        (adevărat) și `#!cpp false` (fals)).

5. `#!cpp short`
      - Un tip de date pentru numere întregi mai mici.
      - Având limite între -2<sup>15</sup> și 2<sup>15</sup> - 1 (-32,768 și
        32,767).
      - Ocupă 16 biți (2 bytes).

!!! note "Observație"

    Printre altele, există și tipul de date `#!cpp __int128`, care ne permite să
    păstrăm numere pe 128 de biți, despre care vom vorbi ulterior în articol,
    deoarece cunoașterea lui nu este necesară decât pentru aplicații mai
    avansate de la olimpiadă.

### Tipurile de date `#!cpp unsigned`

Uneori, putem fi în situația în care să avem nevoie de numere un pic mai mari
decât limitele acestor tipuri de date, fără a avea memoria să folosim tipul de
date superior. Aici devin utile tipurile `#!cpp unsigned`.

Pentru toate aceste tipuri, cu excepția tipului `#!cpp bool`, există și varianta
`#!cpp unsigned`, care ne dă posibilitatea să păstrăm numere de două ori mai
mari cu aceeași memorie, prețul fiind faptul că nu mai avem cum să păstrăm
valori negative.

De exemplu, tipul de date `#!cpp unsigned int` poate păstra în memorie valori
între 0 și 2<sup>32</sup> - 1, intervalul fiind [0, 4.294.967.295].

De cele mai multe ori, tipurile de date pe care le folosim sunt
`#!cpp unsigned int`, respectiv `#!cpp unsigned long long`.

### Tipul de date `#!cpp void`

Pe lângă tipurile de date menționate anterior, există și tipul de date void,
care deși nu are valori și operații, este necesar pentru a indica faptul că o
metodă, o funcție sau un program nu returnează nimic, așa cum veți vedea
ulterior când studiați [funcțiile](./functions.md).

### Sfaturi practice și evitarea overflow-ului

De regulă, atâta timp cât ne permite memoria, folosirea tipului `#!cpp int` este
mai mult decât suficientă, asigurând echilibrul perfect între un consum optim de
timp și memorie pentru operații, respectiv stocarea unor valori suficient de
mari pentru calculele noastre.

!!! info "Overflow"

    Atunci când valoarea păstrată într-un tip de date depășește valoarea maximă
    permisă, se întâmplă ceea ce numim overflow. Cu alte cuvinte, valoarea se
    întoarce la valoarea minimă permisă, ceea ce face calculele viitoare să
    devină eronate.

!!! note "Underflow"

    În mod similar, putem vorbi și de underflow, când valorile devin mai mici
    decât valorile minime permise, dar această situație se întâlnește mult mai
    rar în practică.

Un exemplu tipic de overflow se poate întâlni atunci când adunăm sau înmulțim
două valori care deși ambele se încadrează în `#!cpp int`, suma (sau produsul
lor) depășește valoarea maximă a tipului de date `#!cpp int`.

```cpp
--8<-- "cppintro/data_types/datatypes3.cpp"
```

Cele mai populare două soluții pentru evitarea overflow-ului sunt fie folosirea
tipului de date long long pentru păstrarea termenilor din operații, fie
folosirea operatorului `#!cpp 1LL`, fie folosirea `#!cpp (long long)` pentru
convertirea datelor.

!!! note "Numere mai mari"

    În unele probleme, chiar și numerele pe 64 (sau 128) de biți nu sunt
    îndeajuns de mari, așa că se impune păstrarea datelor sub formă de vectori,
    mai multe detalii despre asta puteți găsi în [acest
    articol](../mediu/bignum.md), după ce vă familiarizați cu
    lucrul cu algoritmi mai dificili decât scopul acestui articol.

## Tipuri de date reale

Tipurile de date reale sunt folosite pentru a stoca valori reale. Acestea sunt
folosite mai ales în unele calcule matematice mai complexe, precum calculele
geometrice sau în privința unor ecuații.

Tipurile de date reale sunt următoarele:

- tipul `#!cpp float` - limite între aproximativ -10<sup>38</sup> și
  10<sup>38</sup>.
- tipul `#!cpp double` - limite între aproximativ -10<sup>208</sup> și
  10<sup>208</sup>, precizie crescută.
- tipul `#!cpp long double` - limite mai mari decât cele de la `#!cpp double`,
  precizie variabilă.

În general, atunci când operăm cu numerele reale, vrem să avem grijă la erorile
de precizie care ar putea apărea. Cel mai bun mod de a face acest lucru, în
special când lucrăm cu valori foarte mari în modul, este acela de a include o
valoare reală foarte mică, $eps$, care să servească drept etalon în ceea ce
privește verificarea egalității.

!!! note "Observație"

    Atunci când scriem coduri mai simple sau avem de-a face cu un examen de
    tipul celui de bacalaureat, nu este nevoie să folosim această metodă pentru
    a verifica egalitatea dintre numere, putem verifica folosind metoda uzuală.

Astfel, pentru a verifica egalitatea dintre două numere reale mai mari, putem
incorpora `eps` astfel:

```cpp
--8<-- "cppintro/data_types/datatypes4.cpp"
```

### Tipul `#!cpp __int128`

Pe lângă aceste tipuri, există și tipul de date `#!cpp __int128` care ne permite
să stocăm valori pe 128 de biți, având limite între -2<sup>127</sup> si
2<sup>127</sup> - 1 (numere de aproximativ 37 de cifre). Acest tip poate fi
folosit doar pe [compilatorul
GCC](./compilers/windows/mingw64.md).

Noi nu putem să citim și să afișăm direct `#!cpp __int128`, deci va trebui să ne
implementăm noi citirea și afișarea. Pentru simplitate, dacă implementăm cum
este mai jos, vom putea să folosim `#!cpp std::cin >> x` chiar dacă $x$ este
`#!cpp __int128`.

```cpp
--8<-- "cppintro/data_types/datatypes5.cpp"
```

## Concluzii

Tipurile de date simple sunt cele cu care operăm în aproape toate programele pe
care le scriem vreodată în C++ (și în alte limbaje de programare care au aceste
tipuri de date), iar înțelegerea lor este fundamentală pentru a putea deveni
programatori tot mai buni.

## Resurse suplimentare

- [Data types - USACO Guide](https://usaco.guide/general/data-types?lang=cpp)
- [Tipuri de date - pbinfo](https://www.pbinfo.ro/articole/58/tipuri-de-date-c-cpp)
