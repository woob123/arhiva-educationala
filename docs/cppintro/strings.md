---
id: strings
author:
  - Ștefan-Cosmin Dăscălescu
  - Denisa-Maria Ursu
  - Ștefan-Iulian Alecu
prerequisites:
  - data-types
  - arrays
  - matrices
tags:
  - C++
  - implementare
  - optimizare
  - siruri de caractere
toc_depth: 3
---

## Introducere

Un șir de caractere este un tablou care stochează caractere. În limbajul C++,
putem stoca aceste șiruri în două moduri: fie folosind un tablou static, similar
limbajului C, fie folosind tipul de date `#!cpp std::string`. În acest articol,
vom analiza ambele opțiuni, incluzând avantajele și dezavantajele lor.

## Tipul `#!cpp char` și tabelul ASCII

Așa cum ați văzut când am discutat despre [tipurile de date](./data-types.md),
limbajul C++ dispune de tipul de date `#!cpp char`, utilizat pentru a stoca
caractere. Deși valorile stocate într-un `#!cpp char` sunt în intervalul
[-128, 127] (pentru reprezentări cu semn), cele mai utilizate caractere sunt
cele afișabile, care aparțin intervalului [32, 127].

!!! note "Codificarea caracterelor"

    Standardul care atribuie valori caracterelor se numește ASCII (American
    Standard Code for Information Interchange). Caracterele din intervalul
    [0, 31] nu sunt afișabile, fiind utilizate pentru instrucțiuni de sistem.

Printre cele mai importante coduri ASCII sunt:

- **32** (spațiu)
- **48-57**: cifrele 0-9
- **65-90**: literele mari ale alfabetului englez (A-Z)
- **97-122**: literele mici ale alfabetului englez (a-z)

### Transformări între litere și cifre

Putem simplifica lucrul cu caractere folosind valorile ASCII, fără a gestiona
direct codurile. De exemplu:

- **Cifre**: Scădeți '0' din caracter.
- **Litere**: Adăugați sau scădeți 32 pentru a transforma între litere mari și
  mici.

```cpp
--8<-- "cppintro/strings/strings01.cpp"
```

Lista completă a codurilor pentru caractere se găsește
[aici](https://www.sciencebuddies.org/science-fair-projects/references/ascii-table).
Pentru conveniență, o voi reproduce aici:

<!-- markdownlint-disable MD038 -->

--8<-- "cppintro/strings/ascii.md"

## Funcții pe tipul `#!cpp char`

Biblioteca standard `#!cpp <cctype>` din C++ oferă un set de funcții predefinite
utile pentru verificarea proprietăților caracterelor individuale. Aceste funcții
simplifică validarea și manipularea stringurilor, fiind ideale pentru lucrul cu
date de tip text.

<!-- markdownlint-disable-file MD024 -->
### Funcția `#!cpp isdigit`

```cpp
int isdigit(int ch);
```

Verifică dacă un caracter anume este una din cele zece cifre zecimale:
(0123456789).

#### Parametri

- `#!cpp ch` - caracterul de clasificat

#### Valoare returnată

O valoare diferită de zero în cazul în care caracterul este un caracter numeric,
zero în caz contrar.

#### Exemplu

```c++
--8<-- "cppintro/strings/strings02.cpp:isdigit"
```

### Funcția `#!cpp isalpha`

```cpp
int alpha(int ch);
```

Verifică dacă un caracter anume este alfabetic. Următoarele caractere sunt
considerate alfabetice:

- literele mari ABCDEFGHIJKLMNOPQRSTUVWXYZ
- literele mici abcdefghijklmnopqrstuvwxyz

#### Parametri

- `#!cpp ch` - caracterul de clasificat

#### Valoare returnată

O valoare diferită de zero în cazul în care caracterul este alfabetic, zero în
caz contrar.

#### Exemplu

```cpp
--8<-- "cppintro/strings/strings02.cpp:isalpha"
```

### Funcția `#!cpp isalnum`

```cpp
int isalnum(int ch);
```

Verifică dacă un caracter anume este alfanumeric. Următoarele caractere sunt
considerate alfanumerice:

- literele mari ABCDEFGHIJKLMNOPQRSTUVWXYZ
- literele mici abcdefghijklmnopqrstuvwxyz
- cifrele 0123456789

!!! note "Observație"
    `#!cpp isalnum(ch)` este echivalent cu `#!cpp isdigit(ch) && isalpha(ch)`.

#### Parametri

- `#!cpp ch` - caracterul de clasificat

#### Valoare returnată

O valoare diferită de zero în cazul în care caracterul este alfanumeric, zero în
caz contrar.

#### Exemplu

```cpp
--8<-- "cppintro/strings/strings02.cpp:isalnum"
```

### Funcțiile `#!cpp islower` și `#!cpp isupper`

```cpp
int islower(int ch);
int isupper(int ch);
```

Verifică dacă un caracter anume este clasificat ca fiind unul mic, respectiv
mare. Implicit, C consideră literele mari ca fiind ABCDEFGHIJKLMNOPQRSTUVWXYZ,
iar cele mici ca abcdefghijklmnopqrstuvwxyz.

#### Parametri

- `#!cpp ch` - caracterul de clasificat

#### Valoare returnată

O valoare diferită de zero în cazul în care caracterul este mic, respectiv mare,
zero în caz contrar.

#### Exemplu

```cpp
--8<-- "cppintro/strings/strings02.cpp:islower"

--8<-- "cppintro/strings/strings02.cpp:isupper"
```

### Funcțiile `#!cpp tolower` și `#!cpp toupper`

```cpp
int toupper(int ch);
int tolower(int ch);
```

Convertește un caracter dat într-o literă mare, respectiv mică, respectiv mare.
C consideră literele mari ca fiind ABCDEFGHIJKLMNOPQRSTUVWXYZ, iar cele mici ca
abcdefghijklmnopqrstuvwxyz.

#### Parametri

- `#!cpp ch` - caracterul de convertit

#### Valoare returnată

Caracterul convertit sau `ch` dacă nu se poate efectua conversia.

#### Exemplu

```cpp
--8<-- "cppintro/strings/strings02.cpp:tolower"

--8<-- "cppintro/strings/strings02.cpp:toupper"
```

## Șiruri de caractere și biblioteca `#!cpp <cstring>`

Biblioteca `#!cpp <cstring>` oferă o suită de funcții utile pentru lucrul cu
șiruri de caractere în stilul C. Aceasta poate fi utilizată pentru manipularea
șirurilor de caractere (vectori de tip `#!cpp char`) prin funcții standardizate.

### Inițializarea șirurilor de caractere

Un șir de caractere poate fi inițializat în diferite moduri. Exemplele de mai
jos ilustrează câteva tehnici comune:

```cpp
--8<-- "cppintro/strings/strings03.cpp"
```

### Citirea șirurilor de caractere

În C++, există mai multe moduri de a citi șiruri de caractere, în funcție de ce
anume dorim să citim (un cuvânt, o linie completă sau un număr specificat de
caractere). Alegerea metodei de citire poate afecta rezultatul, în funcție de
modul în care sunt delimitate datele (spațiu, newline etc.).

Un șir de caractere poate fi citit în diverse moduri, fie la fel ca la vectori
(caracter cu caracter), fie putem citi toate valorile deodată, iar în funcție de
modul cum facem citirea, vom putea citi fie până la întâlnirea primului spațiu,
fie până la sfârșitul liniei.

De asemenea, se va putea observa faptul că putem citi o întreagă linie folosind
funcția getline și fixând numărul de caractere pe care vrem să-l citim.

Să presupunem că șirul de caractere este `roalgo este cel mai bun server de
informatica`. În funcție de cum citim acest șir, vom avea rezultate diferite.

Funcția `#!cpp cin.getline` are ca parametri șirul în care se va stoca
rezultatul citirii și numărul maxim de caractere permis, citind șirul până la
întâlnirea caracterului de newline, un exemplu ar fi `#!cpp cin.getline(s, x)`,
unde s este șirul dat și x este lungimea lui.

Citirea fără o funcție specifică se poate face cu cin, dar se va citi până la
primul spațiu.

Există și funcția `#!cpp cin.get()`, care poate fi folosit fie pentru un singur
caracter, fie pentru a citi $x$ caractere, dar de obicei vom avea nevoie să
citim o linie completă, deci funcția `#!cpp cin.getline()` devine mai utilă.

```cpp
--8<-- "cppintro/strings/strings04.cpp"
```

### Funcția strlen

Funcția strlen ia drept parametru un șir de caractere și returnează lungimea
acestuia.

```cpp
--8<-- "cppintro/strings/strings05.cpp:strlen"
```

### Funcția strcpy și strncpy

Funcția `strcpy` copiază un șir de caractere în alt șir, numit destinație.
Asemănător, funcția `strncpy` copiază doar primele n caractere dintr-un șir.
Aceste funcții nu adaugă caracterul `\0` la finalul șirului.

=== "strcpy"

    ```cpp
    --8<-- "cppintro/strings/strings05.cpp:strcpy"
    ```

=== "strncpy"

    ```cpp
    --8<-- "cppintro/strings/strings05.cpp:strncpy"
    ```

### Funcțiile strcat și strncat

Funcția `strcat` concatenează un șir 'destinatie' cu o copie a unui șir 'sursa'.
Concatenarea a două șiruri înseamnă alipirea acestora. Funcția `strncat`
funcționează asemănător, numai că alipește doar primele n caractere ale șirului
sursă. Iată un exemplu:

=== "strcat"

    ```cpp
    --8<-- "cppintro/strings/strings05.cpp:strcat"
    ```

=== "strncat"

    ```cpp
    --8<-- "cppintro/strings/strings05.cpp:strncat"
    ```

### Funcția strchr și strrchr

Funcțiile `strchr` și `strrchr` returnează un pointer la prima apariție,
respectiv ultima apariție a unui caracter într-un șir. Mai exact se va afișa
șirul de începând cu prima, respectiv ultima apariție a caracterului.

=== "strchr"

    ```cpp
    --8<-- "cppintro/strings/strings05.cpp:strchr"
    ```

=== "strrchr"

    ```cpp
    --8<-- "cppintro/strings/strings05.cpp:strrchr"
    ```

### Funcția strcmp

Funcția `strcmp` este folosită pentru a compara două șiruri de caractere.
Aceasta poate returna 3 valori:

- `0` - dacă șirurile sunt exact la fel;
- `-1` sau o valoare mai mică decât 0 - dacă primul șir este înaintea celuilalt,
  alfabetic vorbind;
- `1` sau o valoare mai mare decât 0 - dacă primul șir este după celălalt,
  alfabetic vorbind.

```cpp
--8<-- "cppintro/strings/strings05.cpp:strcmp"
```

### Funcția strstr

Această funcție primește două șiruri de caractere, s1 și s2, ca argumente și
găsește prima apariție a șirului s2 în șirul s1.

```cpp
--8<-- "cppintro/strings/strings05.cpp:strstr"
```

### Funcția strtok

Funcția `strtok` împarte `str[]` conform delimitatorilor dați și returnează
următorul token. Trebuie apelată într-o structură repetitivă pentru a obține
toate token-urile.

```cpp
--8<-- "cppintro/strings/strings05.cpp:strtok"
```

## Tipul de date std::string

Această structură de date este echivalentul std::vector pentru șirurile de
caractere, remarcându-se prin faptul că spre deosebire de șirurile de caractere
din C, funcția de aflare a lungimii este $O(1)$, în loc de $O(n)$. Tipul string
este unul din tipurile prezente în [STL](./stl.md),
foarte multe funcții fiind similare cu cele pe care le putem folosi cu vectori.

Pentru a putea folosi această structură de date, va trebui să includem
biblioteca `<string>`. Se remarcă faptul că există diferențe semnificative față
de cum folosim șirurile de caractere din C.

În ceea ce privește citirea, funcția getline va avea o sintaxă un pic diferită,
fiind scrisă astfel:

```cpp
--8<-- "cppintro/strings/strings06.cpp:read"
```

Sintaxa unui string va fi de tipul `string nume;`

```cpp
--8<-- "cppintro/strings/strings06.cpp:idk"
```

În mod particular, vom putea concatena două șiruri de caractere cu ușurință,
folosind operatorul `+`. Totuși, trebuie să fim atenți cum folosim acest
operator, pentru a evita efectuarea prea înceată a operațiilor.

```cpp
--8<-- "cppintro/strings/strings06.cpp:concat"
```

Deși în cazul numerelor naturale, aceste operații sunt echivalente, în cazul
stringurilor, `+=` și `+` sunt complet diferite. Prima dintre ele concatenează
șirul primit la șirul existent, cea de-a doua copiază cele două șiruri, le
unește și apoi atribuie rezultatul șirului. Această diferență devine mai
dramatică în situații precum cea de mai jos.

```cpp
--8<-- "cppintro/strings/strings06.cpp:loops"
```

Putem folosi și funcții specifice care caută prima sau ultima apariție a unui
anumit caracter în șir, precum find sau find_last_of, care au complexitate
liniară. Tipul returnat este size_t, dar acesta poate fi folosit și ca int.

```cpp
--8<-- "cppintro/strings/strings07.cpp"
```

## Concluzii

Caracterele și șirurile de caractere sunt foarte folosite în multe aplicații,
fiind des întâlnite în probleme de toate dificultățile, în special la examenele
de bacalaureat și admitere, dar și în multe dintre problemele ce se dau la
olimpiada de informatică, în clasa a X-a și clasa a VIII-a.

Cunoașterea funcțiilor specifice este esențială deoarece foarte multe întrebări
grilă la admitere implică elemente mai dificile din sintaxa lor și orice detaliu
minor poate face diferența.

## Probleme suplimentare

- [Prelucrări elementare pe șiruri de caractere -
  pbinfo](https://www.pbinfo.ro/probleme/categorii/11/Siruri-de-caractere-prelucrari-elementare-pe-siruri-de-caractere)
- [Funcții predefinite cu șiruri de caractere -
  pbinfo](https://www.pbinfo.ro/probleme/categorii/23/Siruri-de-caractere-functii-predefinite-cu-siruri-de-caractere)
- [Probleme diverse -
  pbinfo](https://www.pbinfo.ro/probleme/categorii/84/Siruri-de-caractere-probleme-diverse)
- [Probleme cu stringuri de pe kilonova](https://kilonova.ro/tags/303)

## Resurse suplimentare

- [Clasificarea caracterelor -
  geeksforgeeks](https://www.geeksforgeeks.org/character-classification-c-cctype/)
- [Operații specifice datelor de tip char - CPPI
  Sync](https://cppi.sync.ro/materia/operatii_specifice_datelor_de_tip_char.html)
- [Șiruri de caractere, citire, parcurgeri - CPPI
  Sync](https://cppi.sync.ro/materia/siruri_de_caractere_citire_parcurgeri.html)
- [Șiruri de caractere C++ -
  pbinfo](https://www.pbinfo.ro/articole/19/siruri-de-caractere-in-cpp)
- [Stringuri -
  Infogym](https://events.info.uaic.ro/infogim/2017/lectii/78/783_stringuri.pdf)
- [Siruri de caractere în C++ - infoscience
  3x](http://infoscience.3x.ro/c++/siruridecaractere.htm)
