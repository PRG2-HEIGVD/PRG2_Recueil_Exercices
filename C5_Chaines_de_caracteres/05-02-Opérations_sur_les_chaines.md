# Opérations sur les chaines
<details>
<summary>Source</summary>
(https://stackoverflow.com/questions/7459259/inserting-characters-into-a-string)
</details>

## Exercice 1
Remplacer la fonction _insChar()_ de l'exercice 1 de la série 05-01-Conversions par une fonction _insertStr()_ qui insert toute une chaîne de caractères au lieu d'un caractère unique. 

Contrairement à insChar(), insertStr()_ doit être implémentée en utilisant les fonctions de manipulations des chaines de caractères définies dans le header _string.h_.

<details>
<summary>Solution</summary>

~~~

// insert string ins at position pos into string str
// the resulting string cannot exceed len characters
// len is assumed to be compatible with str buffer
int insertStr(char *str, int len, const char *ins, int pos) {
    int lstr = strlen(str), lins = strlen(ins);

    if ((lstr + lins) > len) return -1; // no more place in string
    if ((pos < 0) || (pos > len)) return -1; // position out of string

    char *s = (char *)calloc(len, sizeof(char));
    strcpy(s, str+pos); // backup end of string from position pos
    strcpy(str+pos, ins); // append added string at position pos
    strcpy(str+pos+lins, s); // append end of string
    free(s);
    return 0;
}

~~~

Insertion d'un caractère avec _insertStr()_ dans le _main()_ de 05-01-Conversions.

~~~

if (insertStr(res, len + ns + 1, "\'", i + ns) < 0) {  // take into account added separators

~~~
</details>
