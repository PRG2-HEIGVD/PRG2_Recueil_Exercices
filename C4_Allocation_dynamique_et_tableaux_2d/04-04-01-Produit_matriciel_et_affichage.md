# Produit matriciel et affichage

Complétez le programme ci-dessous pour qu'il compile sans warning et affiche 

~~~
[[1, 2, 3], [4, 5, 6], [7, 8, 9]] * [1, 2, 3] = [14, 32, 50]
~~~

Pour cela, vous devez
- définir les alias de type `mat3x3` et `vec3` de sorte que les déclarations de `m` et `v` soient équivalentes à `double m[3][3]; double v[3];` 
- définir la fonction `mat_vec_mult` qui calcule le produit de la `mat3x3` reçue en premier paramètre par le `vec3` reçu en second paramètre, et stocke le résultat dans le `vec3` reçu en dernier paramètre
- définir la fonction variadique `print_mat` qui fonctionne de manière similaire à `printf` avec un premier paramètre qui spécifie le format à afficher, et les suivants qui sont soit des `mat3x3`, soit des `vec3`. La chaine de format utilise `%m` pour indiquer l'emplacement d'une `mat3x3`, et `%v` pour celui d'un `vec3`.

~~~cpp
#include <stdio.h>

int main() {
    mat3x3 m = {
        {1, 2, 3},
        {4, 5, 6},
        {7, 8, 9}
    };

    vec3 v = { 1, 2, 3 };
    vec3 w = {};

    mat_vec_mult(m, v, w);
    print_mat("%m * %v = %v\n", m, v, w);
}
~~~

Note: le premier paramètre de `print_mat` est de type `const char *`. Il n'est pas nécessaire de passer en paramètre la taille de ce
tableau de `char`, puisque son dernier élément est conventionellement `'\0'` comme pour toutes les chaines de caractères en C. 

<details>
<summary>Solution</summary>

~~~cpp
#include <stdio.h>
#include <stdarg.h>

typedef double vec3[3];
typedef double mat3x3[3][3];

void mat_vec_mult(const mat3x3 m, const vec3 v, vec3 w) {
    for (int i = 0; i < 3; i++) {
        w[i] = 0;
        for (int j = 0; j < 3; j++) {
            w[i] += m[i][j] * v[j];
        }
    }
}

void display_vec(const vec3 v) {
    printf("[");
    for (int i = 0; i < 3; i++) {
        if (i) printf(", ");
        printf("%g", v[i]);
    }
    printf("]");
}

void display_mat(const mat3x3 m) {
    printf("[");
    for (int i = 0; i < 3; i++) {
        if (i) printf(", ");
        display_vec(m[i]);
    }
    printf("]");
}

int print_mat(const char *fmt, ...) {
    int n = 0;
    va_list ap;
    va_start(ap, fmt);

    for (const char *p = fmt; *p != '\0'; p++) {

        if (*p != '%') {
            putchar(*p);
            continue;
        }

        ++n;
        switch (*++p) {
        case 'm':
            display_mat((double(*)[3]) va_arg(ap, double(*)[3]));
            break;
        case 'v':
            display_vec((double *)va_arg(ap, double *));
            break;
        default:
            return -1;
        }
    }
    va_end(ap);
    return n;
}

int main() {
    mat3x3 m = {
        {1, 2, 3},
        {4, 5, 6},
        {7, 8, 9}
    };

    vec3 v = { 1, 2, 3 };
    vec3 w = {};

    mat_vec_mult(m, v, w);
    print_mat("%m * %v = %v\n", m, v, w);
}

~~~

</details>