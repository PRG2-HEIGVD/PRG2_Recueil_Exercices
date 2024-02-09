# Structures de contrôle - switch
<details>
<summary>Source</summary>
(https://developpement-informatique.com/article/316/exercices-corriges-pour-maitriser-la-structure-de-controle-if-else)
</details>

## Exercice 1
Écrivez un programme pour Saisir le numéro du jour dans une semaine (1-7) et affichez le nom du jour à l'aide de "switch" et "case". Le programme doit également signaler les erreurs dans les données d'entrée.

Exemple
- Entrée: _Saisir le numéro du jour : 2_
- Sorties: _Mardi_

<details>
<summary>Solution</summary>

~~~cpp

#include <stdio.h>

int main()
{
    int jour;

    /* Saisir les données d'entrée */
    printf("Saisir le numero de jour : ");
    scanf("%d", &jour);
     
    switch(jour)
    {
        case 1: 
            printf("Lundi");
            break;
        case 2: 
            printf("Mardi");
            break;
        case 3: 
            printf("Mercredi");
            break;
        case 4: 
            printf("Jeudi");
            break;
        case 5: 
            printf("Vendredi");
            break;
        case 6: 
            printf("Samedi");
            break;
        case 7: 
            printf("Dimanche");
            break;
        default: 
            printf("Entree invalide! Veuillez saisir un chiffre entre 1 et 7.");
    }
 
    return 0;
}

~~~
</details>

## Exercice 2
Écrivez un programme pour créer une calculatrice qui exécute des opérations arithmétiques de base (additionn, soustration, multiplication et division) en utilisant "switch" et "case". Le programme doit également signaler les erreurs dans les données d'entrée.

Exemple
- Entrées: _7 - 2_
- Sorties: _5_

<details>
<summary>Solution</summary>

~~~cpp

#include <stdio.h>
 
int main()
{
    char op;
    float num1, num2, resultat=0.0f;
 
    /* Saisir les données d'entrée */
    printf("Saisir une operation [Num 1] [+ - * /] [Num 2]\n");
    scanf("%f %c %f", &num1, &op, &num2);
 
    switch(op)
    {
        case '+': 
            resultat = num1 + num2;
            break;
 
        case '-':
            resultat = num1 - num2;
            break;
 
        case '*':
            resultat = num1 * num2;
            break;
 
        case '/': 
            resultat = num1 / num2;
            break;
 
        default:
            printf("Operation mal formee ou inconnue!");
			return -1;
    }
 
    /* afficher le résultat */
    printf("%.2f %c %.2f = %.2f", num1, op, num2, resultat);
 
    return 0;
}

~~~
</details>
