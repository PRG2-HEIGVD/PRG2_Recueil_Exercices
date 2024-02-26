# Notion de boucles - while

Écrire un programme qui utilise une boucle while pour déterminer et afficher le plus grand nombre d'une série de nombres saisis par l'utilisateur. L'utilisateur saisit des nombres jusqu'à ce qu'il saisisse -1.

## Exemple d'execution

```cpp
Entrez des nombres pour trouver le maximum (-1 pour terminer):
27
42
1
35
7
-1
Le plus grand nombre est: 42
```

<details>
<summary>Solution</summary>

```cpp
#include <stdio.h>

int main() {
    int nombre, max;

    printf("Entrez des nombres pour trouver le maximum (-1 pour terminer): \n");
    scanf("%d", &nombre);

    max = nombre;
    while (nombre != -1) {
        if (nombre > max) {
            max = nombre;
        }
        scanf("%d", &nombre);
    }

    printf("Le plus grand nombre est: %d\n", max);

    return 0;
}

```

</details>

