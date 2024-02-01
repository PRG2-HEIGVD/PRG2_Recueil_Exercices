# Notion de boucles - for

Écrire un programme en C qui utilise une boucle for pour imprimer les 10 premiers nombres de la table de multiplication de 5.

## Exemple d'execution

```cpp
5 * 1 = 5
5 * 2 = 10
5 * 3 = 15
5 * 4 = 20
5 * 5 = 25
5 * 6 = 30
5 * 7 = 35
5 * 8 = 40
5 * 9 = 45
5 * 10 = 50
```

<details>
<summary>Solution</summary>

```cpp
#include <stdio.h>

int main() {
    for (int i = 1; i <= 10; i++) {
        printf("5 * %d = %d\n", i, 5 * i);
    }
    return 0;
}
```

</details>

