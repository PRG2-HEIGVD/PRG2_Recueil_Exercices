# Notion de boucles - do..while

Écrire un programme qui demande à l'utilisateur de saisir son âge. Si l'âge est inférieur à 18, le programme demande à nouveau l'âge. Ce processus continue jusqu'à ce que l'utilisateur saisisse un âge valide (18 ou plus), après quoi le programme imprime "Accès accordé".

## Exemple d'execution

```cpp
Entrez votre âge: 14
Entrez votre âge: 17
Entrez votre âge: 8
Entrez votre âge: 17
Entrez votre âge: 10
Entrez votre âge: 42
Accès accordé.
```

<details>
<summary>Solution</summary>

```cpp
#include <stdio.h>

int main() {
    int age;
    do {
        printf("Entrez votre âge: ");
        scanf("%d", &age);
    } while (age < 18);
    printf("Accès accordé.\n");
    return 0;
}
```

</details>

