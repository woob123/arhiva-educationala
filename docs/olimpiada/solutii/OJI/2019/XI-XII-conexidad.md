---
id: OJI-2019-XI-XII-conexidad
author:
    - Ștefan-Cosmin Dăscălescu
prerequisites:
    - graphs
    - sorting
tags:
    - OJI
    - clasa XI-XII
    - grafuri
    - sortare
---

# Soluția problemei conexidad (OJI 2019, clasele XI-XII)

!!! note "Link problemă"
    Această problemă poate fi accesată [aici](https://kilonova.ro/problems/20/).

Pentru a rezolva această problemă, ne putem gândi mai întâi să aflăm componentele
conexe, împreună cu dimensiunile lor.

O primă idee pe care o putem folosi este aceea de a conecta componentele conexe
într-un lanț, ceea ce duce să avem $max_{extra} = 2$. Deși aceasta este o soluție
rezonabilă, nu este suficient de bună pentru a obține punctajul maxim, deoarece
putem obține rezultate mai bune folosind un alt algoritm.

Acum, întrebarea se pune cum putem obține $max_{extra} = 1$. Cu alte cuvinte,
vrem să putem conecta componentele conexe fără să folosim un nod de două ori.
Cu alte cuvinte, vrem să avem cât mai multe variante să folosim un nod nou atunci
când avem o altă componentă conexă pe care o folosim.

Acest lucru ne duce cu gândul la o sortare în ordine descrescătoare după dimensiunea
componentelor conexe, iar la un anumit pas, păstrăm nodurile care nu au fost
conectate încă cu o muchie nouă și folosim unul din aceste noduri pentru a conecta
noua componentă conexă, iar mai apoi adăugăm celelalte noduri în lista
noastră.

Deoarece dimensiunea datelor de intrare este mică, complexitatea nu este
foarte relevantă pentru calculele problemei, dar acest algoritm poate fi
îmbunătățit pentru restricții mult mai mari ale datelor de intrare.

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
--8<-- "olimpiada/XI-XII/2019-conexidad.cpp"
```