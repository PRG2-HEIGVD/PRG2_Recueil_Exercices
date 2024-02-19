# Introduction au langage C

1. Qu'est-ce une chaîne d'outil (ou *toolchain*) ?
2. Quel est le rôle du compilateur C (détaillez)
3. Quel est le premier argument reçu dans la fonction *main()* ?

Que contient en général une *toolchain* ?

<details>
<summary>Solution</summary>

1. Qu'est-ce une chaîne d'outil (ou *toolchain*) ?

Il s'agit de l'ensemble d'applications/utilitaires nécessaire à la compilation d'un programme
et à la production d'une exécutable

La *toolchain* contient le préprocesseur. le compilateur, l'assembleur (ou
compilateur d'assemblage), l'éditeur de lien, un *debugger*, etc,

2. Quel est le rôle du compilateur C (détaillez)

Le compilateur traduit le code source en langage machine. Il effectue d'abord
une traduction vers le code assembleur puis vers l'assembleur traduit le code assembleur
vers le code machine

3. Quel est le premier argument reçu dans la fonction *main()* ?

Il s'agit du premier élément (**oken*) récupéré au démarrage de l'application,
i.e. le nom du fichier exécutable.

</details>
