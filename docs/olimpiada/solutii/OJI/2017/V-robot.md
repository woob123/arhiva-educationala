---
tags:
    - OJI
    - clasa V
    - cifre
    - simulare
---

# Soluția problemei robot (OJI 2017, clasa a V-a)

!!! example "Cunoștințe necesare"
    - [Prelucrarea cifrelor](../../../../usor/digits-manipulation.md)

**Autor soluție**: Traian Mihai Danciu

!!! note "Link problemă"
    Această problemă poate fi accesată [aici](https://kilonova.ro/problems/880/). 

## Cerința 1

Trebuie doar să numărăm câte caractere A sunt.

## Cerința 2

Putem să simulăm programul, menținând o variabilă $cf$ care să ne spună la ce cifră suntem.

- Când mergem la dreapta cu $P$ poziții, $cf = cf + p$, iar dacă $cf \geq 10$ (de la 9 mergem la 0), $cf = cf - 10$
- Când mergem la stânga cu $P$ poziții, $cf = cf - p$, iar dacă $cf < 0$ (de la 0 mergem la 9), $cf = cf + 10$

## Cerința 3

Vom citi cifrele caracter cu caracter. Noi trebuie să mergem de la cifră la cifră pe cel mai scurt drum posibil, plecând de la cifra 0.

Drumul cel mai scurt posibil de la cifra $a$ la cifra $b$ este:

- Dacă $a < b$, cel mai scurt dintre:
	- În stânga cu $a-b$ poziții
	- În dreapta cu $10+b-a$ poziții
- Dacă $a < b$, cel mai scurt dintre:
	- În dreapta cu $b-a$ poziții
	- În stânga cu $10+a-b$ poziții
- Dacă $a = b$, nu trebuie să facem nimic

Mai jos puteți găsi o soluție neoficială care ia punctajul maxim.

```cpp
#include <fstream>

using namespace std;

ifstream fin("robot.in");
ofstream fout("robot.out");

int main() {
    short cerinta, comenzi = 0, cifra = 0, aux;
    char comanda;
    fin >> cerinta;

    if (cerinta == 1) {
        fin >> comanda;

        while (comanda != 'T') {
            if (comanda == 'A') {
                comenzi++;
            }

            fin >> comanda;
        }

        fout << comenzi;
    } else if (cerinta == 2) {
        fin >> comanda;

        while (comanda != 'T') {
            if (comanda == 'D') {
                fin >> comanda;
                cifra += comanda - '0';

                if (cifra > 9) {
                    cifra -= 10;
                }
            } else if (comanda == 'S') {
                fin >> comanda;
                aux = comanda - '0';

                if (aux > cifra) {
                    aux -= cifra;
                    cifra = 10 - aux;
                } else {
                    cifra -= aux;
                }
            } else {
                fout << cifra;
            }
            fin >> comanda;
        }
    } else {
        while (fin >> comanda) {
            aux = comanda - '0';

            if (cifra > aux) {
                if (cifra - aux <= 10 - cifra + aux) {
                    fout << 'S' << cifra - aux;
                } else {
                    fout << 'D' << 10 - cifra + aux;
                }
            } else if (aux > cifra) {
                if (aux - cifra <= 10 - aux + cifra) {
                    fout << 'D' << aux - cifra;
                } else {
                    fout << 'S' << 10 - aux + cifra;
                }
            }
            cifra = aux;

            fout << 'A';
        }

        fout << 'T';
    }

    return 0;
}
```