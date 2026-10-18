#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX 100

typedef struct {
    char id[10];
    char name[50];
    char serviceType[20];
    int arrivalTime;
    int processingTime;
    int waitingWeight;
} Customer;

typedef struct Node {
    Customer data;
    struct Node *next;
} Node;

typedef struct {
    Node *front;
    Node *rear;
    int size;
} Queue;

typedef struct {
    char type[10];
    Customer oldData;
    Customer newData;
} Action;

typedef struct {
    Action items[MAX];
    int top;
} Stack;

/* ========= QUEUE ========= */

void initQueue(Queue *q) {
    q->front = q->rear = NULL;
    q->size = 0;
}

int isEmptyQueue(Queue *q) {
    return q->front == NULL;
}

Node* createNode(Customer c) {
    Node *p = (Node*)malloc(sizeof(Node));
    p->data = c;
    p->next = NULL;
    return p;
}

void enqueue(Queue *q, Customer c) {
    Node *p = createNode(c);

    if(isEmptyQueue(q)) {
        q->front = q->rear = p;
    }
    else {
        q->rear->next = p;
        q->rear = p;
    }

    q->size++;
}

Customer dequeue(Queue *q) {

    Customer empty = {"","","",0,0,0};

    if(isEmptyQueue(q)) {
        printf("Queue rong!\n");
        return empty;
    }

    Node *tmp = q->front;
    Customer c = tmp->data;

    q->front = q->front->next;

    if(q->front == NULL)
        q->rear = NULL;

    free(tmp);
    q->size--;

    return c;
}

void displayQueue(Queue *q) {
    Node *p = q->front;

    printf("\nQueue: ");

    if(p == NULL) {
        printf("Rong\n");
        return;
    }

    while(p) {
        printf("%s", p->data.id);

        if(p->next)
            printf(" -> ");

        p = p->next;
    }

    printf("\n");
}

/* ========= STACK ========= */

void initStack(Stack *s) {
    s->top = -1;
}

int isEmptyStack(Stack *s) {
    return s->top == -1;
}

void push(Stack *s, Action a) {

    if(s->top == MAX - 1)
        return;

    s->items[++s->top] = a;
}

Action pop(Stack *s) {

    Action temp;
    strcpy(temp.type, "");

    if(isEmptyStack(s))
        return temp;

    return s->items[s->top--];
}

/* ========= CUSTOMER LIST ========= */

Customer customers[MAX];
int customerCount = 0;

void addCustomer(Customer c) {
    customers[customerCount++] = c;
}

int findCustomer(char id[]) {

    for(int i = 0; i < customerCount; i++) {
        if(strcmp(customers[i].id, id) == 0)
            return i;
    }

    return -1;
}

void deleteCustomer(char id[]) {

    int pos = findCustomer(id);

    if(pos == -1)
        return;

    for(int i = pos; i < customerCount - 1; i++) {
        customers[i] = customers[i+1];
    }

    customerCount--;
}

void updateCustomer(Customer c) {

    int pos = findCustomer(c.id);

    if(pos != -1)
        customers[pos] = c;
}

void printCustomers() {

    printf("\n===== DANH SACH 
