# Liste FIFO avec liste simplement chainee

Ecrivez le fichier `queue.c` de sorte qu'avec les fichiers `main.c` et `queue.h` suivants 
le programme affiche 

~~~
0 1 4 9 16
5 elements remain in queue, from 25 to 81.
~~~

<details>
<summary>main.c</summary>

~~~cpp 
#include <stdio.h>
#include "queue.h"

int main() {
    void* q = new_queue();

    for(int i = 0; i < 10; ++i) {
        push_in_queue(q, i*i);
        if(i % 2 == 0) { 
            printf("%d ",front_of_queue(q));
            pop_from_queue(q);
        }
    }

    printf("\n%d elements remain in queue, from %d to %d.\n",
           size_of_queue(q),
           front_of_queue(q), 
           back_of_queue(q));

    free_queue(q);
}
~~~

</details>

<details>
<summary>queue.h</summary>

~~~cpp 
#ifndef QUEUE_H
#define QUEUE_H

void* new_queue();
void free_queue(void*);

void push_in_queue(void*, int i);
void pop_from_queue(void*);
int front_of_queue(void*);
int back_of_queue(void*);
int size_of_queue(void*);

#endif
~~~

</details>

La solution demandée utilise une liste simplement chainée dont 
on se souvient des premier et dernier maillons. La fonction
`push_in_queue` effectue un `push_back`, i.e. insère en fin de 
liste, tandis que la fonction `pop_from_queue` effectue un 
`pop_front`, i.e. supprime la tête de liste.

<details>
<summary>Solution : queue.c</summary>

~~~cpp 
#include "queue.h"
#include <stdlib.h>
#include <assert.h>

struct node {
    struct node *nxt;
    int val;
};

struct queue {
    struct node *front;
    struct node *back; 
    int size;
};

void* new_queue() {
    struct queue *q = malloc(sizeof(struct queue));
    q->front = q->back = NULL;
    q->size = 0;
    return q;
}

void free_queue(void *_q) {
    while(size_of_queue(_q)) 
        pop_from_queue(_q);
    free(_q);
}

void push_in_queue(void *_q, int i) {
    // push_back()
    struct queue* q = (struct queue*)_q;
    struct node* n = malloc(sizeof(struct node));
    n->nxt = NULL;
    n->val = i;
    if(q->front == NULL) 
        q->front = n;
    if(q->back != NULL) 
        q->back->nxt = n;
    q->back = n;
    q->size++;
}

void pop_from_queue(void *_q) {
    // pop_front()
    assert(size_of_queue(_q));
    struct queue* q = (struct queue*)_q;
    struct node* n = q->front;
    q->front = q->front->nxt;
    q->size--;
    if(q->size == 0) 
        q->back = NULL; 
    free(n); 
}
 
int front_of_queue(void *_q) {
    assert(size_of_queue(_q));
    struct queue* q = (struct queue*)_q;
    return q->front->val;
}

int back_of_queue(void *_q) {
    assert(size_of_queue(_q));
    struct queue* q = (struct queue*)_q;
    return q->back->val;
}

int size_of_queue(void *_q) {
    struct queue* q = (struct queue*)_q;
    return q ? q->size : 0;
}
~~~

</details>

