---
id: algorithms
author:
    - Ștefan-Iulian Alecu
prerequisites:
    - sorting
    - stl
    - lambda
tags:
    - algoritmi
    - stl
---

Până în acest punct, am învățat câțiva algoritmi, de la
[sortare](../usor/sorting.md), [sume parțiale](../usor/partial-sums.md) și
[căutarea binară](../usor/binary-search.md) la
[maxime și minime](../usor/maxime-minime.md) și
[determinarea frecvenței unui element](../usor/frequency-arrays.md). Aceste
probleme sunt frecvente în programare, iar mulți dezvoltatori ajung să
implementeze de la zero structuri de date precum stive, vectori dinamici, liste
înlănțuite sau string-uri, așa cum este cazul pentru programatorii care lucrează
în C.

Din fericire, creatorii C++ au vrut să evite acest efort redundant, și deci
ofera în biblioteca sa standard un set de biblioteci numite la comun STL
(Standard Template Library). STL este compus din patru concepte:

- Containerele, care sunt obiecte care stochează date. Am dat de câteva astfel
  de containere, precum `vector`, `map` sau `set`. Nu voi reitera descrierea
  acestor containere, pentru că s-a făcut deja în articolele corespunzătoare,
  mai ales în [introducerea în STL](../cppintro/stl.md).
- Iteratorii, care ne permit să traversăm un set de elemente într-o manieră
  uniformă și standardizată.
- Algoritmii, care ne permit să efectuăm operații comune pe orice fel de
  container folosind iteratorii, dacă sunt îndeplinite cerințele algoritmului
  (dacă trebuie să sortezi ceva, elementele trebuie să fie comparabile, de
  pildă).
- Funcțiile, mai ales [funcțiile lambda](../cppintro/lambda.md), pentru a putea
  controla cum funcționează algoritmii. De pildă, dacă vrem să căutăm un element
  par, cu funcții (în mod specific, cu un predicat) avem cum să precizăm acest
  lucru.

Vom recapitula iteratorii, chiar dacă au fost menționați în
[introducerea în STL](../cppintro/stl.md), deoarece aceștia sunt foarte
importanți.

# Iteratorii

Iteratorii ne permit să traversăm un set de elemente conținute în orice este ca
un container (deci nu neapărat din STL, poate fi definit și de noi!) într-o
manieră uniformă. Fie că e vorba de un `vector`, `array` sau `set`, cu
iteratorii este irelevant cum anume traversezi acea colecție de elemente, deci
poți avea algoritmi generici.
