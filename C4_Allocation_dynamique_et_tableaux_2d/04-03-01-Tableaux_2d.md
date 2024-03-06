# Table de multiplication

Complétez le code ci-dessous pour qu'il stocke la table de multiplication des nombres de 1 à 7 par les nombres de 1 à 6.

~~~cpp
#include <stdio.h>

#define WIDTH 7
#define HEIGHT 6

int main() {
    // déclarer un tableau tab 

    // remplir ce tableau avec la table de multiplication

    // afficher le tableau
}
~~~

Il doit afficher en résultat

~~~
 1  2  3  4  5  6  7 
 2  4  6  8 10 12 14 
 3  6  9 12 15 18 21 
 4  8 12 16 20 24 28 
 5 10 15 20 25 30 35 
 6 12 18 24 30 36 42 
~~~~

Résoudre ce problème de 4 manières distinctes

- En stockant les données dans un tableau 1d de 42 éléments, rempli et affiché en parcourant ce tableau 1d

<details>
<summary>Solution</summary>

~~~cpp
    int tab[HEIGHT * WIDTH];

    for (int i = 0; i < HEIGHT; i++) {
        for (int j = 0; j < WIDTH; j++) {
            tab[i * WIDTH + j] = (i + 1) * (j + 1);
        }
    }

    for (int i = 0; i < HEIGHT; i++) {
        for (int j = 0; j < WIDTH; j++) {
            printf("%2d ", tab[i * WIDTH + j]);
        }
        printf("\n");
    }
~~~

</details><br>

- En stockant les données dans un tableau 2d de 6 lignes et 7 colonnes, rempli et affiché en parcourant ce tableau 2d

<details>
<summary>Solution</summary>

~~~cpp
    int tab[HEIGHT][WIDTH];

    for (int i = 0; i < HEIGHT; i++) {
        for (int j = 0; j < WIDTH; j++) {
            tab[i][j] = (i + 1) * (j + 1);
        }
    }

    for (int i = 0; i < HEIGHT; i++) {
        for (int j = 0; j < WIDTH; j++) {
            printf("%2d ", tab[i][j]);
        }
        printf("\n");
    }
~~~

</details><br>

- En stockant et remplissant en 1d, et en le parcourant en 2d après conversion du tableau dans le type pointeur approprié (sans copier les données)

<details>
<summary>Solution</summary>

~~~cpp
    int tab[HEIGHT * WIDTH];

    for (int i = 0; i < HEIGHT; i++) {
        for (int j = 0; j < WIDTH; j++) {
            tab[i * WIDTH + j] = (i + 1) * (j + 1);
        }
    }

    int(*tab2d)[WIDTH] = (int(*)[WIDTH]) tab;

    for (int i = 0; i < HEIGHT; i++) {
        for (int j = 0; j < WIDTH; j++) {
            printf("%2d ", tab2d[i][j]);
        }
        printf("\n");
    }
~~~

</details><br>

- En stockant et remplissant en 2d, et parcourant en 1d après conversion du tableau dans le type pointeur approprié (sans copier les données)

<details>
<summary>Solution</summary>

~~~cpp
    int tab[HEIGHT][WIDTH];

    for (int i = 0; i < HEIGHT; i++) {
        for (int j = 0; j < WIDTH; j++) {
            tab[i][j] = (i + 1) * (j + 1);
        }
    }

    int *tab1d = (int *)tab;

    for (int i = 0; i < HEIGHT; i++) {
        for (int j = 0; j < WIDTH; j++) {
            printf("%2d ", tab1d[i * WIDTH + j]);
        }
        printf("\n");
    }
~~~

</details>
