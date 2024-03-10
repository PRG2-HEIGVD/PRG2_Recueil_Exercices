# Arguments de main
<details>
<summary>Source</summary>
(https://azrael.digipen.edu/~mmead/www/mg/getopt/index.html)
(https://stackoverflow.com/questions/53200469/getopts-doesnt-work-when-there-are-arguments-before-options)
</details>

## Exercice 1
Combien d'options, d'arguments d'option\*, et d'arguments de commande\* de la commande sont contenus dans la ligne de commande suivante? Listez les à chaque fois pour justifier chaque réponse (nombre).

_$ gcc -Wall -Wextra main.c foo.c bar.c -O -o program -ansi -pedantic -Werror_

(\*) un argument d'option est un argument qui précise la valeur attendue par l'option. Un argument de commande n'est pas lié à une option.

<details>
<summary>Solution</summary>

On distingue
- 1 commande: _gcc_
- **7** options: _-Wall  -Wextra  -O  -o  -ansi  -pedantic  -Werror_
- **1** argument d'option: _program_
- **3** arguments de commande: _main.c  foo.c  bar.c_

</details>

## Exercice 2
Écrivez un programme _my_cmd_ qui applique la fonction _getopt()_ pour analyser la ligne de commande et afficher le détail de ce qu'elle contient:
- les options reconnues et l'éventuel argument lié, 
- les options qui ne sont pas admises,
- les arguments d'option manquants, 
- les arguments de commande.
 
**NB:** 
- Le programme ne doit pas s'arrêter en cas d'erreur. Il affiche le message signalant l'erreur et traite l'élément suivant de la ligne de commande (dans l'ordre de _getopt()_).
- La fonction getopt() attend les options avant les arguments de commande. Il faut donc traiter manuellement les cas ci-après qui ne respectent pas cette attente.

Exemple
- Commande: _./my_cmd x -a a_
- Sortie: _[!] Option a, argument manquant!_
- Sortie: _[i] Arguments de commande a_

Exemple 2
- Commande: _./my_cmd x -a_
- Sortie: _[!] Option a, argument manquant!_

Exemple 3
- Commande: _./my_cmdt -a one -t -X -b_
- Sortie: _[i] Option a, argument one_
- Sortie: _[!] Option t inconnue!_
- Sortie: _[i] Option X_
- Sortie: _[!] Option b, argument manquant!_ 

Exemple 4
- Commande:  _./my_cmd x y -a one -b two -X z_
- Sortie: _[i] Option a, argument one_
- Sortie: _[i] Option b, argument two_
- Sortie: _[i] Option X_
- Sortie: _[i] Arguments de commande x y z_

<details>
<summary>Solution</summary>

~~~cpp

#include <stdio.h>  // --> printf()
#include <getopt.h> // --> getopt()
#include <string.h> // --> strncat()

int main(int argc, char *argv[]) {
    int opt;
    char cmdargs[512] = "";  // to keep command arguments lying before or between options

    optind = 0;
    while (optind < argc) {  // needed to loop on command arguments before or between options
        while ((opt = getopt(argc, argv, ":a:b:X")) != -1) {
            switch (opt) {
            case 'a':
            case 'b':
                printf("[i] Option %c, argument %s\n", opt, optarg);
                break;
            case 'X':
                printf("[i] Option X\n");
                break;
            case '?':
                printf("[!] Option %c inconnue!\n", optopt);
                break;
            case ':':
                printf("[!] Option %c, argument manquant!\n", optopt);
                break;
            }
        }

        /* Get all of the non-option arguments */
        if (optind < argc) {
            //            printf("[i] Argument de commande ");
            while ((optind < argc)
                && (argv[optind][0] != '-')) { // some more options remaining? 
                //                printf("%s ", argv[optind++]);
                strncat(cmdargs, argv[optind++], 511);
                strncat(cmdargs, " ", 511);
            }
            //            printf("\n");
        }
    }
    if (cmdargs[0] != '\0') printf("[i] Arguments de commande %s\n", cmdargs);

    return 0;
}

~~~
</details>
