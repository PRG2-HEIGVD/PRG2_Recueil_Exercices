
# Calculatrice

On se propose d'écrire un programme qui utilise un tableau de pointeurs de fonctions pour mapper différentes opérations arithmétiques en fonction de l'entrée de l'utilisateur.

Soit le code C suivant :

```cpp

#include <stdio.h>

int addition(int a, int b) {
    return a + b;
}

int soustraction(int a, int b) {
    return a - b;
}

int multiplication(int a, int b) {
    return a * b;
}

int division(int a, int b) {
    if (b != 0) {
        return a / b;
    } else {
        printf("Erreur : division par zéro\n");
        return 0;
    }
}

```

a) Déclarez un tableau de pointeurs de fonctions qui peut pointer vers les fonctions définies ci-dessus.

b) Implémente un programme principal qui permet à l'utilisateur de choisir une opération et de saisir deux nombres. Utilise le tableau de pointeurs de fonctions pour appeler la fonction appropriée.


<details>
<summary>Solution</summary>

```cpp

#include <stdio.h>

int addition(int a, int b) {
    return a + b;
}

int soustraction(int a, int b) {
    return a - b;
}

int multiplication(int a, int b) {
    return a * b;
}

int division(int a, int b) {
    if (b != 0) {
        return a / b;
    } else {
        printf("Erreur : division par zéro\n");
        return 0;
    }
}

int (*operations[4])(int, int) = {addition, soustraction, multiplication, division};

int main() {
    int choix;
    int a, b, resultat;

    printf("Choisissez une opération :\n");
    printf("0. Addition\n");
    printf("1. Soustraction\n");
    printf("2. Multiplication\n");
    printf("3. Division\n");
    printf("Votre choix : ");
    scanf("%d", &choix);

    if (choix < 0 || choix > 3) {
        printf("Choix invalide\n");
        return 1;
    }

    printf("Entrez deux nombres : ");
    scanf("%d %d", &a, &b);

    resultat = operations[choix](a, b);
    printf("Le résultat est : %d\n", resultat);

    return 0;
}

```

</details>