# Opérations sur les buffers
<details>
<summary>Source</summary>
(...)
</details>

## Exercice 1
Compléter le programme ci-après qui, dans un premier temps, invoque la fonction _moreTxt()_ pour initialiser itérativement une liste de mots dans une structure mémoire allouée dynamiquement au fur et à mesure que l'utilisateur les rentre sur _stdin_. Dans un deuxième temps, le programme invoque la fonction _makePhrase()_ qui alloue dynamiquement un buffer pour une chaîne de caractères unique composée de tous les mots rentrés séparés par un espace.

~~~

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// ask for a new word on stdin
// add the new word to the list
int moreTxt(char ***ptrList) {
    // **********************
    // *** your code here ***
    // **********************
}

// build a string (a phrase) from a list of words
// Words are appended, separated by a white space
int makePhrase(char **ptrPhrase, char **list) {
    // **********************
    // *** your code here ***
    // **********************
}

int main(void) {
    char **list = NULL;    // <-- list of words
    char *phrase = NULL;   // <-- resulting "phrase"
    int count = 0;         // <-- number of words in list

    // reading words
    while (moreTxt(&list)) ++count;
    printf("[i] La liste finale contient %d mots\n", count);

    // making phrase
    if (count >0) {
       if (makePhrase(&phrase, list))
           printf("[i] Ce qui donne la phrase \"%s\"\n", phrase);
    }

    return 0;
}
~~~

Les buffers alloués pour contenir la liste de mots entrés par l'utilisateur ainsi que la phrase résultante sont dimensionnés au strict minimum nécessaire. L'espace pour la liste de mots est allouée de manière incrémentale en utilisant la fonction _realloc()_ de _string.h_ chaque fois qu'un mot est ajouté. Le programme affiche les changements d'adresse de la liste, suite à cette réallocation. La fin de la liste est décidée par l'utilisateur en rentrant le mot "0".

La phrase est construite à partir de la liste de mots en utilisant la fonction _memcpy()_ de _string.h_.

**NB**: Dans un programme complet, il faudrait encore implémenter la libération des espaces alloués dynamiquement lorsque ceux-ci ne sont plus nécessaires. Cette partie sortant du scope de cet exercice, nous nous en dispenserons à titre exceptionnel.

Exemple:
- Entrée: _[?] Ajouter un mot (STOP = '0'): Ceci_
- Entrée: _[?] Ajouter un mot (STOP = '0'): est_
- Sortie: _[i] la liste a ete deplacee de 3ad61450 a 3ad61490_
- Entrée: _[?] Ajouter un mot (STOP = '0'): un_
- Entrée: _[?] Ajouter un mot (STOP = '0'): mot_
- Sortie: _[i] la liste a ete deplacee de 3ad61490 a 3ad614e0_
- Entrée: _[?] Ajouter un mot (STOP = '0'): 0_
- Sortie: _[i] La liste finale contient 4 mots_
- Sortie: _[i] Ce qui donne la phrase "Ceci est un mot"_

<details>
<summary>Solution</summary>

~~~

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// ask for a new word on stdin
// add the new word to the list
int moreTxt(char ***ptrList) {
    char word[256];        // word to read on stdin
    char **list = *ptrList;

    // prompting user for new word
	printf("[?] Ajouter un mot (STOP = '0'): ");
    scanf("%255s", word);  // <-- words should not exceed 255 characters

    if (word[0] == '0') return 0; // <-- 0 = stop

    // first time, list shall be created with no content
    if (list == NULL) {
        list = (char **)calloc(1, sizeof list);
        if (list == NULL) return -1;
        *list = NULL;      // first word empty yet
    }

    char **curWord = list; // <-- place to store new word
    int count = 0;         // <-- counting words in list

    // finding end of list
    while (*curWord != NULL) {
        curWord++;
        count++;
    }
    count++;

    // extending list with room for the new word
    //(NULL pointer at end)
    char **old = list;     // <-- backup current list address
    list = realloc(list, (count + 1) * sizeof(*list));
    if (list == NULL) return -1;
    if (list != old) {
        printf("[i] la liste a ete deplacee de %8x a %8x\n", old, list);
        curWord = list + (curWord - old);
    }
    
    // adding word to list
    int ll = strlen (word); // number of characters composing word

    *curWord = (char *)calloc(ll+1, sizeof(char));
    if (*curWord == NULL) return -1;

    strncpy(*curWord, word, ll);

    // setting end of list to NULL
    *(curWord + 1) = NULL;

    // updating pointer to new list
    *ptrList = list;

    return count;          // <-- positive <=> continue
}

// build a string (a phrase) from a list of words
// Words are appended, separated by a white space
int makePhrase(char **ptrPhrase, char **list) {
    int ll = 0;
    char **curWord = list;
    char *phrase = *ptrPhrase;

    // counting characters
    while (*curWord != NULL) {
        ll += strlen(*curWord) + 1;  // +1 for separators between words and '\0' at the end
        curWord++;
    }
    
    // creating the string for the phrase
	phrase = (char *)calloc(ll, sizeof(char));
    if (phrase == NULL) return 0;

    // adding words from the list to the phrase
    curWord = list;
    char *ptr = phrase;
    while (*curWord != NULL) {
        ll = strlen(*curWord);
        memcpy(ptr, *curWord, ll);
        ptr += ll;
        *ptr = ' ';
        ptr++;
        curWord++;
    }
    *(ptr - 1) = '\0';

    // updating pointer to phrase
    *ptrPhrase = phrase;

    return 1;
}

int main(void) {
    char **list = NULL;    // <-- list of words
    char *phrase = NULL;   // <-- resulting "phrase"
    int count = 0;         // <-- number of words in list

    // reading words
    while (moreTxt(&list)) ++count;
    printf("[i] La liste finale contient %d mots\n", count);

    // making phrase
    if (count >0) {
       if (makePhrase(&phrase, list))
           printf("[i] Ce qui donne la phrase \"%s\"\n", phrase);
    }

    return 0;
}

~~~

</details>
