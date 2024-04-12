# Alignement des structures

## Exercice 1

Concevez une structure en C pour représenter des données provenant d'un capteur, qui doivent être stockées de manière contiguë en mémoire. Cette structure sera utilisée dans un contexte où l’espace mémoire est limité et précieux.
Les données du capteur sont les suivantes :

- Identifiant du capteur (2 octets)
- Horodatage (estampillage) en secondes
- Type de capteur (1 byte)
- Valeur du capteur (4 octets)

Le programme devra allouer dynamiquement une zone mémoire contiguë réservée où le capteur
pourra venir écrire ces valeurs, puis implémenter une fonction d'affichage.

Faire un test dans la fonction main(), avec les données suivantes :

- Identifiant du capteur :  12345
- Horodatage :              161803398
- Type de capteur :         2
- Valeur du capteur :       3.14159

<details>
<summary>Solution</summary>

~~~

#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>

// Définition de la structure pour les données du capteur
 
typedef struct __attribute__((packed)) {
    uint16_t id;        // Identifiant du capteur (2 octets)
    uint32_t timestamp; // Horodatage en secondes (4 octets)
    uint8_t type;       // Type de capteur (1 octet)
    float value;        // Valeur du capteur (4 octets)
} SensorData;
 
// Fonction pour afficher les données du capteur
void displaySensorData(SensorData *data) {
    printf("Identifiant du capteur : %u\n", data->id);
    printf("Horodatage : %u\n", data->timestamp);
    printf("Type de capteur : %u\n", data->type);
    printf("Valeur du capteur : %.5f\n", data->value);
}

int main() {
    // Allocation dynamique de la mémoire pour les données du capteur
    SensorData *sensorData = (SensorData *) malloc(sizeof(SensorData));
    if (!sensorData) {
        perror("Allocation de mémoire échouée");
        return EXIT_FAILURE;
    }

    // Initialisation des données du capteur
    sensorData->id = 12345;         // Identifiant du capteur
    sensorData->timestamp = 161803398; // Horodatage
    sensorData->type = 2;           // Type de capteur
    sensorData->value = 3.14159f;   // Valeur du capteur

    // Affichage des données du capteur
    displaySensorData(sensorData);

    // Libération de la mémoire allouée
    free(sensorData);

    return EXIT_SUCCESS;
}


~~~

</details>



