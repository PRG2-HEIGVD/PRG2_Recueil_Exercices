# Fichiers et buffers

## Exercice 1
Le dernier slide du chapitre 9 sur les fichiers montre un extrait de l'output obtenu en exécutant un programme sous Windows pour illustrer l'effet du buffering sur _stdout_. Un output complet très proche de celui de ce slide est illustré ci-après.

<details>
<summary>output de bufferedPrint</summary>

```
WARNING: testé sous Windows 11/x86_64 avec mingw.
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
// taken at https://fastbitlab.com/microcontroller-embedded-c-programming-lecture-66-scanf-exercise-implementation/
// this program demonstrate the need for flushing buffer when stdout is in full buffer mode (no care for '\n')
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

Compilez-le et exécutez-le afin de répondre aux questions ci-après.

### Question 1: TBD

<details>
<summary>Solution</summary>
<p></p>

TBD	
</details>

### Question 2: TBD

<details>
<summary>Solution</summary>

<p></p>

TBD
</details>
