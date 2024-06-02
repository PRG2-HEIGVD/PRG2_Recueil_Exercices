
# Itérateur sur une pile de fonctions

Pour cet exercice, il faut reprendre les fonctions décrites dans le chapitre C12
pour la gestion d'une collection de fonctions (fonctions ``func_register()``, ``do_work()``, etc.)

a)Implémentez les fonctions  ``push()``, ``pop()``, ``top()`` et ``empty()``pour la gestion d'une pile de pointeurs sur fonction.

b) Modifiez les fonctions ``func_register()`` et ``do_work()`` afin de gérer la collection de fonctions à l'aide d'une pile de fonctions. Une fois la fonction exécutée, elle sera ôtée de la collection.

<details>
<summary>Solution</summary>

```cpp

#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

typedef void (*fn_t)(void *);

typedef struct {
    fn_t fn;
    void *args;
} entry_t;

typedef struct Node {
    struct Node *next;
    entry_t entry;
} Node;

typedef Node task_collection_t;

void push(Node **topNode, entry_t entry) {
    Node *n = malloc(sizeof(Node));
    if (n) {
        n->next = *topNode; 
        n->entry = entry;
        *topNode = n;
    }   
}

void pop(Node **topNode) {
    Node *tmp = *topNode;

    *topNode = (*topNode)->next;
    free(tmp);
}  

entry_t top(Node *topNode) { 
    return topNode->entry; 
}

bool empty(Node *topNode) {
    return (topNode == NULL);
}

void func_register(task_collection_t **task_collection, fn_t fn, 
                   void *args) {
    entry_t entry;
    entry.fn = fn;
    entry.args = args;

    push(task_collection, entry);
}

void do_work(task_collection_t **task_collection) {
        while (!empty(*task_collection)) {
                top(*task_collection).fn(top(*task_collection).args);
                pop(task_collection);
        }
}

void task_A(void *args) {
    printf("Execution of task A\n");
}

void task_B(void *args) {
    printf("Execution of task B with args: %s\n", (char *) args);
}

int main() {
    task_collection_t *task_collection = NULL;

    func_register(&task_collection, task_A, NULL);
    func_register(&task_collection, task_B, "Hello");

    do_work(&task_collection);
}

```

</details>

