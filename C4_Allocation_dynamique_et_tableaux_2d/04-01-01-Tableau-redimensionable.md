# Tableau redimensionable

Ecrivez un programme qui demande à l'utilisateur d'entrer des entiers jusqu'à ce qu'il entre autre chose qu'un entier. A ce moment là, il sort de sa boucle et affiche tous les entiers entrés. 

Un exemple d'exécution : 

~~~
Entrez un entier positif (une lettre pour finir): 4
Entrez un entier positif (une lettre pour finir): 2
Entrez un entier positif (une lettre pour finir): 5
Entrez un entier positif (une lettre pour finir): 7
Entrez un entier positif (une lettre pour finir): 6
Entrez un entier positif (une lettre pour finir): 3
Entrez un entier positif (une lettre pour finir): 4
Entrez un entier positif (une lettre pour finir): q
Vous avez entre: 4 2 5 7 6 3 4 
~~~

Pour le réaliser, utilisez un tableau alloué dynamiquement dont vous 
doublez la capacité avec `realloc` dès que le nombre d'éléments qui
doivent y être stockés dépasse sa capacité actuelle. 

N'oubliez pas de tester si les allocations dynamiques ont fonctionné et
de libérer la mémoire en fin de programme. 


<details>
<summary>Solution</summary>

~~~cpp
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

int main() {
    size_t n = 0;      // nombre d'elements
    size_t cap = 1;    // capacite du tableau
    int *old_tab, *tab;

    tab = (int *)malloc(sizeof(int));
    if (tab == NULL) {
        printf("Erreur d'allocation de memoire\n");
        return 1;
    }

    while (true) {
        printf("Entrez un entier positif (une lettre pour finir): ");
        int i;
        if (scanf("%d", &i) != 1) {
            // L'utilisateur n'a pas entré un entier. 
            // Nettoyer le buffer d'entrée et sortir de la boucle
            while (getchar() != '\n')
                ;
            break;
        }
        if (n == cap) {
            // Le tableau est plein, redimensionner
            old_tab = tab;
            tab = realloc(old_tab, cap * 2 * sizeof(int));
            if (tab == NULL) {
                // Mémoire pleine. On conserve l'ancien tableau 
                // et on sort de la boucle
                printf("Mémoire pleine !\n");
                tab = old_tab;
                break;
            } else {
                // Redimensionnement réussi
                cap *= 2;
            }
        }
        tab[n++] = i;
    }

    printf("Vous avez entre: ");
    for (int i = 0; i < n; i++) {
        printf("%d ", tab[i]);
    }
    printf("\n");

    free(tab);
}
~~~

</details>
