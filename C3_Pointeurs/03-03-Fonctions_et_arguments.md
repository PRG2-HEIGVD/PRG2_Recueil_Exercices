# Fonctions et arguments


Écrire un programme en langage C qui prend en entrée deux tableaux de 20 entiers, 
calcule leur somme élément par élément et stocke le résultat dans un troisième tableau. 

Ensuite, le programme doit afficher le contenu des trois tableaux.

<details>
<summary>Solution</summary>

~~~cpp

#include <stdio.h>
#include <stdlib.h>

#define TAILLE 5

void somme_tableaux(int taille, const int *tab1, const int *tab2, int *resultat) {
    int i;

    for (i = 0; i < taille; i++) {
        resultat[i] = tab1[i] + tab2[i];
    }
}

void afficher_tableau(int taille, const int *tab) {
    int i;

    for (i = 0; i < taille; i++) {
        printf("%d ", tab[i]);
    }
    printf("\n");
}

int main(void) {
    int tab1[TAILLE], tab2[TAILLE], resultat[TAILLE];
    int i;

    printf("Entrez les éléments du premier tableau : ");
    for (i = 0; i < TAILLE; i++) {
        scanf("%d", &tab1[i]);
    }

    printf("Entrez les éléments du deuxième tableau : ");
    for (i = 0; i < TAILLE; i++) {
        scanf("%d", &tab2[i]);
    }

    somme_tableaux(TAILLE, tab1, tab2, resultat);

    printf("Premier tableau : ");
    afficher_tableau(sizeof tab1 / sizeof(int), tab1);

    printf("Deuxième tableau : ");
    afficher_tableau(sizeof tab2 / sizeof(int), tab2);

    printf("Résultat de la somme : ");
    afficher_tableau(sizeof resultat / sizeof(int), resultat);
 
~~~cpp
 
</details>
