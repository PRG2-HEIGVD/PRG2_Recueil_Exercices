
# Champs de bits

## Etape 1
Écrivez une structure appelée ``Packet`` qui représente un paquet de données réseau avec les champs suivants :

- Un champ pour le numéro de séquence, sur 16 bits.
- Un champ pour le type de paquet, sur 4 bits.
- Un champ pour le statut du paquet, sur 2 bits.
- Un champ pour la longueur des données, sur 10 bits.

## Etape 2
Ecrivez des fonctions pour :

- Initialiser une variable de type Packet avec les valeurs données pour chaque champ.
- Afficher les informations d'un paquet (numéro de séquence, type, statut, longueur des données).
- Modifier le type d'un paquet donné.
- Modifier le statut d'un paquet donné.
- Calculer la taille en octets d'un paquet, en tenant compte de la taille des champs spécifiés.

<details>
<summary>Solution</summary>

```cpp

#include <stdio.h>

struct Packet {
    unsigned int sequence_number : 16;
    unsigned int packet_type : 4;
    unsigned int packet_status : 2;
    unsigned int data_length : 10;
};

void init_packet(struct Packet *packet, unsigned int seq_num, unsigned int type, unsigned int status, unsigned int length) {
    packet->sequence_number = seq_num;
    packet->packet_type = type;
    packet->packet_status = status;
    packet->data_length = length;
}

void print_packet(struct Packet packet) {
    printf("Numéro de séquence : %u\n", packet.sequence_number);
    printf("Type de paquet : %u\n", packet.packet_type);
    printf("Statut du paquet : %u\n", packet.packet_status);
    printf("Longueur des données : %u\n", packet.data_length);
}

void set_packet_type(struct Packet *packet, unsigned int type) {
    packet->packet_type = type;
}

void set_packet_status(struct Packet *packet, unsigned int status) {
    packet->packet_status = status;
}

size_t packet_size(struct Packet packet) {
    // Calcul de la taille en octets du paquet
    size_t size = sizeof(packet.sequence_number) + sizeof(packet.packet_type) + sizeof(packet.packet_status) + sizeof(packet.data_length);

    return size;
}

int main() {
    struct Packet my_packet;

    init_packet(&my_packet, 1234, 3, 1, 512);

    printf("Informations du paquet initial :\n");
    print_packet(my_packet);
    
    set_packet_type(&my_packet, 2);
    printf("\nType modifié :\n");

    print_packet(my_packet);
    
    set_packet_status(&my_packet, 0);
    printf("\nStatut modifié :\n");
    
    print_packet(my_packet);
    
    printf("\nTaille du paquet : %zu octets\n", packet_size(my_packet));
    
    return 0;
}


```
</details>

