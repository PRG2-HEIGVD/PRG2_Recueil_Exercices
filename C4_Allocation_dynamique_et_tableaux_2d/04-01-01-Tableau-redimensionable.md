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
doivent y être stoqués dépasse sa capacité actuelle. 

N'oubliez pas de tester si les allocations dynamiques ont fonctionné et
de libérer la mémoire en fin de programme. 


<details>
<summary>Solution</summary>

~~~cpp
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

int main() {
    int n = 0;      // nombre d'elements
    int cap = 1;    // capacite du tableau
    int *tab = (int *)malloc(sizeof(int));

    if (tab == NULL) {
        printf("Erreur d'allocation de memoire\n");
        return 1;
    }

    while (true) {
        printf("Entrez un entier positif (une lettre pour finir): ");
        int i;
        if (scanf("%d", &i) != 1) {
            break;
        }
        if (n == cap) {
            tab = realloc(tab, (cap *= 2) * sizeof(int));
            if (tab == NULL) {
                printf("Erreur d'allocation de memoire\n");
                return 1;
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