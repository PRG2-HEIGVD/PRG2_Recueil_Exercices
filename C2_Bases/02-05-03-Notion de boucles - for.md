# Notion de boucles - for

Écrire un programme en ``C`` qui utilise une boucle ``for`` pour calculer la somme des 100 premiers nombres naturels ``(1+2+3+4+...+100)`.

## Exemple d'execution

```cpp
La somme est: 5050
```

<details>
<summary>Solution</summary>

```cpp
#include <stdio.h>

int main() {
    int somme = 0;
    int i;

    for (i = 1; i <= 100; i++) {
        somme += i;
    }
    printf("La somme est: %d\n", somme);
    return 0;
}
```

</details>

