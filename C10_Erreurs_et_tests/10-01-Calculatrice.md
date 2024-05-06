# Tests unitaires avec ``CUnit``

## Prérequis
Il faut d'abord installer le *framework* ``CUnit`` à l'aide de la commande suivante :

~~~sh
sudo apt install libcunit1-ncurses-dev
~~~

## Calculatrice en nombres entiers

Soit la fonction ci-dessous qui implémente les fonctions d'une mini-calculatrice
en nombres entiers uniquement.

~~~cpp
#include <math.h>

int calculer(int nombre1, int nombre2, char operateur) {

        switch (operateur) {
        case '+':
                return nombre1 + nombre2;

        case '-':
                return nombre1 - nombre2;

        case '*':
                return nombre1 * nombre2;

        case '/':
                if (nombre2 == 0) {
                        // Gérer l'erreur de division par zéro
                        return -1;
                } else {
                        return nombre1 / nombre2;
                }

        case '^':
                // Puissance non implémentée avec des entiers
                return (int) pow((double) nombre1, (double) nombre2);

        default:
                // Gérer l'opérateur invalide
                return -1;
        }
}
~~~

Prroposez une implémentation de tests unitaires afin de valider le bon fonctionnement
de la calculatrice. Faites un test pour chaque opération (addition, soustraction,
multiplication, puissance).


<p></p>
 
<details>
<summary>Solution</summary>

Bien entendu, il y a plusieurs manière de tester.
Ci-après l'une d'entre elle.

~~~cpp
#include <CUnit/CUCurses.h>
#include <CUnit/CUnit.h>

#include <math.h>

// Fonction à tester
int calculer(int nombre1, int nombre2, char operateur) {

        switch (operateur) {
        case '+':
                return nombre1 + nombre2;

        case '-':
                return nombre1 - nombre2;

        case '*':
                return nombre1 * nombre2;

        case '/':
                if (nombre2 == 0) {
                        // Gérer l'erreur de division par zéro
                        return -1;
                } else {
                        return nombre1 / nombre2;
                }

        case '^':
                // Puissance non implémentée avec des entiers
                return (int) pow((double) nombre1, (double) nombre2);

        default:
                // Gérer l'opérateur invalide
                return -1;
        }
}


void test_addition(void) {
        CU_ASSERT_EQUAL(calculer(2, 2, '+'), 4);
        CU_ASSERT_EQUAL(calculer(-3, 5, '+'), 2);
}

void test_soustraction(void) {
        CU_ASSERT_EQUAL(calculer(5, 2, '-'), 3);
        CU_ASSERT_EQUAL(calculer(-1, 4, '-'), -5);
}

void test_multiplication(void) {
        CU_ASSERT_EQUAL(calculer(3, 4, '*'), 12);
        CU_ASSERT_EQUAL(calculer(-2, 5, '*'), -10);
}

void test_division(void) {
        CU_ASSERT_EQUAL(calculer(8, 2, '/'), 4);
        CU_ASSERT_EQUAL(calculer(-10, 5, '/'), -2);
        CU_ASSERT_EQUAL(calculer(1, 0, '/'), -1); // Division par zéro
}

void test_puissance(void) {
        CU_ASSERT_EQUAL(calculer(2, 3, '^'), 8);
}

int main() {
        CU_pSuite pSuite = NULL;
        float f;

        if (CU_initialize_registry() != CUE_SUCCESS)
                return CU_get_error();

        pSuite = CU_add_suite("Test calculatrice", NULL, NULL);
        if (!pSuite)
                goto out;

        if (!CU_add_test(pSuite, "Addition", test_addition) ||
            !CU_add_test(pSuite, "Soustraction", test_soustraction) ||
            !CU_add_test(pSuite, "Multiplication", test_multiplication) ||
            !CU_add_test(pSuite, "Division", test_division) ||
            !CU_add_test(pSuite, "Puissance", test_puissance))
                goto out;

        CU_curses_run_tests();

out:
   CU_cleanup_registry();

   return CU_get_error();
}


~~~

</details>
