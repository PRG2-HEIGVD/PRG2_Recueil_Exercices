# Conversions de et vers des chaines de caracteres
<details>
<summary>Source</summary>
(https://zestedesavoir.com/tutoriels/755/le-langage-c-1/1043_aggregats-memoire-et-fichiers/4283_les-chaines-de-caracteres/)
(https://codereview.stackexchange.com/questions/211384/biginteger-check-in-c-from-a-string)
</details>

## Exercice 1
Ecrire un programme qui demande deux entiers en arguments, les multiplie l'un par l'autre puis affiche le résultat formatté après l'avoir converti et stocké dans une chaine de caractères de longueur minimale et allouée dynamiquement. 

Le programme vérifie que 
1) les arguments sont des entiers avec une fonction à créer _isInteger()_
2) chaque argument tient dans un long long lors de la conversion avec la fonction _strtoll()_ vue en cours 
3) la multiplication des arguments tient dans un _long long_ et est calculée comme indiqué dans https://stackoverflow.com/questions/1815367/catch-and-compute-overflow-during-multiplication-of-two-large-integers

Le programme affiche
1) les erreurs, notamment lors des vérifications ci-avant
2) la taille nécessaire pour la chaîne résultat formattée
3) la chaine résultat formattée avec des séparateurs de milliers

Le programme insert les séparateurs un à un dans la chaine qui contient le résultat. Elle fait pour cela appel à la fonction _insChar()_.

Le programme ne fait pas appel aux fonctions déclarées dans _string.h_.

**NB**: Dans un programme complet, il faudrait encore implémenter la libération des espaces alloués dynamiquement lorsque ceux-ci ne sont plus nécessaires. Cette partie sortant du scope de cet exercice, nous nous en dispenserons à titre exceptionnel.

Exemple
- Commande: _./05-01-convert 123 12345678901234567_
- Sortie: _[i] 25 characters needed to store 1518518504851851741_
- Sortie: _[i] Result = 1'518'518'504'851'851'741_

Exemple 2:
- Commande: _./05-01-convert -123 +123_
- Sortie: _[i] 7 characters needed to store -15129_
- Sortie: _[i] Result = -15'129_

Exemple 3:
- Commande: _./05-01-convert_
- Sortie: _Usage: 05-01-convert <integer 1> <integer 2>_

Exemple 4:
- Commande: _./05-01-convert 1234 12345678901234567890_
- Sortie: _[e] 12345678901234567890 is too long!_

Exemple 5:
- Commande: _./05-01-convert 123456789 1234567890123456789_
- Sortie: _[e] 123456789 * 1234567890123456789 is too long!_

<details>
<summary>Solution</summary>

~~~cpp

#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include <ctype.h>
#include <errno.h>

// check whether a string contains an integer or not
// taken at https://stackoverflow.com/questions/1815367/catch-and-compute-overflow-during-multiplication-of-two-large-integers
bool isInteger(const char *str) {
    // negative or positive sign
    if (*str == '-' || *str == '+') {
        str++;
    }
    // at least one number
    if (*str == '\0') {
        return false;
    }
    // as many as you like
    while (isdigit((unsigned char)*str)) {
        str++;
    }

    return *str == '\0';
}

// insert char c at position pos into string str 
// the resulting string cannot exceed len characters
// len is assumed to be compatible with str buffer
int insChar(char *str, int len, char c, int pos) {
    if ((pos < 0) || (pos > len)) return -1;
    for (int i = len; i >= pos; i--) str[i + 1] = str[i];  // include '\0'
    str[pos] = c;
    return 0;
}

int main(int argc, char *argv[]) {

    // usage: <command> <integer 1> <integer 2>
    if ((argc != 3) || !isInteger(argv[1]) || !isInteger(argv[2])) {
        printf("Usage: %s <integer 1> <integer 2>\n", argv[0]);
        return EXIT_FAILURE;
    }

    long long a, b, mult;  // argument integers and multiplication result

    // check that entered integers are not too long
    errno = 0;             // reset error flag
    a = strtoll(argv[1], NULL, 10);
    if (errno == ERANGE) { // test error flag for overflow
        printf("[e] %s is too long!\n", argv[1]);
        return EXIT_FAILURE;
    }
    b = strtoll(argv[2], NULL, 10);
    if (errno == ERANGE) { // test error flag for overflow
        printf("[e] %s is too long!\n", argv[2]);
        return EXIT_FAILURE;
    }

    mult = a * b;

    // check overflow during multiplication
    if ((a != 0) && (mult / a != b)) {
        printf("[e] %s * %s is too long!\n", argv[1], argv[2]);
        return EXIT_FAILURE;
    }

    // compute needed string length to display result
    int len = snprintf(NULL, 0, "%lld", mult);

    int ns = 0; // number of separators

    if (len < 0) {
        printf("[e] Error while converting %lld\n", mult);
        return EXIT_FAILURE;
    } else {
        ns = (len - 1) / 3;      // count separators 
        len += ns;               // and add them to length
        printf("[i] %d characters needed to store %lld\n", len + 1, mult);
    }

    // allocate the string buffer
    char *res = (char *)calloc(len + 1, sizeof * res); // include '\0'
    if (res == NULL) {
        printf("[e] Could not allocate %d characters!\n", len);
        free(res);
        return EXIT_FAILURE;
    }

    // convert the result to a string
    len = snprintf(res, len + 1, "%lld", mult);
    if (len < 0) {
        printf("[e] Error while converting %lld\n", mult);
        return EXIT_FAILURE;
    }
    // format the result with separators before every 3 digits
    ns = 0; // no separator yet
    for (int i = 1; i < len - 2; i++) { // separators start at position 3 and end before last digit
        if ((len - i) % 3 == 0) {       // separatord at position 3, 6, 9, ...
            if (insChar(res, len + ns, '\'', i + ns) < 0) {  // take into account added separators
                printf("[e] Could not insert separator at position %d!\n", i);
                return -1;
            }
            ns++;                       // one separator added
        }
    }

    printf("[i] Result = %s\n", res);
    free(res);
    return 0;
}

~~~
</details>
