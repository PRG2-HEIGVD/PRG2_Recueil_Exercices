# Simulation d'une Guirlande de Noël Clignotante

Écrire un programme en C qui simule l'affichage d'une guirlande de Noël clignotante à l'aide de caractères sur la console. Le programme doit utiliser les opérations binaires.

Votre programme doit simuler un affichage de guirlande de Noël composée de 8 lumières sur une seule ligne. Chaque lumière peut être dans l'un des deux états suivants : allumée ou éteinte. Pour représenter visuellement cet état sur la console, vous utiliserez le caractère `*` pour une lumière allumée et le caractère `-` pour une lumière éteinte.

- Au démarrage du programme, les lumières de la guirlande doivent être initialisées dans une configuration spécifique : alternance des lumières allumées et éteintes, soit `*-*-*-*-`.

- Ensuite, le programme doit faire clignoter **toutes** les lumières de la guirlande CLIGNOTEMENTS fois.

- Entre chaque itération de clignotement, introduisez un délai d'attente aléatoire compris entre 0.1 et 1 seconde. Ce délai doit être différent à chaque itération pour simuler un comportement de clignotement irrégulier de la guirlande. Pour le délai, vous pouvez utiliser la fonction `usleep` avec des valeurs en microsecondes (entête `<unistd.h>`).

- Le programme doit mettre à jour l'affichage de la guirlande en ligne sans ajouter de nouvelles lignes à chaque itération, pour donner l'impression que la guirlande clignote sur place. Vous pouvez faire ceci avec `printf("\r");`. Puisqu'il n'y a pas de saut de ligne, un flush du buffer de sortie est aussi nécessaire : `fflush(stdout);`.

- Assurez-vous d'initialiser le générateur de nombres aléatoires avec `srand(time(NULL))` au début de votre programme pour que les délais générés soient réellement aléatoires.

<details>
<summary>Solution</summary>

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <unistd.h> // Pour usleep

#define NMAX 100
#define CLIGNOTEMENTS 20

void afficherLEDs(unsigned char leds) {
    printf("\r"); // Déplace le curseur au début de la ligne
    for (int i = 7; i >= 0; i--) {
        printf("%c", (leds & (1 << i)) ? '*' : '-');
    }
    fflush(stdout); // Vide le tampon de sortie pour afficher immédiatement les LEDs
}

int main() {
    // Initialisation du générateur de nombres aléatoires
    srand(time(NULL));

    unsigned char leds = 0xAA; // Les LEDs *-*-*-*- sont allumées au début

    for (int i = 0; i < CLIGNOTEMENTS; i++) {
        afficherLEDs(leds);
        leds = ~leds; // Inverse l'état des LEDs pour simuler le clignotement

        float delai = (rand() % 900 + 100) / 1000.0; // Génère un délai aléatoire entre 0.1 et 1 seconde

        usleep(delai * 1000000); // Attend le délai aléatoire
    }

    printf("\n");

    return 0;
}
```

</details>
