# Chaines de caracteres
<details>
<summary>Source</summary>
(https://zestedesavoir.com/tutoriels/755/le-langage-c-1/1043_aggregats-memoire-et-fichiers/4283_les-chaines-de-caracteres/)
</details>

## Exercice 1
Un palindrome est un texte identique lorsqu’il est lu de gauche à droite et de droite à gauche. Ainsi, le mot radar est un palindrome, de même que les phrases "engage le jeu que je le gagne" et "élu par cette crapule". Normalement, il n’est pas tenu compte des accents, trémas, cédilles ou des espaces. Toutefois, pour cet exercice, nous nous contenterons de vérifier si un mot donné est un palindrome sans tenir compte des majuscules.

Exemple
- Entrée: _[?] Saisir un mot (que des lettres dans [a-z,A-Z]): Radar_
- Sortie: _[i] Radar est un palindrome_

Exemple 2:
- Entrée: _baobab_
- Sortie: _[i] baobab n'est pas un palindrome_

<details>
<summary>Solution</summary>

~~~cpp

#include <stdio.h>
#include <stdbool.h>
#include <ctype.h>

// test si la chaine passée en argument est un palindrome
bool palindrome(char *s) {
    size_t len = 0;

    // calcul de la longueur de la chaine
    while (s[len] != '\0') len++;

    // test de la symétrie des caractères
    for (size_t i = 0; i < len; i++)
        if (s[i] != s[len - 1 - i]) return false;

    return true;
}

int main() {
    char str[100], upstr[100];
    int i;

    // mot à saisir
    printf("[?] Saisir un mot (que des lettres dans [a-z,A-Z]): ");
    scanf("%99s", str);

    // conversion en majuscules
    for (i = 0; i < 99; i++) upstr[i] = toupper(str[i]);

    // test du palindrome sur le mot en majuscules
    if (palindrome(upstr)) printf("[i] %s est un palindrome\n", str);
    else printf("[i] %s n'est pas un palindrome\n", str);

    return 0;
}

~~~
</details>

## Exercice 2
Écrivez un programme qui lit une ligne et vérifie que chaque parenthèse ouvrante est bien refermée par la suite.
 
**NB:** La lecture de la ligne est effectuée dans une fonction _lire_ligne()_ qui lit les caractères un à un jusqu'au retour à la ligne (_'\n'_) ou après avoir atteint la longueur de ligne maximale passée en paramètre. La fonction signale une éventuelle erreur lors de l'opération au code appelant.

Exemple
- Entrée: _[?] Saisissez une ligne de mots et parentheses: printf("%zu\n", sizeof(int);_
- Sortie: _[i] Il manque des parentheses :(_

<details>
<summary>Solution</summary>

~~~cpp

#include <stdio.h>
#include <stdbool.h>

// Wikipedia: "Lisp (historically LISP, an abbreviation of "list processing") is a family of programming languages
//             with a long history and a distinctive, fully parenthesized"
// Anecdote: certains interprètent "lisp" comme "Lot of Idiot and Stupid Parenthesis"... :o 

bool lire_ligne(char *chaine, size_t max) {
    size_t i;

    for (i = 0; i < max - 1; ++i) {
        char c;

        if (scanf("%c", &c) != 1) return false;
        else if (c == '\n') break;

        chaine[i] = c;
    }

    chaine[i] = '\0';
    return true;
}

int main(void) {
    char s[255];

    printf("Saisissez une ligne de mots et parentheses: ");

    if (!lire_ligne(s, sizeof s)) {
        printf("[!] Erreur lors de la saisie.\n");
        return -1;
    }

    int n = 0;

    for (char *t = s; *t != '\0'; ++t) {
        if (*t == '(') ++n;
        else if (*t == ')') --n;

        if (n < 0) break;
    }

    if (n == 0) printf("[i] Le compte est bon :)\n");
    else printf("[i] Il manque des parentheses :(\n");

    return 0;
}

~~~
</details>

## Exercice 3
Ecrire un programme qui demande une ligne de mots et en extrait puis affiche une sous-chaîne.

**NB**:
- Le programme doit valider les positions et longueurs entrées par l'utilisateur sans avoir recours à _string.h_.
- Le programme doit refuser de traiter les lignes vides.
- La lecture de la ligne de mot doit se faire avec la fonction _fgets()_ de la librairie standard.

Exemple
- Entrée: _[?] Saisissez une ligne de mots: c'est la chaine de test_
- Sortie: _[i] La ligne contient 23 caracteres._
- Entrée: _[?] Extraire APRES combien de caracteres? 9_
- Entrée: _[?] Extraire combien de caracteres? 4_
- Sortie: _[i] La sous-chaîne extraite de la chaîne est : "test"_

Exemple 2
- Entrée: _[?] Saisissez une ligne de mots: c'est la chaîne de test_
- Sortie: _[i] La ligne contient 23 caracteres._
- Entrée: _[?] Extraire APRES combien de caracteres? 29_
- Sortie: _[!]Erreur: position impossible!_

Exemple 3
- Entrée: _[?] Saisissez une ligne de mots: c'est la chaîne de test_
- Sortie: _[i] La ligne contient 23 caracteres._
- Entrée: _[?] Extraire APRES combien de caracteres? 9_
- Entrée: _[?] Extraire combien de caracteres? Z_
- Sortie: _[!] Erreur: longueur impossible!_

<details>
<summary>Solution</summary>

~~~cpp

#include <stdio.h>

int main() {
    char str[100], sstr[100]; // Input string and extracted substring
    int pos = -1, l = 0, ll = 0, c = 0; // Position, lengths of string and substring, counter

    printf("\n\nExtract a substring from a given string:\n"); // Information about the task
    printf("----------------------------------------\n");

    // Get a string from the standard input
    printf("[?] Saisissez une ligne de mots: ");
    fgets(str, sizeof str, stdin);
    while (!(str[ll] == '\n' || str[ll] == '\0')) ll++;
    str[ll] = '\0'; // Remove '\n' if any

    printf("[i] La ligne contient %d caracteres.\n", ll);
    if (ll == 0) {
        printf("[!] Erreur: ligne vide!\n");
        return -1; // Stopped execution
    }

    // Get the starting position of substring
    printf("[?] Extraire APRES combien de caracteres? ");
    pos = -1;
    if (scanf("%d", &pos) == 0 || pos < 0 || pos >= ll) {
        printf("[!] Erreur: position impossible!\n");
        return -1; // Stopped execution
    }

    printf("[?] Extraire combien de caracteres? ");
    // Get the length of the substring
    if (scanf("%d", &l) == 0 || l <= 0 || l >= (ll - pos)) {
        printf("[!] Erreur: longueur impossible!\n");
        return -1; // Stopped execution
    }

    // Extracting the substring
    while (c < l) {
        sstr[c] = str[pos + c]; // Copy characters from the specified position
        c++;
    }
    sstr[c] = '\0'; // end of the substring
    printf("[i] La sous-chaîne extraite de la chaîne est : \"%s\" \n\n", sstr); // Display the extracted substring

    return 0; // Successful execution
}


~~~
</details>
