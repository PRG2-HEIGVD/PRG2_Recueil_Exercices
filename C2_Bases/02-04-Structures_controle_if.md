# Structures de contrôle - if
<details>
<summary>Source</summary>
(https://developpement-informatique.com/article/316/exercices-corriges-pour-maitriser-la-structure-de-controle-if-else)
</details>

## Exercice 1
Écrivez un programme pour vérifier si un nombre est divisible par 3 et 13 ou non, en utilisant if-else.

Exemple
- Entrée: _117_
- Sorties: _117 est divisible par 3 et 13_

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
        printf("%d n'est pas divisible par 3 et 13",nb);
    }

    return 0;
}

~~~
</details>

## Exercice 2
Écrivez un programme pour trouver un maximum entre trois nombres en utilisant une if-else ou if imbriquée.

Exemple
- Entrées: _17 12 16_
- Sorties: _Le maximum est : 17_

<details>
<summary>Solution</summary>

~~~cpp

#include <stdio.h>

int main()
{
    int num1, num2, num3, max;
 
    /* Fournir les données d'entrée */
    printf("Saisir 3 nombres: ");
    scanf("%d%d%d", &num1, &num2, &num3);
 
    if (num1 > num2)
    {
        if (num1 > num3) {
            /* si num1 > num2 et num1 > num3 */
            max = num1;
        } else {
            /* si num1 > num2 mais num1 > num3 est fausse */
            max = num3;
        }
    } else {
        if (num2 > num3) {
            /* Si num1 < num2 et num2 > num3 */
            max = num2;
        } else {
            /* si num1 < num2 et num2 > num3 */
            max = num3;
        }
    }
     
    /* afficher le résultat */
    printf("le maximum est = %d", max);
 
    return 0;
}

~~~
</details>
