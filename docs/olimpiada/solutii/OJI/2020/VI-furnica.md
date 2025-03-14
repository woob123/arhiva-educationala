---
id: OJI-2020-VI-furnica
title: Soluția problemei furnica (OJI 2020, clasa a VI-a)
problem_id: 922
authors: [stefdasca]
prerequisites:
    - simulating-solution
tags:
    - OJI
    - clasa VI
    - implementare
    - simulare
---


Pentru rezolvarea problemei identificăm pe traseul furnicii porțiunile în care
furnica urcă cu câte un centimetru în fiecare 5 secunde, porțiunile
de pe traseu în care coboară câte un centimetru la fiecare 2 secunde precum
si porțiunile de pe traseu în care se deplasează pe orizontală cu câte un centimetru
la fiecare 3 secunde.

Furnica urcă pe latura verticală a primei foi, apoi parcurge conform
traseului specificat, fiecare foaie.

Pentru a identifica tipul porţiunii (urcare, coborâre, deplasare pe orizontală)
comparăm laturile foii curente $K$, cu laturile foii anterioare $K-1$.

La cerința 1, calculăm timpul necesar furnicii pentru fiecare porțiune
din traseul său, cu ajutorul unei variabile sumative care are următoarea formulă:

$Timp=5*(portiune_{urcare})+2*(portiune_{coborâre})+3*(portiune_{orizontală})$

La cerința 2, identificăm doar porțiunile de pe traseul în care aceasta se
deplasează doar pe orizontală sau pe verticală (în urcare) și calculăm cea mai mare
lungime (exprimată în centimetri) a unei porțiuni continue de pe traseul furnicii
ce respectă condiţiile din cerinţă (furnica NU coboară).

Într-un final, la cerința 3, contorizăm pas cu pas timpul necesar deplasării
furnicii pe traseul determinat de marginile libere ale foilor galbene, până în
momentul în care am ajuns la valoarea T. Afișăm numărul de ordine al foii galbene
pe care se află furnica în acest moment.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
--8<-- "olimpiada/VI/2020-furnica.cpp"
```
