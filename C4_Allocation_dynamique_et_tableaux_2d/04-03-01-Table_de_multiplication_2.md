# Table de multiplication (2)

Modifier le code de l'exercice [04-02-01](04-02-01-Table_de_multiplication.md) pour 
qu'il permette de créer une table de multiplication de taille quelconque.

Le table de multiplication doit être stockée dans un tableau 2d de type `int **` 
dont chaque ligne est allouée indépendamment des autres. Prenez soin de garantir 
l'absence de fuites mémoire. 

Exemple d'exécution : 

~~~
Entrez le nombre de lignes: 5
Entrez le nombre de colonnes: douze
Erreur de saisie. Entrez un entier positif : -1 
Erreur de saisie. Entrez un entier positif : 12
 1  2  3  4  5  6  7  8  9 10 11 12 
 2  4  6  8 10 12 14 16 18 20 22 24 
 3  6  9 12 15 18 21 24 27 30 33 36 
 4  8 12 16 20 24 28 32 36 40 44 48 
 5 10 15 20 25 30 35 40 45 50 55 60 
~~~~

<details>
<summary>Solution</summary>

~~~cpp
#include <stdio.h>    // printf, scanf
#include <stddef.h>   // size_t
#include <stdlib.h>   // calloc, free, malloc
#include <stdint.h>   // int64_t 
#include <inttypes.h> // SCNd64

size_t lire_size_t(const char *message) {
    // lecture avec vérification d'un entier de type size_t 
   
    int64_t n;
    printf("%s", message);
    while (scanf("%" SCNd64, &n) != 1 || n <= 0) {
        printf("Erreur de saisie. Entrez un entier positif : ");
        while (getchar() != '\n')
            ;
    }
    return (size_t)n;
}

int main() {
    size_t lignes;
    size_t colonnes;
    int **tab;

    lignes = lire_size_t("Entrez le nombre de lignes: ");
    colonnes = lire_size_t("Entrez le nombre de colonnes: ");

    // allocation du tableau de tableaux
    tab = (int **)calloc(lignes, sizeof(int *));
    if (tab == NULL) {
        printf("Erreur d'allocation de memoire\n");
        return 1;
    }

    // allocation des tableaux pour chaque ligne
    for (size_t i = 0; i < lignes; i++) {
        tab[i] = (int *)malloc(colonnes * sizeof(int));
        if (tab[i] == NULL) {
            printf("Erreur d'allocation de memoire\n");
            goto liberation;
        }
    }

    // remplissage avec les valeurs de la table de multiplication
    for (size_t i = 0; i < lignes; i++) {
        for (size_t j = 0; j < colonnes; j++) {
            tab[i][j] = (i + 1) * (j + 1);
        }
    }

    // affichage
    for (size_t i = 0; i < lignes; i++) {
        for (size_t j = 0; j < colonnes; j++) {
            printf("%2d ", tab[i][j]);
        }
        printf("\n");
    }

liberation:
    for (size_t i = 0; i < lignes; i++) {
        free(tab[i]);
    }
    free(tab);
}  
~~~

</details>