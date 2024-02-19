# Pointeurs en C


Écrivez un programme en langage C qui :

- Déclare une variable nombre de type int et lui attribue une valeur entière de votre choix.
- Déclare un pointeur ptr vers un entier.
- Affecte à ce pointeur l'adresse de la variable nombre.
- Affiche la valeur de nombre à l'aide du pointeur ptr.

Votre programme devrait donc :

- Déclarer une variable entière.
- Déclarer un pointeur vers un entier.
- Affecter l'adresse de la variable entière au pointeur.
Utiliser le pointeur pour accéder à la valeur de la variable entière et l'afficher. 

<details>
<summary>Solution</summary>

~~~cpp
#include <stdio.h>

int main(void) {
    int nombre = 42; /* Déclaration de la variable nombre et initialisation */

    int *ptr; /* Déclaration d'un pointeur vers un entier */

    ptr = &nombre; /* Affectation de l'adresse de nombre au pointeur ptr */

    /* Affichage de la valeur de nombre à l'aide du pointeur ptr */
    printf("La valeur de nombre est : %d\n", *ptr);

    return 0;
}
~~~
 
</details>
