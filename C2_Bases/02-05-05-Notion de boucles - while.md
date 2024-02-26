# Notion de boucles - while

Écrire un programme en C qui demande à l'utilisateur de saisir des nombres jusqu'à ce qu'il saisisse 0. Après quoi, le programme affiche la somme des nombres saisis.

## Exemple d'execution

```cpp
Entrez des nombres pour additionner (0 pour terminer):
2
4
5
7
1
0
La somme des nombres est: 19
```

<details>
<summary>Solution</summary>

```cpp
#include <stdio.h>

int main() {
    int nombre, somme = 0;

    printf("Entrez des nombres pour additionner (0 pour terminer): \n");
    scanf("%d", &nombre);

    while (nombre != 0) {
        somme += nombre;
        scanf("%d", &nombre);
    }

    printf("La somme des nombres est: %d\n", somme);

    return 0;
}

```

</details>

