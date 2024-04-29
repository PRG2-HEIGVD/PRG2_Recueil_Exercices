# Fichiers texte
<details>
<summary>Sources</summary>
https://enit.rnu.tn/fr/Minds/informatique/TD5%20avec%20corrig%C3%A9.pdf
</details>

## Exercice 1
Soit un fichier de données structuré en une suite de lignes contenant chacune un nom de marque, une désignation d’article, un nombre d'articles en stock, un prix unitaire et un prix pour l'ensemble. 

Exemple _stock.txt_ :
~~~
Federer raquette 999.90 10 9999
Odermatt skis 567.50 20 11350
~~~

Ecrire un programme qui accepte un nom de fichier texte en argument et qui lit ce fichier ligne par ligne pour en extraire le contenu. Le contenu lu est affiché ligne par ligne, chaque élément séparé par des virgules, et les prix affichés au centime.

Exemple d'affichage pour _stock.txt_ :
~~~
importing Federer,raquette,999.90,10,9999.00
importing Odermatt,skis,567.50,20,11350.00
~~~

<details>
<summary>Solution</summary>

~~~cpp
#include <stdio.h>

#define MAXLEN 256

int main(int argc, char **argv) {

    if (argc != 2) {
        printf("Usage: %s file\n", argv[0]);
        return 1;
    }

    FILE *fin = fopen(argv[1], "r");
    if (!fin) {
        printf("[e] could not open %s\n", argv[1]);
        return 1;
    }

    // start reading

    char brand[MAXLEN]; char item[MAXLEN];
    float unit, total;
    int qty;

    while (fscanf(fin, "%s %s %f %d %f", brand, item, &unit, &qty, &total) != EOF)
        printf("reading %s,%s,%.2f,%d,%.2f\n", brand, item, unit, qty, total);

    fclose(fin);
    return 0;
}
~~~
</details>


## Exercice 2
Compléter le programme de l'exercice précédent pour qu'il importe le contenu lu dans des enregistrements en mémoire puis sauvegarde ces derniers dans un fichier binaire qui fera lieu de base de données.

Le contenu est chargé en mémoire dans un tableau de structures _stockItem_ définies ainsi:

~~~cpp
struct stockItem {
    char brand[MAXLEN];
    char item[MAXLEN];
    int qty;
    float unit, total;
};
~~~

Le fichier binaire produit contient tous les enregistrements présents en mémoire ainsi que le nombre de ces enregistrements au début du fichier.

<details>
<summary>Solution</summary>

~~~cpp
#include <stdio.h>

#define MAXLEN 256
#define MAXSTOCK 80

struct stockItem {
    char brand[MAXLEN];
    char item[MAXLEN];
    int qty;
    float unit, total;
};

int main(int argc, char **argv) {

    if (argc != 3) {
        printf("Usage: %s importfile dbfile\n", argv[0]);
        return 1;
    }

    FILE *fin = fopen(argv[1], "r");
    if (!fin) {
        printf("[e] could not open %s\n", argv[1]);
        return 1;
    }

    // start importing

    struct stockItem stock[MAXSTOCK] = { 0 };
    int i = 0;
    while (fscanf(fin, "%s %s %f %d %f", stock[i].brand, stock[i].item, &(stock[i].unit), &(stock[i].qty), &(stock[i].total)) != EOF) {
        printf("importing %s,%s,%.2f,%d,%.2f\n", stock[i].brand, stock[i].item, stock[i].unit, stock[i].qty, stock[i].total);
        i++;
    }

    fclose(fin);

    // create/overwrite DB file
	
    FILE *fout = fopen(argv[2], "wb");
    if (!fout) {
        printf("[e] could not open %s\n", argv[2]);
        return 1;
    }

    // DB size as header
	
    if (fwrite(&i, sizeof i, 1, fout) != 1) {
        printf("[e] could not write size of stock to %s\n", argv[2]);
        return 1;
    }
    
	// DB rows
	
	if (fwrite(stock, sizeof stock[0], i, fout) != i) {
        printf("[e] could not write stock to %s\n", argv[2]);
        return 1;
    }

    fclose(fout);
    return 0;
}

~~~

</details>


## Exercice 3
Ecrivez le programme qui importe le fichier binaire produit lors de l'exercice précédent en mémoire puis affiche le contenu à l'écran au format:

~~~
[i] The stock contains 2 products:
[i] - row 0: [Federer],[raquette],[999.90],[10],[9999.00]
[i] - row 1: [Odermatt],[skis],[567.50],[20],[11350.00]
~~~

NB: Le fichier binaire est chargé en mémoire dans un tableau de structures _stockItem_ telles que définies dans l'exercice 2.

<details>
<summary>Solution</summary>

~~~cpp
#include <stdio.h>

#define MAXLEN 256
#define MAXSTOCK 80

struct stockItem {
    char brand[MAXLEN];
    char item[MAXLEN];
    int qty;
    float unit, total;
};

int main(int argc, char **argv) {

    if (argc != 2) {
        printf("Usage: %s dbfile\n", argv[0]);
        return 1;
    }

    FILE *fin = fopen(argv[1], "rb");
    if (!fin) {
        printf("[e] could not open %s\n", argv[1]);
        return 1;
    }

    // load DB into memory

    struct stockItem stock[MAXSTOCK] = { 0 };
    int n = 0;

    // get DB size (number of rows)

    if (fread(&n, sizeof n, 1, fin) != 1) {
        printf("[e] error trying to read db size\n");
        return 1;
    }

    // get DB rows

    if (fread(stock, sizeof stock[0], n, fin) != n) {
        printf("[e] error trying to read %d db contents\n", n);
        return 1;
    }

    // display DB contents

    printf("[i] The stock contains %d products:\n", n);
    for (int i = 0; i < n; i++) {
        printf("[i] - row %d: ", i);
        printf("[%s],[%s],", stock[i].brand, stock[i].item);
        printf("[%.2f],[%d],[%.2f]\n", stock[i].unit, stock[i].qty, stock[i].total);
    }

    fclose(fin);
    return 0;
}
~~~

</details>
