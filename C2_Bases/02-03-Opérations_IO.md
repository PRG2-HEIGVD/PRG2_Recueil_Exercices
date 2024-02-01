# Opérations I/O

Écrivez un programme qui demande à l'utilisateur son prénom et son âge, 
puis affiche un message de bienvenue avec ces informations.

Indication
----------
Déclarez les nombres dans des variables locales et stockez le résultat dans une variable globale.

- Addition de deux nombres entiers.
- Soustraction de deux nombres réels.
- Multiplication de deux nombres entiers.
- Division de deux nombres réels.

<details>
<summary>Solution</summary>

~~~cpp

#include <stdio.h>

int main(void) {
    char prenom[50];
    int age;

    printf("Entrez votre prénom: ");
    scanf("%s", prenom);

    printf("Entrez votre âge: ");
    scanf("%d", &age);

    printf("Bienvenue, %s! Vous avez %d ans.\n", prenom, age);

    return 0;
}

~~~
</details>
