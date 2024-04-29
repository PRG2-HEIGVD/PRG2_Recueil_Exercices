# Fichiers et buffers
<details>
<summary>Sources</summary>
https://fastbitlab.com/microcontroller-embedded-c-programming-lecture-66-scanf-exercise-implementation/
</details>

## Exercice 1
Le dernier slide du chapitre 9 sur les fichiers montre un extrait de l'output obtenu en exécutant un programme sous Windows pour illustrer l'effet du buffering sur _stdout_. Un output complet très proche de celui de ce slide est illustré ci-après.

<details>
<summary>output de bufferedPrint</summary>

```
INFO: produit sous Windows 11/x86_64 avec mingw.
```

```
$ .\bufferedPrint.exe 10 0
buffer size = 10 characters
flush never
Enter the first numb
```

```
$ .\bufferedPrint.exe 10 0
buffer size = 10 characters
flush never
Enter the first numb12
er: Enter the second
```

```
$ .\bufferedPrint.exe 10 0
buffer size = 10 characters
flush never
Enter the first numb12
er: Enter the second12
 number: Enter the third numbe
```

```
$ .\bufferedPrint.exe 10 0
buffer size = 10 characters
flush never
Enter the first numb12
er: Enter the second12
 number: Enter the third numbe12
r:
Average is : 12.000000
```

</details>

### Question 1: Expliquez l'output ci-avant avec le mécanisme de buffering par bloc mis en oeuvre dans cet exemple.

<details>
<summary>Solution</summary>
<p></p>

    $ .\bufferedPrint.exe 10 0

On appelle le programme avec un buffer de 10 octets (caractères) et on désactive les appels à fflush().

    buffer size = 10 characters
    flush never
    Enter the first numb

L'affichage s'arrête après avoir afficher les **20 premiers caractères** de la chaîne d'invite _"Enter the first number: "_ qui en compte 24. En fait, le buffer de 10 caractères alloué pour stdout a été vidé chaque fois qu'il était plein jusqu'à rencontrer le premier _scanf()_ qui met le programme en attente d'input sur stdin. Le buffer de stdout contient maintenant les 4 caractères _"er: "_ pas encore affichés.
 
    Enter the first numb12
    er: Enter the second

Après que l'utilisateur ait saisi une valeur (_12_ ici), le _scanf()_ rend la main au programme qui affiche **encore 20 caractères** correspondant aux 4 caractères en attente dans le buffer suivi des 16 premmier caractères de la 2ème invite _"Enter the second number: "_ qui en compte 25. Le programme est maintenant en attente sur le second scanf() et il reste 9 caractères dans le buffer: _" number: "_.

    er: Enter the second12
     number: Enter the third numbe

Même opération que précédemment, sauf que cette fois le programme a pu afficher **30 caractères** incluant les 9 caractères encore dans le buffer et 21 des 24 caractères de la dernière invite. Après celà, il reste donc les 3 caractères "r: " dans le buffer.

    number: Enter the third numbe12
    r:
    Average is : 12.000000

Finalement, le dernier scanf() traite la 3ème entrée de l'utilisateur et le programme se termine, provoquant la fermerture de stdin et stdout et donc l'envoi du reste du buffer de ce dernier au terminal.

</details>

## Exercice 2
Le code du programme _bufferedPrint.c_ fourni ci-après est le code source du programe utilisé pour produire l'output de l'exercice précédent. 

<details>
<summary>09-04-bufferedPrint.c</summary>

~~~cpp
// this program demonstrates the need for flushing buffer when stdout is in full buffer mode (no care for '\n')
// call it like this: bufferedPrint <nb chars in buffer for display> <when fflush should be invoked>
// fflush to be invoked
// - 0 => never
// - 1 => only after all inputs were requested
// - >1 => everytime

#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[]) {

    if (argc != 3) {
        printf("Usage: bufferedPrint <buffer size = nb chars> <nb flushes = 0 (never), 1 (at end) or more (always)>\n");
        return EXIT_FAILURE;
    }

    int bufsiz = atoi(argv[1]), nflush = atoi(argv[2]);
    printf("buffer size = %d characters\n", bufsiz);
    printf("flush %s\n", (nflush > 1) ? "always" : (nflush > 0) ? "at the end" : "never");

    // according to https://en.cppreference.com/w/c/io/setvbuf
    // set FULL BUFFERING mode with fixed length
    setvbuf(stdout, (char *)NULL, _IOFBF, bufsiz);

    float number1, number2, number3;
    float average;

    printf("Enter the first number: ");
    if (nflush > 1) fflush(stdout);
    scanf("%f", &number1);
    printf("Enter the second number: ");
    if (nflush > 1) fflush(stdout);
    scanf("%f", &number2);
    printf("Enter the third number: ");
    if (nflush > 0) fflush(stdout);
    scanf("%f", &number3);

    average = (number1 + number2 + number3) / 3;

    printf("\nAverage is : %f\n", average);

    return 0;
}
~~~

</details>

<p></p>

Compilez-le et exécutez-le dans votre VM Ubuntu afin de répondre aux questions ci-après.

### Question 1: Lorsque le programme est exécuté avec les mêmes arguments que pour l'exercice précédent, quelle différence observe-t-on? Quel rôle joue les 2 OS dans cette différence de comportement?

<details>
<summary>Solution</summary>
<p></p>

L'exécution sous Linux produit l'output suivant:

~~~
$ 09-04-bufferedPrint 10 0
buffer size = 10 characters
flush never
12
Enter the first number: 12
12
Enter the second number: Enter the third number: 
Average is : 12.000000
~~~

On constate que les prompts sont affichés en entier mais après que l'on ait saisi les inputs. 

Cette différence de comportement peut être en partie expliquée par le fait que l'ordre de buffering passé avec _setvbuf()_ est traité de manière différente par les 2 OS:
- Sous Windows: le buffering est effectué en mode bloc avec des blocs de 10 caractères, comme demandé
- sous Linux: le buffering est en fait effectué en mode texte, le buffer est vidé lorsque un '\n' y est écrit

</details>

### Question 2: Si on analyse plus avant l'output produit dans les mêmes conditions que pour la question précédente, on peut aussi noter une "bizarrerie" dans l'affichage sous Linux. Quelle est-elle? Comment l'expliquer?

<details>
<summary>Solution</summary>

<p></p>
La différence de stratégie de buffering n'explique pas tout le comportement que nous observons dans l'output de la question précédente. En effet, si on regarde le code source de <i>09-04-bufferedPrint.c</i> on peut noter que les invites sont affichées par des appels à <i>printf()</i> sans <i>'\n'</i>. Les 3 invites ne devraient donc pas être affichées à l'écran avant que le résultat du calcul de ne le soit, puisque c'est ici seulement que le premier <i>'\n'</i> est écrit dans le buffer de <i>stdout</i>. Or, les messages d'invite sont affichés:

- le premier, après avoir saisi 1 input,
- les 2 suivants, après avoir saisi les 2 inputs suivants.

Cette observation plus attentive ne peut pas être expliquée avec la seule différence de stratégie de buffering entre les 2 OS. Pour l'expliquer il faut d'abord savoir que, sur les systèmes Posix:

1) _stdin_ et _stdout_ appliquent toutes deux une stratégie de buffering par ligne lorsque elles sont en mode buffering.
2) _stdout_ est automatiquement vidée lorsqu'une tentative de lecture sur _stdin_ nécessite une lecture depuis le terminal.

Ensuite, il faut comprendre les appels à _scanf()_ et leur influence sur _stdin_. Sans rentrer dans les détails, _scanf()_ ne consomme pas le _'\n'_ après le nombre lu. C'est le _scanf()_ suivant qui le fait pour atteindre le premier caractère qui correspond au format attendu: "%d" attend un chiffre. La lecture de la ligne sur _stdin_, et donc le vidage de _stdout_, est déclenchée en retard d'un _scanf()_, ce qui esplique le phénomène observé.

</details>
