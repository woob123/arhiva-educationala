---
id: intro
author:
  - Ștefan-Cosmin Dăscălescu
  - Ștefan-Iulian Alecu
prerequisites:
  - index
tags:
  - C++
  - introducere
---

!!! note "Observație"

    Dacă nu ți-ai instalat un compilator sau un editor, te invităm să accesezi mai
    întâi [acest articol](./index.md) pentru a putea începe să scrii cod corect 
    și eficient.

În continuare, vom explora câteva aspecte introductive ale limbajului de
programare C++ (sintaxa, compilarea, structura de bază a unui program), însoțite
de un exemplu simplu de program.

## Ce este un limbaj de programare?

Limbajele de programare reprezintă un sistem de notații pe care le folosim
pentru a scrie programe destinate calculatoarelor. Aceste limbaje permit
oamenilor să comunice cu calculatoarele într-o manieră asemănătoare cu limbajul
uman.

Există multe limbaje de programare, care pot fi împărțite în categorii, în
funcție de modul în care le utilizăm și cum interacționează cu procesorul sau
alte criterii.

Unul dintre cele mai populare moduri de a împărți limbajele de programare este
în funcție de cum interacționează cu procesorul. De exemplu, unele limbaje
necesită un compilator care să genereze cod executabil ce poate fi rulat de
procesor (ex: C, C++, Pascal), iar altele se folosesc cu un interpret care
execută codul linie cu linie (ex: Python, JavaScript).

O altă metodă de a clasifica limbajele de programare este în funcție de modul
principal în care operăm cu ele, putând vorbi astfel de limbaje imperative,
funcționale, logice și orientate pe obiecte.

Limbajele de programare pot fi, de asemenea, clasificate în funcție de modul
principal în care sunt folosite:

- **Limbaje imperative**: Definirea unui algoritm pas cu pas, în care fiecare
  comandă execută o acțiune specifică (ex: C, Pascal).
- **Limbaje funcționale**: Se concentrează pe aplicarea unor funcții asupra
  datelor, evitând modificarea lor directă (ex: Haskell, Lisp).
- **Limbaje logice**: Bazate pe reguli și logica propozițională, unde sistemul
  decide ordinea de execuție (ex: Prolog).
- **Limbaje orientate pe obiecte**: Permite structurarea codului în clase ce
  conțin date și funcții pentru a opera pe aceste date (ex: Java, Python).

Unele limbaje combină aceste paradigme, devenind limbaje multiparadigmă. De
exemplu, C++ combină paradigma imperativă cu programarea orientată pe obiecte,
iar Python este un limbaj orientat pe obiecte, dar permite și programarea
imperativă și funcțională.

!!! note "Observație"

    Pe parcursul studiului informaticii, fie că veți studia doar la liceu sau
    veți ajunge să aveți o carieră, mai lungă sau mai scurtă în domeniu, veți
    avea de-a face cu foarte multe limbaje de programare, iar deși arhiva
    noastră se concentrează pe limbajul C++ datorită avantajelor sale în ceea ce
    privește algoritmica, nu există vreun limbaj de programare care să fie în
    totalitate inutil.

## Limbajul de programare C++

Articolele din această arhivă se vor axa pe limbajul C++, iar acest articol va
introduce cititorii în elementele de bază ale acestui limbaj.

Limbajul C++ a fost creat de Bjarne Stroustrup în 1979 ca o extindere a
limbajului C, care fusese dezvoltat între 1969 și 1973 de Dennis Ritchie pentru
a crea sistemul de operare Unix. Astfel, aproape orice program scris în C poate
fi compilat și în C++, de obicei cu modificări minime.

### Primul program în C++

În continuare, vom scrie un program simplu care calculează și afișează valorile
generate de conjectura lui Collatz, preluând un număr natural de la tastatură.

!!! note "Observație"

    Scopul acestui articol este acela de a vă ajuta să fiți capabili să înțelegeți
    și să reproduceți programe similare folosind cunoștințele pe care le veți
    acumula mai jos.

!!! note "Comentarii"

    Pentru a explica diverse părți din cod, vom folosi de-a lungul arhivei
    comentariile, acestea fiind o instrucțiune din limbajul C++. Pentru a pune un
    comentariu pe un singur rând, vom folosi `#!cpp //`, iar pentru a comenta mai
    multe rânduri, vom folosi perechea `#!cpp /* */`.

```cpp
--8<-- "cppintro/intro/intro.cpp"
```

### Elemente de bază ale sintaxei C++

După cum observi, majoritatea liniilor de cod din acest program se încheie cu un
semn de punctuație „;” (punct și virgulă). Acesta semnalează finalul
instrucțiunii respective. Excepție fac liniile în care declarăm biblioteci,
funcții, structuri alternative și repetitive.

De asemenea, folosim acolade pentru a grupa blocuri de cod. Acestea ajută la
îmbunătățirea lizibilității programului și sunt necesare pentru a marca
instrucțiunile subordonate structurilor alternative (de ex., `#!cpp if`) și
repetitive (de ex., `#!cpp while`).

Un alt element foarte important care, deși nu e obligatoriu, este foarte util și
face scrierea codurilor mult mai ușoară, reprezintă spațierea instrucțiunilor.
Chiar dacă vom intra ulterior în detaliu în ceea ce privește coding style-ul,
acesta este un element de bază pentru a scrie coduri cât mai de calitate, obicei
care dacă îl deprindeți de la început, vă va fi mult mai ușor să îl adaptați
oricărui limbaj de programare pe care îl veți folosi.

### Compilarea programului

După finalizarea scrierii codului, compilatorul va verifica corectitudinea
sintactică. Dacă există erori, editorul îți va indica exact unde se află
greșelile.

Atunci când programul este corect, compilatorul va genera un fișier obiect (.o),
pe care îl poți utiliza pentru a crea un executabil (*.exe). După rularea
acestuia, programul va prelua datele de la tastatură, va efectua calculele și va
afișa rezultatele.

!!! note "Observație"

    Pentru a vedea modificările la răspunsurile generate, trebuie să compilați
    și să rulați din nou executabilul, în caz contrar datele vor fi afișate
    conform cu versiunea anterioară a programului. Pare a fi o observație
    banală, însă este o greșeală comună să uiți să recompilezi codul sursă.

### Inițializarea programului

Primul element pe care îl întâlnim este linia `#!cpp #include <iostream>`, care
importă o bibliotecă. Aceste biblioteci sunt colecții de funcții și clase ce ne
permit să reutilizăm cod deja scris, fără a-l regenera. Un echivalent în viața
de zi reprezintă utilizarea unei cărți de rețete pentru prepararea unui fel de
mâncare, astfel noi putem folosi acele metode fără a fi nevoie să le reinventăm.

!!! note "Observație"

    Există și biblioteca `#!cpp <bits/stdc++.h>`, care include toate bibliotecile
    necesare în programul tău. Deși această abordare este recomandată doar la
    competiții de algoritmică (de exemplu, olimpiade), există dezavantaje precum
    un timp mai mare de compilare.

Următoarea linie, `#!cpp using namespace std;` ne permite să folosim direct
toate elementele din spațiul de nume standard (standard namespace). **Un
namespace** (spațiu de nume) este un mecanism de organizare a codului, care face
posibilă gruparea funcțiilor, variabilelor, claselor și altor identificatori sub
un nume comun. Aceasta ajută la evitarea conflictelor între nume, mai ales
atunci când se folosesc biblioteci multiple care ar putea să definească aceleași
identificatori.

!!! note "Observație"

    Aproape toate programele scrise în limbajul C++ vor avea aceste două linii,
    eventual împreună cu alte biblioteci și namespace-uri, detalii pe care le vom
    lăsa ulterior pe măsură ce vă veți obișnui mai bine cu limbajul în sine.

### Funcția `#!cpp main()`

Funcția `#!cpp int main()` este esențială în orice program C++, fiind prima
funcție apelată de compilator.

Ea este cea în care vom scrie toate instrucțiunile, inclusiv așa cum veți vedea
într-un articol ulterior, cele care vor fi folosite pentru a apela funcții
auxiliare.

De asemenea, aici vom avea toate instrucțiunile și structurile de care avem
nevoie pentru a citi și afișa datele, prelucrarea lor, precum și multe alte
facilități specifice, precum manipularea lor folosind structuri logice,
alternative și repetitive.

### Concluzie

Pe parcursul studiilor, prin scrierea mai multor programe, vei deveni mai
familiarizat cu aceste concepte și vei putea să le folosești corect și eficient.
Vom explora detalii suplimentare despre C++ și bunele practici de scriere a
codului în articolele viitoare.

## Resurse suplimentare

- [Sectiunile Elemente de bază ale limbajului C++ si Structuri de control de pe
  pbinfo](https://www.pbinfo.ro/articole/5547/informatica-clasa-a-ix-a)
- [Linkurile de pe w3schools](https://www.w3schools.com/cpp/cpp_getstarted.asp)
- [Learning to code - USACO
  Guide](https://usaco.guide/general/resources-learning-to-code?lang=cpp)

## Probleme suplimentare

- [Probleme usoare si medii din capitolul Operatori și
  expresii](https://www.pbinfo.ro/probleme/categorii/6/elemente-de-baza-ale-limbajului-operatori-si-expresii)
