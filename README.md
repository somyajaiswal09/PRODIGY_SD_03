#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_CONTACTS 100
#define MAX_NAME_LEN 50
#define MAX_PHONE_LEN 15
#define MAX_EMAIL_LEN 50
#define FILE_NAME "contacts.txt"

// Define a structure to store contact information
typedef struct {
    char name[MAX_NAME_LEN];
    char phone[MAX_PHONE_LEN];
    char email[MAX_EMAIL_LEN];
} Contact;

// Function declarations
void addContact(Contact contacts[], int *count);
void viewContacts(Contact contacts[], int count);
void editContact(Contact contacts[], int count);
void deleteContact(Contact contacts[], int *count);
void saveContactsToFile(Contact contacts[], int count);
void loadContactsFromFile(Contact contacts[], int *count);

int main() {
    Contact contacts[MAX_CONTACTS];
    int count = 0;
    int choice;

    // Load contacts from file on program startup
    loadContactsFromFile(contacts, &count);

    do {
        printf("\nContact Management System\n");
        printf("1. Add Contact\n");
        printf("2. View Contacts\n");
        printf("3. Edit Contact\n");
        printf("4. Delete Contact\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        getchar();  // Consume the newline character left by scanf

        switch (choice) {
            case 1:
                addContact(contacts, &count);
                break;
            case 2:
                viewContacts(contacts, count);
                break;
            case 3:
                editContact(contacts, count);
                break;
            case 4:
                deleteContact(contacts, &count);
                break;
            case 5:
                saveContactsToFile(contacts, count);
                printf("Exiting program...\n");
                break;
            default:
                printf("Invalid choice. Please try again.\n");
        }
    } while (choice != 5);

    return 0;
}

// Function to add a new contact
void addContact(Contact contacts[], int *count) {
    if (*count >= MAX_CONTACTS) {
        printf("Contact list is full!\n");
        return;
    }

    printf("Enter contact name: ");
    fgets(contacts[*count].name, MAX_NAME_LEN, stdin);
    contacts[*count].name[strcspn(contacts[*count].name, "\n")] = '\0';  // Remove newline

    printf("Enter phone number: ");
    fgets(contacts[*count].phone, MAX_PHONE_LEN, stdin);
    contacts[*count].phone[strcspn(contacts[*count].phone, "\n")] = '\0';  // Remove newline

    printf("Enter email address: ");
    fgets(contacts[*count].email, MAX_EMAIL_LEN, stdin);
    contacts[*count].email[strcspn(contacts[*count].email, "\n")] = '\0';  // Remove newline

    (*count)++;
    printf("Contact added successfully!\n");
}

// Function to view all contacts
void viewContacts(Contact contacts[], int count) {
    if (count == 0) {
        printf("No contacts available.\n");
        return;
    }

    printf("\nContacts List:\n");
    for (int i = 0; i < count; i++) {
        printf("Contact %d:\n", i + 1);
        printf("Name: %s\n", contacts[i].name);
        printf("Phone: %s\n", contacts[i].phone);
        printf("Email: %s\n\n", contacts[i].email);
    }
}

// Function to edit a contact
void editContact(Contact contacts[], int count) {
    int index;
    printf("Enter the contact number to edit (1 to %d): ", count);
    scanf("%d", &index);
    getchar();  // Consume the newline character

    if (index < 1 || index > count) {
        printf("Invalid contact number.\n");
        return;
    }

    index--;  // Adjust index to match the array (0-based)

    printf("Editing Contact %d:\n", index + 1);
    printf("Enter new name (leave empty to keep existing): ");
    fgets(contacts[index].name, MAX_NAME_LEN, stdin);
    if (contacts[index].name[0] != '\n') {
        contacts[index].name[strcspn(contacts[index].name, "\n")] = '\0';  // Remove newline
    }

    printf("Enter new phone number (leave empty to keep existing): ");
    fgets(contacts[index].phone, MAX_PHONE_LEN, stdin);
    if (contacts[index].phone[0] != '\n') {
        contacts[index].phone[strcspn(contacts[index].phone, "\n")] = '\0';  // Remove newline
    }

    printf("Enter new email address (leave empty to keep existing): ");
    fgets(contacts[index].email, MAX_EMAIL_LEN, stdin);
    if (contacts[index].email[0] != '\n') {
        contacts[index].email[strcspn(contacts[index].email, "\n")] = '\0';  // Remove newline
    }

    printf("Contact updated successfully!\n");
}

// Function to delete a contact
void deleteContact(Contact contacts[], int *count) {
    int index;
    printf("Enter the contact number to delete (1 to %d): ", *count);
    scanf("%d", &index);
    getchar();  // Consume the newline character

    if (index < 1 || index > *count) {
        printf("Invalid contact number.\n");
        return;
    }

    index--;  // Adjust index to match the array (0-based)

    // Shift remaining contacts to delete the selected one
    for (int i = index; i < *count - 1; i++) {
        contacts[i] = contacts[i + 1];
    }

    (*count)--;
    printf("Contact deleted successfully!\n");
}

// Function to save contacts to file
void saveContactsToFile(Contact contacts[], int count) {
    FILE *file = fopen(FILE_NAME, "w");
    if (file == NULL) {
        printf("Error opening file for saving contacts.\n");
        return;
    }

    for (int i = 0; i < count; i++) {
        fprintf(file, "%s\n%s\n%s\n", contacts[i].name, contacts[i].phone, contacts[i].email);
    }

    fclose(file);
}

// Function to load contacts from file
void loadContactsFromFile(Contact contacts[], int *count) {
    FILE *file = fopen(FILE_NAME, "r");
    if (file == NULL) {
        printf("No previous contacts found, starting fresh.\n");
        return;
    }

    while (fscanf(file, "%49[^\n]\n%14[^\n]\n%49[^\n]\n", contacts[*count].name, contacts[*count].phone, contacts[*count].email) == 3) {
        (*count)++;
    }

    fclose(file);
}
Footer
© 2025 GitHub, Inc.
Footer navigation
Terms
Privacy
Security
