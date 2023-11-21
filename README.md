#include <stdio.h>
#include <stdlib.h>

// Node structure
struct Node {
    int data;
    struct Node* next;
};

// Function to insert a node at the end of the circular linked list
void insertNode(struct Node** head, int value) {
    // Create a new node
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->data = value;

    // If the list is empty, make the new node the only node
    if (*head == NULL) {
        *head = newNode;
        newNode->next = *head;
    } else {
        // Find the last node and update pointers
        struct Node* last = *head;
        while (last->next != *head) {
            last = last->next;
        }

        last->next = newNode;
        newNode->next = *head;
    }
}

// Function to delete a node with a given value from the circular linked list
void deleteNode(struct Node** head, int key) {
    if (*head == NULL) {
        printf("List is empty, cannot delete.\n");
        return;
    }

    struct Node* current = *head, *prev = NULL;

    // If the node to be deleted is the only node
    if (current->data == key && current->next == *head) {
        free(current);
        *head = NULL;
        return;
    }

    // Search for the node to be deleted
    while (current->data != key) {
        if (current->next == *head) {
            printf("Node with value %d not found.\n", key);
            return;
        }
        prev = current;
        current = current->next;
    }

    // Adjust pointers to delete the node
    if (current == *head) {
        prev = *head;
        while (prev->next != *head) {
            prev = prev->next;
        }
        *head = current->next;
        prev->next = *head;
    } else {
        prev->next = current->next;
    }

    free(current);
    printf("Node with value %d deleted successfully.\n", key);
}

// Function to display the circular linked list
void displayList(struct Node* head) {
    if (head == NULL) {
        printf("List is empty.\n");
        return;
    }

    struct Node* temp = head;
    do {
        printf("%d ", temp->data);
        temp = temp->next;
    } while (temp != head);

    printf("\n");
}

// Main function
int main() {
    struct Node* head = NULL;

    // Insert nodes
    insertNode(&head, 1);
    insertNode(&head, 2);
    insertNode(&head, 3);

    // Display the initial list
    printf("Initial Circular Linked List: ");
    displayList(head);

    // Delete a node
    deleteNode(&head, 2);

    // Display the updated list
    printf("Updated Circular Linked List: ");
    displayList(head);

    return 0;
}
