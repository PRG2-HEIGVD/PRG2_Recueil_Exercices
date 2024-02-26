# Notion de boucles - do..while

Écrire un programme en C qui utilise une boucle do..while pour inverser les chiffres d'un nombre entier saisi par l'utilisateur. Par exemple, si l'utilisateur saisit 123, le programme affiche 321.

## Exemple d'execution

```cpp
Entrez un nombre pour inverser ses chiffres: 42987123
Le nombre inversé est: 32178924
```

<details>
<summary>Solution</summary>

```cpp
#include <stdio.h>

int main() {
    int nombre, inverse = 0;

    printf("Entrez un nombre pour inverser ses chiffres: ");
    scanf("%d", &nombre);
    do {
        inverse = inverse * 10 + nombre % 10;
        nombre /= 10;
    } while (nombre > 0);

    printf("Le nombre inversé est: %d\n", inverse);

    return 0;
}
```

</details>

