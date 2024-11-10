#include <stdio.h>
#include <stdlib.h>

struct stack {
    int size;
    int top;
    int *arr;
};

void create(struct stack *ptr) {
    ptr->size = 10;
    ptr->top = -1;
    ptr->arr = (int*)malloc(ptr->size * sizeof(int));
}

int isfull(struct stack *ptr) {
    return ptr->top == ptr->size - 1;
}

int isempty(struct stack *ptr) {
    return ptr->top == -1;
}

void push(struct stack *ptr, int val) {
    if (isfull(ptr)) {
        printf("Stack is full\n");
    } else {
        ptr->top++;
        ptr->arr[ptr->top] = val;
    }
}

int pop(struct stack *ptr) {
    if (isempty(ptr)) {
        printf("Stack is empty\n");
        return -1;
    } else {
        int x = ptr->arr[ptr->top];
        ptr->top--;
        return x;
    }
}

int peek(struct stack *ptr) {
    if (isempty(ptr)) {
        printf("Stack is empty\n");
        return -1;
    } else {
        return ptr->arr[ptr->top];
    }
}

void display(struct stack *ptr) {
    if (isempty(ptr)) {
        printf("Stack is empty\n");
    } else {
        printf("Stack elements: ");
        for (int i = 0; i <= ptr->top; i++) {
            printf("%d ", ptr->arr[i]);
        }
        printf("\n");
    }
}

// Recursive function to insert an element in sorted order in the stack
void insertInSortedOrder(struct stack *ptr, int element) {
    if (isempty(ptr) || peek(ptr) <= element) {
        push(ptr, element);
        return;
    }
    int topElement = pop(ptr);
    insertInSortedOrder(ptr, element);
    push(ptr, topElement);
}

// Recursive function to sort the stack
void sortStack(struct stack *ptr) {
    if (!isempty(ptr)) {
        int topElement = pop(ptr);
        sortStack(ptr);
        insertInSortedOrder(ptr, topElement);
    }
}

// Function to find the maximum element
int findMax(struct stack *ptr) {
    if (isempty(ptr)) {
        printf("Stack is empty\n");
        return -1;
    } else {
        int max = ptr->arr[0];
        for (int i = 1; i <= ptr->top; i++) {
            if (ptr->arr[i] > max) {
                max = ptr->arr[i];
            }
        }
        printf("Max element: %d\n", max);
        return max;
    }
}

// Function to find the minimum element
int findMin(struct stack *ptr) {
    if (isempty(ptr)) {
        printf("Stack is empty\n");
        return -1;
    } else {
        int min = ptr->arr[0];
        for (int i = 1; i <= ptr->top; i++) {
            if (ptr->arr[i] < min) {
                min = ptr->arr[i];
            }
        }
        printf("Min element: %d\n", min);
        return min;
    }
}

// Function to find the middle element
int findMiddle(struct stack *ptr) {
    if (isempty(ptr)) {
        printf("Stack is empty\n");
        return -1;
    } else {
        int midIndex = ptr->top / 2;
        printf("Middle element: %d\n", ptr->arr[midIndex]);
        return ptr->arr[midIndex];
    }
}

// Function to find the middle element using another stack
int findMiddleUsingStack(struct stack *ptr) {
    if (isempty(ptr)) {
        printf("Stack is empty\n");
        return -1;
    } else {
        struct stack temp;
        create(&temp);
        
        int middleIndex = ptr->top / 2;
        int middleElement;
        
        for (int i = 0; i < middleIndex; i++) {
            push(&temp, pop(ptr));
        }
        
        middleElement = pop(ptr);
        printf("Middle element: %d\n", middleElement);
        
        while (!isempty(&temp)) {
            push(ptr, pop(&temp));
        }
        free(temp.arr);
        return middleElement;
    }
}

int main() {
    struct stack ptr;
    create(&ptr);

    int ch, val;
    while (1) {
        printf("\n--- Stack Menu ---\n");
        printf("1. Push\n");
        printf("2. Pop\n");
        printf("3. Peek\n");
        printf("4. Sort Stack using Recursion\n");
        printf("5. Display\n");
        printf("6. Find Middle Element\n");
        printf("7. Find Max Element\n");
        printf("8. Find Min Element\n");
        printf("9. Find Middle Element Using Another Stack\n");
        printf("10. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &ch);

        switch (ch) {
            case 1:
                printf("Enter the value to push: ");
                scanf("%d", &val);
                push(&ptr, val);
                break;

            case 2:
                pop(&ptr);
                break;

            case 3:
                printf("Top element: %d\n", peek(&ptr));
                break;

            case 4:
                sortStack(&ptr);
                printf("Stack is sorted using recursion\n");
                break;

            case 5:
                display(&ptr);
                break;
                
            case 6:
                findMiddle(&ptr);
                break;
                 
            case 7:
                findMax(&ptr);
                break;
                 
            case 8:
                findMin(&ptr);
                break;
            
            case 9:
                findMiddleUsingStack(&ptr);
                break;
                
            case 10:
                free(ptr.arr);
                exit(0);

            default:
                printf("Invalid choice\n");
        }
    }

    return 0;
}
