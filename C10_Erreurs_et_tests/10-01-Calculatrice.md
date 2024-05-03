# Fichiers texte
<details>
<summary>Sources</summary>
https://cedric.cnam.fr/~lamberta/enseignements/C/corrections/07/correction_tp7.pdf
</details>

## Exercice 1
Voici ci-après un programme C à compléter pour compter les caractères dans un fichier texte.

<details>
<summary>09-02_count.c</summary>

~~~cpp
#include <stdio.h>

// *** TODO: counting functions

int main(int argc, char **argv) {
    if (argc != 2) {
        printf("Usage: %s file\n", argv[0]);
        return 1;
    }

	FILE *fin;

    // *** TODO: open file

    // count characters in file

    printf("%s contains %d characters\n", argv[1], countc(fin));

    fclose(fin);
    return 0;
}
~~~

</details>
<p></p>

Pour le compléter:
- écrire la fonction _countc()_ qui renvoie le nombre de caractères contenu dans le flux de texte passé en argument à la fonction, 
- ouvrir le fichier texte passé en argument à la commande.

<details>
<summary>Solution</summary>

~~~cpp

...

int countc(FILE *f) {
    int cpt = 0;

    while (fgetc(f) != EOF)
        cpt++;

    return cpt;
}

...

    // *** TODO: open file

    FILE *fin = fopen(argv[1], "r");
    if (!fin) {
        printf("[e] could not open %s\n", argv[1]);
        return 1;
    }

    // count characters in file

...


~~~
</details>


## Exercice 2
Compléter le programme de l'exercice précédent pour qu'il compte également le nombre de mots.

NB: Les mots sont séparés par des espaces ou des retours à la ligne.


<details>
<summary>Solution</summary>

~~~cpp

#include <stdbool.h>

...

int countm(FILE *f) {
    int c;
    int cpt = 0;
    int curw = false;

    while ((c = fgetc(f)) != EOF)
        if (c == ' ' || c == '\n')
            curw = false;
        else if (!curw) {
            curw = true;
            cpt++;
        }

    return cpt;
}

...

    // reopen file
	
    fin = freopen(argv[1], "r", fin);
    printf("%s contains %d words\n", argv[1], countm(fin));

...

~~~

</details>
<p></p>

Pour le compléter:
- écrire une fonction _countm()_ qui renvoie le nombre de mots dans le flux passé en argument de la fonction,
- le fichier texte passé en argument à la commande doit être remis au début.



## Exercice 3
Compléter le programme des exercices précédents pour qu'il compte en plus le nombre de lignes. Pource faire, écrire une fonction _countl()_ qui renvoie le nombre de lignes dans le flux passé en argument.

<details>
<summary>Solution</summary>

~~~cpp

int countl(FILE *f) {
    int c = fgetc(f);
    int cpt = (c != EOF);

    while ((c = fgetc(f)) != EOF)
        if (c == '\n')
            cpt++;

    return cpt;
}

...

    // rewind can be used in place of freopen to move at the beginning

    rewind(fin);
    printf("%s contains %d lines\n", argv[1], countl(fin));

...

~~~

</details>
