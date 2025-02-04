---
id: problemsetting
author:
    - Ștefan-Cosmin Dăscălescu
prerequisites:
    - debugging
tags:
    - meta
    - sfaturi
    - comisie
    - olimpiada
---

## Introducere

Ai rezolvat foarte multe probleme și poate ți-ai pus întrebarea: "Oare nu
cumva pot și eu să creez o problemă la rândul meu?" Răspunsul este afirmativ,
oricine poate crea o problemă nouă, dar așa cum se întâmplă și atunci când ne
gândim la idei pentru rezolvarea problemelor, nu toate ideile sunt bune (a se
citi originale), iar de asemenea, vrem de preferat să evităm situația în care
o problemă să se reducă la altă problemă, deja dată la un concurs anterior.

În acest articol voi discuta câteva aspecte pe care le puteți avea în vedere
atunci când creați probleme noi, astfel încât acestea să ajungă să fie date
fie la concursuri de informatică, fie de ce nu să le folosiți într-un mod mai
educațional pentru diferite proiecte (inclusiv arhiva noastră!).

## Crearea ideilor pentru probleme

Deși aceasta este cea mai grea parte a propunerii unei probleme, este și cea mai
interesantă și plăcută, deoarece deși nu multe idei sunt complet originale,
există foarte moduri de a ajunge la idei valoroase care să fie folosite în
competiții. De remarcat este faptul că acest proces trebuie să fie cât mai
natural, iar metodele artificiale trebuie evitate cât de mult posibil.

Câteva dintre cele mai frecvent întâlnite surse de inspirație pentru crearea
unor probleme noi de algoritmică sunt:

- imaginarea unui anumit proces (real sau imaginar) și adaptarea lui la un model
informatic. Cu alte cuvinte, se poate întâmpla să vedeți ceva interesant în
viața reală sau într-un joc video și pe baza acestei secvențe de evenimente,
să creați o poveste care să vă ducă la o idee de problemă. Aceasta este una
dintre cele mai naturale moduri în care o problemă ajunge să fie creată, iar
multe dintre problemele foarte bune și apreciate de concurenți sunt create așa.
- rezolvarea unor probleme anterioare și dezvoltarea unor versiuni mai grele sau
diferite față de cele originale, care uneori au chiar soluții complet diferite.
În multe situații, deoarece rezolvați multe probleme de algoritmică, ajungeți să
vă gândiți la multe idei și inevitabil, vă gândiți și la alte abordări care
chiar dacă nu sunt strict legate de problema pe care încercați să o rezolvați,
vă pot duce la idei alternative care în unele situații, pot deveni chiar o
problemă complet diferită. Această abordare este folosită foarte des de cei care
sunt experimentați și găsesc foarte ușor variații noi la problemele deja
existente.
- crearea unei probleme pe baza unei povești sau idei întipărite în minte (de
exemplu, gândirea la o problemă de dinamică care are legătură cu un anume joc).
Deși această metodă ne ajută să creăm probleme care pot deveni bune, în multe
situații problemele create prin această metodă nu sunt la fel de valoroase ca
cele create prin primele două metode, dar cu rafinare și eventual mici (sau mai
mari) ajustări, se pot obține idei frumoase.

Chiar dacă în aceste paragrafe am menționat noțiunea de problemă originală
și cea de problemă interesantă, concursurile au nevoie de multe ori și de
probleme care să testeze într-o anumită măsură cunoștințe precum cea de
implementare sau cele de casework, iar de aceea poate veți vedea mai ales în
cadrul problemelor mai ușoare, idei care deși par standard sau repetitive pentru
concurenții experimentați, sunt foarte utile pentru audiența țintă a problemelor
respective. Acest lucru se întâmplă la rundele de Codeforces cu probleme ușoare
sau la competiții precum IIOT, unde comisia trebuie să creeze un set de probleme
cu o gamă de dificultate foarte largă.

Înainte de a propune o idee pentru un concurs, este indicat să verificați dacă
s-a mai dat aceeași problemă anterior la un alt concurs, în special la o
competiție recentă. Pentru verificarea într-o bază de date internațională,
există siteul [yuantiji](http://yuantiji.ac/en/) unde se poate pune cerința
problemei în engleză și îți potrivește enunțul cu posibile probleme asemănătoare.

Apoi, poți trimite problema unui coordonator (de preferat, cu un enunț scurt,
1-2 exemple și o idee a soluției) sau unor prieteni și poate ajunge chiar în
următorul concurs de profil, fie că este olimpiadă, concurs online sau alt proiect.

## Crearea unui enunț clar

Să presupunem că problema a fost selectată de unul sau mai mulți coordonatori și
trebuie să fie pregătită. Primul pas în pregătirea problemei constă în crearea
unui enunț cât mai clar (de preferat în format $\LaTeX$), care să conțină toate
detaliile necesare pentru toți cititorii, precum restricții, precizări,
eventuale punctaje parțiale și exemple clare și cuprinzătoare în limita
preferinței propunătorilor. De asemenea, este indicat ca exemplele să vină cu
explicații care să ușureze înțelegerea lor.

Un enunț ideal este destul de scurt, la obiect și cu o poveste minimă, dar care
să nu fie intruziv sau artificial adăugată. Povestea poate fi sărită dacă nu se
dorește acest lucru, dar de multe ori, un anumit personaj din probleme poate
deveni celebru în acest fel (de exemplu, foarte multe probleme au un Gigel în
cerința lor).

## Crearea testelor

Următorul pas constă în crearea unui set de date de intrare cât mai cuprinzătoare,
care să testeze sursele concurenților cât mai bine. Acest lucru se face creând
cât mai multe teste care să respecte cât mai multe tipuri de restricții și de
particularități.

În primul rând, trebuie să aveți teste cu dimensiunile datelor cât mai diverse,
începând de la cele mai mici seturi posibile (teste cu valori ale lui $N$
foarte mici și alte valori mici), până la teste cu valori maxime pentru datele
de intrare.

De asemenea, în funcție de tipul de probleme de care vorbim, ne putem gândi la
cazuri particulare pe care trebuie să le includem neapărat în testele de
evaluare, printre altele voi enumera următoarele:

- teste cu $N = 1$ și $N = 2$ (cu alte cuvinte, teste cu $N$ minim).
- teste cu valori foarte mici (vă pot ajuta să detectați eventualele greșeli
în soluția voastră în procesul de pregătire).
- după caz, teste care să aibă o structură particulară (multe valori distincte,
puține valori distincte, seturi cu multe valori egale etc.)
- dacă vorbim de probleme de grafuri, teste cu grafuri complete, arbori,
lanțuri, grafuri de tip stea (un nod din care se crează mai multe lanțuri),
grafuri aciclice etc.
- dacă vorbim de probleme de arbori, teste care să includă arbori binari, arbori
lanț, arbori stea, arbori cu o înălțime mare etc.
- dacă vorbim de probleme cu divizori și/sau teoria numerelor, vrem să avem
numere prime, numere cu mulți divizori, numere cu mulți factori primi etc.
- în cazul problemelor cu structuri de date, vrem să avem queryuri cât mai variate,
care să aibă lungimi mari, lungimi mici, lungimi variate, care să acopere cât
mai multe tipuri de restricții.
- dacă avem de-a face cu corner cases, acestea trebuie incluse în întregime.

Nu în ultimul rând, dacă aveți posibilitatea și tipul problemei vă permite
acest lucru, nu ezitați să faceți problema una de tip multi-test (cu un
parametru $t$ la începutul datelor de intrare pentru a putea include un
număr mare de teste, în special cu date mici, în cadrul unui singur fișier
test).

## Generatorul de teste

Pentru a genera teste, recomandăm folosirea toolurilor din Polygon, fiind cel
mai inovativ mod de a genera teste la ora actuală.

!!! warning "Tutorial Polygon"
    Recomandăm citirea [acestui blog](https://kilonova.ro/posts/polygon-guide-415-bot7ke)
    pentru o introducere scurtă, sau a
    [acestuia](https://quangloc99.github.io/posts/polygon-codeforces-tutorial/)
    pentru ceva mai cuprinzător.

[Polygon](https://polygon.codeforces.com/) este o platformă dezvoltată de
Mike Mirzayanov, creatorul [Codeforces](https://codeforces.com/) și
este unul dintre cele mai importante medii folosite în zilele noastre
pentru dezvoltarea problemelor date la concursurile de programare competitivă
din întreaga lume. Această platformă face acest proces mult mai puțin anevoios,
iar de asemenea permite și o adaptare ușoară la cerințele altor interfațe de
concurs (CMS / DomJudge etc.).

Pe lângă platformă în sine, există și biblioteca testlib.h care reprezintă
resursa folosită cel mai frecvent de cei care lucrează în Polygon pentru
crearea problemelor, bibliotecă care ușurează foarte mult procesul de generat
date de intrare și generat teste, folosind interfața Polygon.

Pentru a apela un generator numit gen, trebuie să folosim instrucțiuni de tipul
$gen \ par_1 \ par_2 \dots \ par_k$ > $.

Pentru codul de mai jos unde se generează un șir de lungime $n$, putem să dăm
ca parametri în secțiunea tests următoarele date.

```cpp
--8<-- "dificil/problemsetting/generator.cpp"
```

## Checkere și validatoare

Polygon pune la dispoziție o gamă largă de checkere utlizatorilor, acestea
având și descrieri detaliate. De asemenea, se pot scrie și checkere manual,
având grijă să se respecte diferite particularități ale tipului de soluție
cerut. Ideea de bază este aceea de a se verifica dacă răspunsul dat de
utilizator respectă toate restricțiile problemei și dacă este corect pentru
testul dat.

Checkerele puse la dispoziție de ei sunt următoarele:

- fcmp.cpp - verifică liniile, nu ignoră spațiile goale
- hcmp.cpp - potrivit pentru numere mari
- lcmp.cpp - verifică liniile, ignoră spațiile goale
- ncmp.cpp - unul sau mai multe numere de tip long long, ignoră spațiile goale
- nyesno.cpp - zero sau mai multe YES/NO, nu ține cont de litere mari sau mici
- rcmp4.cpp - unul sau mai multe numere reale, eroarea maximă $10^{-4}$.
- rcmp6.cpp - unul sau mai multe numere reale, eroarea maximă $10^{-6}$.
- rcmp9.cpp - unul sau mai multe numere reale, eroarea maximă $10^{-9}$.
- wcmp.cpp - secvență de tokens
- yesno.cpp - un singur YES/NO, nu ține cont de litere mari sau mici

Cele mai des folosite checkere sunt lcmp, nyesno și rcmp9. Celelalte checkere
sunt și ele utile după caz.

Totuși, în anumite situații avem nevoie să scriem niște checkere mai specifice,
pentru a verifica corectitudinea răspunsurilor date de participanți. Pentru
a face asta, trebuie să folosim un set de parametri specifici, care să returneze
-1 dacă avem un răspuns greșit.

Mai jos aveți un exemplu de checker specific pentru codeforces. De asemenea,
puteți consulta [aici](https://kilonova.ro/posts/problem-guide#checker) și
exemple specifice pentru kilonova.

```cpp
--8<-- "dificil/problemsetting/checker.cpp"
```

Deși testlib.h nu dă erori în ceea ce privește generarea datelor, scrierea unui
validator este o idee foarte bună deoarece se previn erori nedorite la generarea
datelor și posibile bug-uri la scrierea parametrilor pentru generarea valorilor
prezente în datele de intrare.

Pentru mai multe informatii, se poate accesa [acest blog](https://codeforces.com/blog/entry/18426).

## Testarea soluțiilor și încărcarea testelor

Aceasta se poate face în multe moduri, fie folosind infrastructura polygon, fie
infrastructura platformei de evaluare, și este important ca de fiecare dată când
găsiți o soluție care ar trebui să obțină un anumit număr de puncte dar obține
mai multe puncte (sau trece testele când nu ar trebui să treacă), atunci
înseamnă că trebuie îmbunătățite testele.

## Concluzii

Crearea unei probleme este un proces care deși pare complicat, este unul foarte
frumos și plin de satisfacție. De asemenea, propusul de probleme te poate ajuta
să găsești cazuri particulare și să-ți îmbunătățești abilitățile de depanare
a soluțiilor. Nu în ultimul rând, poți ajunge să propui probleme la diferite
concursuri sau poate chiar la olimpiadele de informatică, ceea ce poate fi o
experiență frumoasă în sine. Te așteptăm și pe tine să propui probleme, poate
chiar la concursurile marca RoAlgo.

## Resurse suplimentare

- [Problem guide - kilonova](https://kilonova.ro/posts/problem-guide)
- [Polygon Guide - Stefan Dascalescu](https://kilonova.ro/posts/polygon-guide-415-bot7ke)
- [Polygon.Codeforces Tutorial - A Guide to Problem Preparation [Part 1]](https://quangloc99.github.io/posts/polygon-codeforces-tutorial/)
- [Sectiunea testlib - codeforces](https://codeforces.com/testlib)
- [On problemsetting - codeforces](https://codeforces.com/blog/entry/70178)
- [Problemsetting - oiwiki](https://oi-wiki.org/contest/problemsetting/)