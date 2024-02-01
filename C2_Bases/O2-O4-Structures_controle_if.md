# Structures de contrôle - if

Écrivez un programme pour vérifier si un nombre est divisible par 3 et 13 ou non, en utilisant if-else.
<details>
<summary>Source</summary>
(https://developpement-informatique.com/article/316/exercices-corriges-pour-maitriser-la-structure-de-controle-if-else)
</details>

Exemple
-------
- Données d'entrée: _117_
- Données de sortie: _117 est divisible par 3 et 13_

<details>
<summary>Solution</summary>

~~~cpp

#include <stdio.h>
 
int main() {
    int nb;
 
    /* Fournir les données d'entrée */
    printf("Saisir un nombre: ");
    scanf("%d", &nb);
 
 
    if((nb % 3 == 0) && (nb % 13 == 0))     {
        printf("%d est divisible par 3 et 13",nb);
    } else {
        printf("%d n'est divisible par 3 ni 13",nb);
    }
 
    return 0;
}

~~~
</details>
