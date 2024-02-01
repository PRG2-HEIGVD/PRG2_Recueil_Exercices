# Notion de boucles - while

Que va afficher à l'exécution chacun des groupes d'instructions ci-dessous ?

```cpp
//1
int i = 10;
while (i-- > 0) {
    printf("%d ", i);
}
```

<details>
<summary>Solution</summary>

9 8 7 6 5 4 3 2 1 0

</details>


```cpp
//2
int i = 1;
while (i <= 8) {
    printf("%d ", i);
    i *= 2;
}
```

<details>
<summary>Solution</summary>

1 2 4 8

</details>


