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

void somme_tableaux(const int *tab1, const int *tab2, int *resultat) {
    for (int i = 0; i < TAILLE; i++) {
        resultat[i] = tab1[i] + tab2[i];
    }
}

void afficher_tableau(const int *tab) {
    for (int i = 0; i < TAILLE; i++) {
        printf("%d ", tab[i]);
    }
    printf("\n");
}

int main(void) {
    int tab1[TAILLE], tab2[TAILLE], resultat[TAILLE];

    printf("Entrez les éléments du premier tableau : ");
    for (int i = 0; i < TAILLE; i++) {
        scanf("%d", &tab1[i]);
    }

    printf("Entrez les éléments du deuxième tableau : ");
    for (int i = 0; i < TAILLE; i++) {
        scanf("%d", &tab2[i]);
    }

    somme_tableaux(tab1, tab2, resultat);

    printf("Premier tableau : ");
    afficher_tableau(tab1);

    printf("Deuxième tableau : ");
    afficher_tableau(tab2);

    printf("Résultat de la somme : ");
    afficher_tableau(resultat);

   
 
~~~cpp
 
</details>
