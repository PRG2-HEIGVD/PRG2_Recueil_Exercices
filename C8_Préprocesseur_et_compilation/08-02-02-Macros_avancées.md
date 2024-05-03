# Macros en C

## Exercice 1

### Décodage d'une macro

Expliquez ce que fait la macro ci-dessous :

#define DEFINE_TAB(name, type)                                                 \
                                                                               \
        struct name##_item {                                                   \
                int pos;                                                       \
                type val;                                                      \
        };                                                                     \
                                                                               \
        struct name##_item tab_##name[10];                                     \
                                                                               \
        static inline type get_##name##_val(int pos) {                         \
                return tab_##name[pos].val;                                    \
        }

<details>
<summary>Solution</summary>

Cette macro définie une structure composée de deux champs dont l'un des champs
est de type ``type`` (second argument de la macro). 

Puis, un tableau 10 éléments de type correspondant à cette
structure est déclarée, ainsi qu'un *accesseur* pour récupérer la valeur d'une
entrée à la position ``pos`` ; il s'agit de la fonction get_*name*_val(int pos) 
où *name* correspondant au premier argument de la macro.

</details>

## Exercice 2

### Utilisation de la macro DEFINE_TAB

Expliquez ce que fait la macro ci-dessous :

Codez un exemple d'utilisation de la macro DEFINE_TAB
pour des entiers et des chaînes de caractères.

<details>
<summary>Solution</summary>

~~~
 
 
#include <stdio.h>

#define DEFINE_TAB(name, type)                                                 \
                                                                               \
        struct name##_item {                                                   \
                int pos;                                                       \
                type val;                                                      \
        };                                                                     \
                                                                               \
        struct name##_item tab_##name[10];                                     \
                                                                               \
        static inline type get_##name##_val(int pos) {                         \
                return tab_##name[pos].val;                                    \
        }

 
DEFINE_TAB(entier, int);
DEFINE_TAB(str, char *);

int main() {

        tab_entier[0].pos = 0;
        tab_entier[0].val = 100;

        tab_str[0].pos = 0;
        tab_str[0].val = "Hello, world!";

        printf("Value at position 0 in 'entier' table: %d\n",
               get_entier_val(0));
        printf("Value at position 0 in 'str' table: %s\n", get_str_val(0));

        return 0;
}

~~~

</details>
