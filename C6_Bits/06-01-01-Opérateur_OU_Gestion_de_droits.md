# Opérateur "OU" : Gestion de droits

Imaginons que chaque bit dans un octet représente un droit d'accès différent pour un fichier — lire, écrire, exécuter, etc.

En définissant un masque pour chaque action, on peut aisément combiner les droits d'accès à l'aide de l'opérateur OU.

Écrivez un programme en langage C qui réalise la gestion des droits d'accès d'un système de fichiers simple.

<details>
<summary>Solution</summary>

```cpp
#include <stdio.h>

// Définition des masques pour chaque droit d'accès
#define LIRE 0x01
#define ECRIRE 0x02
#define EXECUTER 0x04

int main() {
    // Droits d'accès actuels: aucun droit accordé
    unsigned char droitsAcces = 0x00;

    // L'utilisateur obtient le droit de lire et d'écrire
    droitsAcces |= LIRE | ECRIRE;

    printf("Droits d'accès après modification: 0x%X\n", droitsAcces);

    // Vérification des droits
    if (droitsAcces & LIRE) printf("Droit de lire accordé.\n");
    if (droitsAcces & ECRIRE) printf("Droit d'écrire accordé.\n");
    if (droitsAcces & EXECUTER) printf("Droit d'exécuter accordé.\n");

    return 0;
}
```

</details>
