---
id: OJI-2020-VI-forta
title: Soluția problemei forta (OJI 2020, clasa a VI-a)
problem_id: 921
authors: 
    - stefdasca
prerequisites:
    - sieve
    - divisibility
tags:
    - OJI
    - clasa VI
    - numere prime
    - ciur
---

Pentru a afla forța maximă a unei valori din șir, trebuie să aflăm eficient pentru
fiecare număr câți divizori are. Acest lucru poate fi făcut fie folosind algoritmul
consacrat până la radicalul unui număr, fie folosind o optimizare a ciurului lui
Eratostene, în care aflăm numerele prime până la $45000$ și mai apoi aflăm
descompunerea în factori primi ai acelui număr, folosind formula descrisă în
articolul de mai sus despre divizibilitate.

Deoarece limita de timp este una strânsă, trebuie să avem grijă la implementare
pentru a putea afla eficient numărul de divizori, precum și numărul maxim de valori
cu același număr de divizori.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
--8<-- "olimpiada/VI/2020-forta.cpp"
```
