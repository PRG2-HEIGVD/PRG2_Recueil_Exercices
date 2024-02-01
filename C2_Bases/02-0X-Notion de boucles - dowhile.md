# Notion de boucles - do..while

Que va afficher à l'exécution chacun des groupes d'instructions ci-dessous ?

```cpp
//1
int i = 3;
do {
    printf("%d ", i);
    i++;
} while (i <= 6);
```

<details>
<summary>Solution</summary>

3 4 5 6

</details>


```cpp
//2
int i = 5;
do {
    printf("%d ", i);
    i *= i;
} while (i < 25);
```

<details>
<summary>Solution</summary>

5

</details>


