---
id: OJI-2025-V-palindrom
title: Soluția problemei palindrom (OJI 2025, clasa a V-a)
problem_id: 3635
authors: [spatarel]
prerequisites:
    - digits-manipulation
tags:
    - OJI
    - clasa V
---

## Cerința 1

În continuare vom descrie rezolvarea pentru un singur număr. Pentru rezolvarea
problemei se aplică algoritmul de mai jos în mod repetat, de $N$ ori.

Pentru rezolvarea primei cerințe vom elimina simultan prima și ultima cifră a
numărului cât timp acestea coincid și numărul are cel puțin două cifre. La
final, dacă numărul rămas este 0 sau are o cifră, atunci putem afirma că numărul
inițial era palindrom.

## Cerința 2

Pentru rezolvarea celei de-a doua cerințe, observăm că operația de inserare a
unei cifre în numărul dat și verificarea ulterioară a proprietă ții de palindrom
este echivalentă cu operația de ștergere a unei cifre din numărul dat și
verificarea proprietă ții de palindrom a numărului astfel obținut.

În plus, observăm că în orice număr palindrom putem insera o cifră (în mijlocul
său) astfel încât numărul nou obținut să fie tot palindrom.

Deci, orice număr care respectă prima cerință o respectă și pe a doua.

Pentru a determina eficient care cifră ar trebui ștearsă, vom efectua următorul algoritm:

1. vom elimina simultan prima și ultima cifră a numărului cât timp acestea
   coincid și numărul are cel puțin două cifre;
2. dacă numărul rămas este 0 sau are o cifră, atunci putem spune că răspunsul la
   cerință este afirmativ;
3. altfel vom investiga două cazuri:
    - dacă eliminăm prima cifră;
    - dacă eliminăm ultima cifră;
4. în ambele cazuri, după eliminarea cifrei corespunzătoare, vom repeta pașii
   (1) și (2);
5. dacă la final numărul rămas are mai mult de o cifră, atunci răspunsul la
   cerință este negativ;

## Cerința 3

Pentru rezolvarea celei de-a treia cerințe, observăm că operația de inserare a
două cifre în numărul dat și verificarea ulterioară a proprietății de
palindromicitate este echivalentă cu operația de ștergere a două cifre din
numărul dat și verificarea proprietății de palindromicitate a numărului astfel
obținut. În plus, observăm că în orice număr palindrom putem insera o cifră (în
mijlocul său) astfel încât numărul nou obținut să fie tot palindrom.

Deci, orice număr care respectă a doua cerință o respectă și pe a treia. Pentru
a determina eficient care cifre ar trebui șterse, vom efectua următorul
algoritm:

- pentru ștergerea primei cifre, vom efectua pașii (1), (2) și (3);
- pentru ștergerea celei de-a doua cifre, vom efectua din nou pașii (1), (2) și
  (3);
- pentru determinarea răspunsului, vom efectua iarăși pașii (1), (2) și (5).

Observați că, efectuând de două ori pasul (3), vom ajunge să investigăm 4 scenarii:

1. la prima nepotrivire eliminăm prima cifră iar la a doua nepotrivire eliminăm
   din nou prima cifră;
2. la prima nepotrivire eliminăm prima cifră iar la a doua nepotrivire eliminăm
   a doua cifră;
3. la prima nepotrivire eliminăm a doua cifră iar la a seconda nepotrivire
   eliminăm prima cifră;
4. la prima nepotrivire eliminăm a doua cifră iar la a doua nepotrivire eliminăm
   din nou a doua cifră.

Pentru a evita cazul particular în care nu avem voie să adăugăm cifra 0 la
începutul numărului inițial, atunci când eliminăm ultima cifră trebuie să
verificăm și să ignorăm cazul în care numărul rămas pe care lucrăm este
numărul original și ultima sa cifră este 0.

Complexitatea timp: $\mathcal{O}(N \cdot X)$

## Rezolvare

Mai jos puteți găsi o soluție care ia punctajul maxim.

```cpp
#include <fstream>

int main() {
    std::ifstream fisier_in("palindrom.in");
    std::ofstream fisier_out("palindrom.out");
    int C, N;
    fisier_in >> C >> N;
    int raspuns = 0;
    for (int i = 0; i < N; i++) {
        int nr;
        fisier_in >> nr;
        bool este_bun = false;
        int nr_original = nr;
        // Calculez puterea lui 10 care are la fel de multe cifre ca și numărul
        // citit. Mă va ajuta să aflu care este prima cifră a numărului și
        // eventual să o elimin.
        int pow10 = 1;
        while (nr / pow10 > 9) {
            pow10 *= 10;
        }
        // Cât timp numărul are cel puțin două cifre iar
        // prima și ultima cifră a numărului coincid, le elimin.
        while (pow10 > 1 && nr / pow10 == nr % 10) {
            nr %= pow10;
            nr /= 10;
            pow10 /= 100;
        }
        if (pow10 <= 1) {
            // Dacă numărul rămas este 0 sau are o singură cifră, atunci
            // indiferent de cerința pe care trebuie să o rezolv, răspunsul este
            // afirmativ.
            este_bun = true;
        } else if (C >= 2) {
            // Dacă cerința este 2 sau 3, trebuie să investighez două scenarii:
            // (dacă op1 == 1) din numărul rămas voi elimina prima cifră;
            // (dacă op1 == 2) din numărul rămas voi elimina ultima cifră.
            for (int op1 = 1; op1 <= 2; op1++) {
                // Voi lucra pe o copie a numărului.
                int copie1_nr = nr;
                if (op1 == 1) {
                    copie1_nr %= pow10;
                } else {
                    // Dacă încerc să elimin ultima cifră din numărul original
                    // și aceasta este 0, atunci înseamnă că ceea ce fac este
                    // echivalent cu a insera o cifră de 0 în fața numărului
                    // original, ceea ce nu este permis.
                    if (copie1_nr == nr_original && copie1_nr % 10 == 0) {
                        continue;
                    }
                    copie1_nr /= 10;
                }
                int copie1_pow10 = pow10 / 10;
                // Cât timp numărul are cel puțin două cifre iar
                // prima și ultima cifră a numărului coincid, le elimin.
                while (copie1_pow10 > 1
                       && copie1_nr / copie1_pow10 == copie1_nr % 10) {
                    copie1_nr %= copie1_pow10;
                    copie1_nr /= 10;
                    copie1_pow10 /= 100;
                }
                if (copie1_pow10 <= 1) {
                    // Dacă numărul rămas este 0 sau are o singură cifră, atunci
                    // indiferent de cerința pe care trebuie să o rezolv,
                    // răspunsul este afirmativ.
                    este_bun = true;
                } else if (C == 3) {
                    // Dacă cerința este 3, trebuie să investighez două
                    // scenarii: (dacă op2 == 1) din numărul rămas voi elimina
                    // prima cifră; (dacă op2 == 2) din numărul rămas voi
                    // elimina ultima cifră.
                    for (int op2 = 1; op2 <= 2; op2++) {
                        // Voi lucra pe o a doua copie a numărului.
                        int copie2_nr = copie1_nr;
                        if (op2 == 1) {
                            copie2_nr %= copie1_pow10;
                        } else {
                            copie2_nr /= 10;
                        }
                        int copie2_pow10 = copie1_pow10 / 10;
                        // Cât timp numărul are cel puțin două cifre iar
                        // prima și ultima cifră a numărului coincid, le elimin.
                        while (copie2_pow10 > 1
                               && copie2_nr / copie2_pow10 == copie2_nr % 10) {
                            copie2_nr %= copie2_pow10;
                            copie2_nr /= 10;
                            copie2_pow10 /= 100;
                        }
                        // Trebuie să rezolv cerința 2 și verific dacă am rămas
                        // cu un număr de o singură cifră sau cu 0.
                        if (copie2_pow10 <= 1) {
                            este_bun = true;
                        }
                    }
                }
            }
        }
        if (este_bun) {
            raspuns++;
            // fisier_out << nr_original;
        }
    }
    fisier_out << raspuns;
    return 0;
}
```
