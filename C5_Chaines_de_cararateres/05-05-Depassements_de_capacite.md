# Dépassements de capacité

## Exercice 1
Le code ci-après est celui du programme _bof.c_ mentionné dans les slides du chapitre 5. Compilez-le et exécutez-le afin de répondre aux questions ci-après.

<details>
<summary>bof.c</summary>

~~~cpp

#include <stdio.h>
#include <string.h>

int moreTxt(char *list) {
    char str[5];  // <-- tiny sized array

    printf("> Ajouter un mot (exit = '0'): ");
    scanf("%s", str); // <-- unlimited input!!!

    if (str[0] == '0') return 0; // <-- 0 = stop
    printf("- Je note %s!\n", str);

    if (strlen(list) > 0) strcat(list, ",");
    strcat(list, str); // <-- unlimited extension!!!
    printf(". La liste contient maintenant: %s\n", list);
    return 1; // <-- 1 = continue
}

void overW(const char *src, char *dst) {
    int i;

    for (i = 0; src[i] != '\0'; i++) dst[i] = src[i];
    dst[i] = '\0';
}

void overR(const char *src, int len) {
    int i;

    for (int i = 0; i < len; i++) printf("%c", src[i]);
    printf("\n");
}

int testOverRW() {
    // arrays below are restricted-size buffers
    char sec4[20] = "C3C1 3ST UN S3CR3T";
    char sec3[20] = "CECI EST UN SECRET";
    char outs[20] = { 0 };
    char sec2[20] = "ceci est un secret";
    char inps[40] = "phrase par defaut";
    char sec1[20] = "ceci est un secret";
    int inpl = 0;

    while (1) {
        fflush(stdin); // <-- needed to flush previous inputs
        printf("! phrase actuelle = '%s'\n", inps);
        printf("> voulez-vous la changer (no = '1', exit = '0')? ");
        scanf("%d", &inpl); // <-- integer input
        if (inpl == 0) return 0; // <-- 0 = stop
        if (inpl != 1) {
            printf("> Saisir une phrase a copier: ");
            fflush(stdin); // <-- again, need to flush inputs
            fgets(inps, sizeof inps, stdin); // <-- limited input!!!
            inps[strlen(inps) - 1] = '\0';   // <-- remove '\n'
        }
        printf(". La chaine a lire fait %d caracteres\n", strlen(inps));
        printf(". La chaine a ecrire attend %d caracteres\n", (sizeof outs / sizeof * outs) - 1);

        overW(inps, outs);  // <-- test overwrite
        printf(". La chaine ecrite contient: %s\n", outs);

        printf("> Saisis le nombre de lettres a afficher (exit = '0'): ");
        scanf("%d", &inpl);  // <-- integer input
        if (inpl == 0) return 0; // <-- 0 = stop

        overR(inps, inpl); // <-- test overread
    }
    return 1; // <-- 1 = continue
}

int main(void) {
    char str[16] = { 0 };  // <-- short sized array

    while (moreTxt(str));
    printf("La liste finale contient: %s\n---\n\n", str);

    while (testOverRW());
    printf("Done!\n---\n\n");

    return 0;
}

~~~
</details>

### Question 1: Essayez de mettre le mot le plus long possible en entrée. Qu'observez-vous? A partir de quelle(s) taille(s)? Que se passe-t-il?

<details>
<summary>Solution</summary>
<p></p>

```
WARNING: testé sous Windows 11/x86_64 avec mingw, vous pouvez obtenir un comportement légèrement différent sur une autre plateforme.
```

Si on entre un mot de **4 caractères ou moins**, on reste dans l'espace de _char str[5]_ prévu par la fonction _moreTxt()_ et on obtient la sortie suivante. 

    $ ./bof
    > Ajouter un mot (exit = '0'): long
    - Je note long!
    . La liste contient maintenant: long
    > Ajouter un mot (exit = '0'): 0
    La liste finale contient: long

Si on rentre **5 caractères**, la fonction _moreTxt()_  semble se comporter correctement jusqu'à ce que le programme appelant (_main()_) affiche le résultat. On a effectivement provoqué un dépassement de capacité car les 5 caractères tiennent bien dans _char str[5]_ mais le '\0' qui termine la chaîne est lui ajouté en dehors de l'espace alloué: i.e. dans _str[5]_.

    $ ./bof
    > Ajouter un mot (exit = '0'): longs
    - Je note longs!
    . La liste contient maintenant: longs
    > Ajouter un mot (exit = '0'): 0
    La liste finale contient: x²ƒ^╕

A partir de **6 caractères**, le programme hésite puis se termine en erreur sans avoir pu afficher la ligne résultat. La mémoire est corrompue.

    $ ./bof
	> Ajouter un mot (exit = '0'): treslo
    - Je note treslo!
    . La liste contient maintenant: treslo
    > Ajouter un mot (exit = '0'): 0
    La liste finale contient:
    $ 

</details>

### Question 2: En rentrant plusieurs mots de 4 caractères (2, 3, ... 40) qu'observez-vous? A partir de quelle(s) taille(s)? Que se passe-t-il?

<details>
<summary>Solution</summary>
<p></p>

```
WARNING: testé sous Windows 11/x86_64 avec mingw, vous pouvez obtenir un comportement légèrement différent sur une autre plateforme.
```

On peut dépasser les **15 caractères** prévus dans _char str[16]_ (15 caractères suivi du '\0') par le _main()_. Si on rentre 4 mots de 4 caractères, tout se passe bien. La concaténation de ces 16 caractères et des 3 séparateurs ',' donnent une longueur totale de **20 caractères** avec le '\0' sans perturber le programme.

En revanche, on obtient un comportement altéré en entrant un 5ème mot de 4 caractères. La chaîne résultante de **25 caractères** provoque dans l'exemple ci-après un effet inattendu. Le programme se met à boucler là où aucune boucle n'existe dans le code source! Nous avons altéré la mémoire du processus ce qui altère son comportement.

	$ ./bof
	> Ajouter un mot (exit = '0'): 1234
	- Je note 1234!
	. La liste contient maintenant: 1234
	> Ajouter un mot (exit = '0'): 1234
	- Je note 1234!
	. La liste contient maintenant: 1234,1234
	> Ajouter un mot (exit = '0'): 1234
	- Je note 1234!
	. La liste contient maintenant: 1234,1234,1234
	> Ajouter un mot (exit = '0'): 1234
	- Je note 1234!
	. La liste contient maintenant: 1234,1234,1234,1234
	> Ajouter un mot (exit = '0'): 1234
	- Je note 1234!
	. La liste contient maintenant: 1234,1234,1234,1234,1234
	> Ajouter un mot (exit = '0'): 0
	La liste finale contient: 1234,1234,1234,1234,1234
	---

	! phrase actuelle = 'phrase par defaut'
	> voulez-vous la changer (no = '1', exit = '0')? 0
	Done!
	---

	> Ajouter un mot (exit = '0'): 1234
	- Je note 1234!
	. La liste contient maintenant: 1234
	> Ajouter un mot (exit = '0'):	 

En poursuivant avec un mot de 4 caractères supplémentaires, le programme ne boucle plus. On peut rentrer jusqu'à 40 mots de 4 caractères, soir **200 caractères** en tout avec le '\0', sans perturber le programme (en apparence du moins, car le programme se termine en fait en erreur).

	$ ./bof
	> Ajouter un mot (exit = '0'): 1234
	- Je note 1234!
	. La liste contient maintenant: 1234
	> Ajouter un mot (exit = '0'): 1234
	- Je note 1234!
	. La liste contient maintenant: 1234,1234
	> Ajouter un mot (exit = '0'): 1234
	- Je note 1234!
	. La liste contient maintenant: 1234,1234,1234
	> Ajouter un mot (exit = '0'): 1234
	- Je note 1234!
	. La liste contient maintenant: 1234,1234,1234,1234
	> Ajouter un mot (exit = '0'): 1234
	- Je note 1234!
	. La liste contient maintenant: 1234,1234,1234,1234,1234
	> Ajouter un mot (exit = '0'): 1234
	- Je note 1234!
	. La liste contient maintenant: 1234,1234,1234,1234,1234,1234
	> Ajouter un mot (exit = '0'): 0
	La liste finale contient: 1234,1234,1234,1234,1234,1234
	---

	! phrase actuelle = 'phrase par defaut'
	> voulez-vous la changer (no = '1', exit = '0')? 0
	Done!
	---
    $ 
	
</details>

### Question 3: En ne rentrant que des mots de 4 caractères maximum, à partir de quelle longueur de phrase, celle-ci n'est pas intégralement copiée avec _overW()_? Pourquoi?

<details>
<summary>Solution</summary>

<p></p>

```
WARNING: testé sous Windows 11/x86_64 avec mingw, vous pouvez obtenir un comportement légèrement différent sur une autre plateforme.
```

On passe la première partie qui demande de rentrer les mots avec un mot='0', puis on teste les dépassements de capacités sur la déclaration _char outs[20]_ de la fonction _testOverRW()_ qui teste les fonctions et _overW()_ et _overR()_.

La sortie ci-dessous montre ce qu'il se passe en rentrant 20 caractères et donc **21 caractères** avec le '\0'. Le programme se comporte normalement.

	$ ./bof
	> Ajouter un mot (exit = '0'): 0
	La liste finale contient:
	---
	
	! phrase actuelle = 'phrase par defaut'
	> voulez-vous la changer (no = '1', exit = '0')? 2
	> Saisir une phrase a copier: 12345678901234567890
	. La chaine a lire fait 20 caracteres
	. La chaine a ecrire attend 19 caracteres
	. La chaine ecrite contient: 12345678901234567890
	> Saisis le nombre de lettres a afficher (exit = '0'): 20
	12345678901234567890
	! phrase actuelle = '12345678901234567890'

Le résultat est le même avec **31 caractères**, '\0' inclus: le programme se comporte toujours normalement.

	$ ./bof
	> Ajouter un mot (exit = '0'): 0
	La liste finale contient:
	---

	! phrase actuelle = 'phrase par defaut'
	> voulez-vous la changer (no = '1', exit = '0')? 2
	> Saisir une phrase a copier: 123456789012345678901234567890
	. La chaine a lire fait 30 caracteres
	. La chaine a ecrire attend 19 caracteres
	. La chaine ecrite contient: 123456789012345678901234567890
	> Saisis le nombre de lettres a afficher (exit = '0'): 30
	123456789012345678901234567890
	! phrase actuelle = '123456789012345678901234567890'	

Avec **41 caractères**, '\0' inclus, le programme ne retient plus que 39 caractères sur la chaîne entrée, donc 38 sans le '\0'. Puis le programme s'arrête en erreur, ce qui veut dire que nous avons écrasé en mémoire des informations importantes pour l'exécution normale.

	$ ./bof
	> Ajouter un mot (exit = '0'): 0
	La liste finale contient:
	---
	
	> voulez-vous la changer (no = '1', exit = '0')? 2
	> Saisir une phrase a copier: 1234567890123456789012345678901234567890
	. La chaine a lire fait 38 caracteres
	. La chaine a ecrire attend 19 caracteres
	. La chaine ecrite contient: 12345678901234567890123456789012345678
	> Saisis le nombre de lettres a afficher (exit = '0'): Done!
	---
	$
	
On peut trouver le "point de rupture" du flux d'instructions en testant plusieurs valeurs entre 30 et 40 caratères. Il s'avère que celui-ci se trouve à 39 caractères entrés, et donc **40 caractères** stockés avec le '\0'.

	$ ./bof
	> Ajouter un mot (exit = '0'): 0
	La liste finale contient:
	---
	
	> voulez-vous la changer (no = '1', exit = '0')? 2
	> Saisir une phrase a copier: 123456789012345678901234567890123456789
	. La chaine a lire fait 38 caracteres
	. La chaine a ecrire attend 19 caracteres
	. La chaine ecrite contient: 12345678901234567890123456789012345678
	> Saisis le nombre de lettres a afficher (exit = '0'): 39
	12345678901234567890123456789012345678
	! phrase actuelle = '12345678901234567890123456789012345678'

</details>

### Question 4: Qu'arrivez vous à lire avec _overR()_ en demandant d'afficher une sous-chaîne au delà de la chaîne donnée en input? Comment l'expliquez vous?

<details>
<summary>Solution</summary>

<p></p>

```
WARNING: testé sous Windows 11/x86_64 avec mingw, vous pouvez obtenir un comportement légèrement différent sur une autre plateforme.
```

La fonction _testOverRW()_ invoque la fonction _overR()_ en lui passant la chaîne _char inps[20]_. On peut essayer de lire plus de 20 caractères pour essayer de lire au delà de l'espace alloué pour _inps[20]_. 

On notera que la fonction **_overR()_ affiche ce qu'elle lit caractère par caractère**, ce qui fait qu'elle ne sera pas arrêtée par un '\0' de fin de chaîne.

Dans le premier extrait ci-après, on lit **30 caractères**, ce qui ne dévoile rien de suprenant. Probablement qu'il n'y a tout simplement pas de caractère imprimable à afficher dans cet espace.

	$ ./bof
	> Ajouter un mot (exit = '0'): 0
	La liste finale contient:
	---

	! phrase actuelle = 'phrase par defaut'
	> voulez-vous la changer (no = '1', exit = '0')? 1
	. La chaine a lire fait 17 caracteres
	. La chaine a ecrire attend 19 caracteres
	. La chaine ecrite contient: phrase par defaut
	> Saisis le nombre de lettres a afficher (exit = '0'): 30
	phrase par defaut

Il en va de même avec **40 caractères**.

	! phrase actuelle = 'phrase par defaut'
	> voulez-vous la changer (no = '1', exit = '0')? 1
	. La chaine a lire fait 17 caracteres
	. La chaine a ecrire attend 19 caracteres
	. La chaine ecrite contient: phrase par defaut
	> Saisis le nombre de lettres a afficher (exit = '0'): 40
	phrase par defaut

Cela change avec **50 caractères** mais sans vraiment dévoiler quoi que ce soit d'intéressant.

	! phrase actuelle = 'phrase par defaut'
	> voulez-vous la changer (no = '1', exit = '0')? 1
	. La chaine a lire fait 17 caracteres
	. La chaine a ecrire attend 19 caracteres
	. La chaine ecrite contient: phrase par defaut
	> Saisis le nombre de lettres a afficher (exit = '0'): 50
	phrase par defautùÇ∞≈ce

La situation devient plus intéressante avec **60 caractères**. On voit apparaître la chaîne "ceci est un" qui pourrait être le contenu de l'une des variables locales de _testOverRW()_: _char sec2[20] = "ceci est un secret"_ ou _char sec1[20] = "ceci est un ******"_.

	! phrase actuelle = 'phrase par defaut'
	> voulez-vous la changer (no = '1', exit = '0')? 1
	. La chaine a lire fait 17 caracteres
	. La chaine a ecrire attend 19 caracteres
	. La chaine ecrite contient: phrase par defaut
	> Saisis le nombre de lettres a afficher (exit = '0'): 60
	phrase par defautùÇ∞≈ceci est un

Avec **70 caractères**, on voit que c'est le contenu de _sec2[]_ que l'on arrive à lire, ce qui veut dire que le variable sec2[] est stockés juste après _inps[]_ en mémoire!

	! phrase actuelle = 'phrase par defaut'
	> voulez-vous la changer (no = '1', exit = '0')? 1
	. La chaine a lire fait 17 caracteres
	. La chaine a ecrire attend 19 caracteres
	. La chaine ecrite contient: phrase par defaut
	> Saisis le nombre de lettres a afficher (exit = '0'): 70
	phrase par defautùÇ∞≈ceci est un secretΓ

On le confirme en lisant **100 caractères** et on voit apparaître le contenu de _inps[]_ un deuxième fois. C'est en fait le contenu de l'autre variable locale _outs[]_ dans laquelle _inps[]_ a été copié par _overW()_. Celle-ci est donc stockée immédiatement après _sec2[]_ en mémoire, et on voit la logique d'organisation de la mémoire qui emmerge. On peut s'attendre à lire le contenu des autres variables locales déclarées avant _outs[]_ en lisant plus de caractères. 

	! phrase actuelle = 'phrase par defaut'
	> voulez-vous la changer (no = '1', exit = '0')? 1
	. La chaine a lire fait 17 caracteres
	. La chaine a ecrire attend 19 caracteres
	. La chaine ecrite contient: phrase par defaut
	> Saisis le nombre de lettres a afficher (exit = '0'): 100
	phrase par defautùÇ∞≈ceci est un secretΓ▌v]·phrase par defaut

Effectivement, on arrive à lire les variables locales suivantes _sec3[]_ et _sec4[]_ en lisant **200 caractères**.

	! phrase actuelle = 'phrase par defaut'
	> voulez-vous la changer (no = '1', exit = '0')? 1
	. La chaine a lire fait 17 caracteres
	. La chaine a ecrire attend 19 caracteres
	. La chaine ecrite contient: phrase par defaut
	> Saisis le nombre de lettres a afficher (exit = '0'): 200
	phrase par defautùÇ∞≈ceci est un secretΓ▌v]·phrase par defaut=δÇ∞≈CECI EST UN SECRET≈K■ ΓC3C1 3ST UN S3CR3TÉ■ ΓÇ∞≈Éü∞≈

Pour terminer, on peut tenter de lire **1'000 caractères**. Dans ce cas, on provoque une erreur et le crash du programme. On a vraisemblablement tenter de lire au delà de l'espace alloué au processus et l'OS l'a interdit puis a terminé le programme.

	! phrase actuelle = 'phrase par defaut'
	> voulez-vous la changer (no = '1', exit = '0')? 1
	. La chaine a lire fait 17 caracteres
	. La chaine a ecrire attend 19 caracteres
	. La chaine ecrite contient: phrase par defaut
	> Saisis le nombre de lettres a afficher (exit = '0'): 1000
	phrase par defautùÇ∞≈ceci est un secretΓ▌v]·phrase par defaut=δÇ∞≈CECI EST UN SECRET≈K■ ΓC3C1 3ST UN S3CR3TÉ■ ΓÇ∞≈Éü∞≈Ç■ ΓP
	7=└7=εÇ∞≈;Ç∞≈}%∞\·X¬╨^·0√  ≡0√  ╨!
	$

</details>

