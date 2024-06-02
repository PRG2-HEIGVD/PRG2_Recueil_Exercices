
# Utilisation de typeof

Écrivez un programme en C qui définit une macro ``calculate_average()`` pour calculer la moyenne de plusieurs tableaux de différents types. 

Vous devez ensuite créer au moins trois tableaux de types différents (par exemple, ``int``, ``float``, et ``double``) et chaque tableau doit contenir **au moins 5 éléments**.

<details>
<summary>Solution</summary>

```cpp

#include <stdio.h>

// Définition d'une macro pour calculer la moyenne d'un tableau
#define calculate_average(arr, n) ({ \
    typeof(arr[0]) sum = 0; \
    for (int i = 0; i < n; ++i) { \
        sum += arr[i]; \
    } \
    sum / n; \
})

int main() {
   
    int int_array[] = {4, 8, 15, 16, 23};
    float float_array[] = {3.14, 2.71, 1.41, 1.73, 2.58};
    double double_array[] = {6.022, 9.81, 3.1415, 2.7182, 1.4142};

    int int_average;
    float float_average;
    double double_average;
    int array_size;

    array_size = sizeof(int_array) / sizeof(int_array[0]);
    int_average = calculate_average(int_array, array_size);
    
    printf("Moyenne du tableau d'entiers : %d\n", int_average);

    array_size = sizeof(float_array) / sizeof(float_array[0]);
    float_average = calculate_average(float_array, array_size);
    
    printf("Moyenne du tableau de flottants : %f\n", float_average);

    array_size = sizeof(double_array) / sizeof(double_array[0]);
    double_average = calculate_average(double_array, array_size);
    
    printf("Moyenne du tableau de doubles : %f\n", double_average);

    return 0;
}

```
