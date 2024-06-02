
# Utilisation de _Generic

Ecruvez une fonction qui initialise une structure de données *polymorphique* nommée Value. 
Cette structure peut stocker des valeurs de différents types : ``entiers``, ``flottants`` et ``chaînes de caractères``. 


<details>
<summary>Solution</summary>

```cpp

#include <stdio.h>

#define TYPE_INT 0
#define TYPE_FLOAT 1
#define TYPE_STRING 2

struct Value {
    int type;
    union {
        int int_value;
        float float_value;
        char *string_value;
    } data;
};

#define make_value(value) _Generic((value), \
    int: make_int_value, \
    float: make_float_value, \
    char *: make_string_value)(value)

struct Value make_int_value(int value) {
    return (struct Value) {TYPE_INT, {.int_value = value}};
}

struct Value make_float_value(float value) {
    return (struct Value) {TYPE_FLOAT, {.float_value = value}};
}

struct Value make_string_value(char *value) {
    return (struct Value) {TYPE_STRING, {.string_value = value}};
}

int main() {
    // Initialisation de la structure Value avec différents types de données
    struct Value int_val = make_value(10);
    struct Value float_val = make_value(3.14f);
    struct Value string_val = make_value("Hello, world!");

    return 0;
}

```
