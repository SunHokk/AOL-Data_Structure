#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

#define MAX_CHAR 26
#define MAX_LEN 105

typedef struct TrieNode {
    struct TrieNode* children[MAX_CHAR];
    char* description;
    int isEndOfWord;
} TrieNode;

TrieNode* createNode() {
    TrieNode* newNode = (TrieNode*)malloc(sizeof(TrieNode));
    newNode->isEndOfWord = 0;
    newNode->description = NULL;
    for (int i = 0; i < MAX_CHAR; i++) newNode->children[i] = NULL;
    return newNode;
}

int isValidWord(char* word) {
    if (strlen(word) <= 1 || strchr(word, ' ')) return 0;
    return 1;
}

int wordCount(char* sentence) {
    int count = 0, inWord = 0;
    for (int i = 0; sentence[i]; i++) {
        if (!isspace(sentence[i]) && !inWord) {
            inWord = 1;
            count++;
        } else if (isspace(sentence[i])) {
            inWord = 0;
        }
    }
    return count;
}

void insert(TrieNode* root, char* word, char* desc) {
    TrieNode* curr = root;
    for (int i = 0; word[i]; i++) {
        int idx = tolower(word[i]) - 'a';
        if (!curr->children[idx])
            curr->children[idx] = createNode();
        curr = curr->children[idx];
    }

    curr->isEndOfWord = 1;

    if (curr->description)
        free(curr->description);

    curr->description = (char*)malloc(strlen(desc) + 1);
    strcpy(curr->description, desc);
}

TrieNode* search(TrieNode* root, char* word) {
    TrieNode* curr = root;
    for (int i = 0; word[i]; i++) {
        int idx = tolower(word[i]) - 'a';
        if (!curr->children[idx]) return NULL;
        curr = curr->children[idx];
    }
    return (curr && curr->isEndOfWord) ? curr : NULL;
}

void printAll(TrieNode* root, char* prefix, int level, int* count) {
    if (root->isEndOfWord) {
        prefix[level] = '\0';
        printf("%d. %s\n", ++(*count), prefix);
    }

    for (int i = 0; i < MAX_CHAR; i++) {
        if (root->children[i]) {
            prefix[level] = 'a' + i;
            printAll(root->children[i], prefix, level + 1, count);
        }
    }
}

void printWithPrefix(TrieNode* root, char* prefix) {
    TrieNode* curr = root;
    for (int i = 0; prefix[i]; i++) {
        int idx = tolower(prefix[i]) - 'a';
        if (!curr->children[idx]) {
            printf("There is no prefix \"%s\" in the dictionary.\n", prefix);
            return;
        }
        curr = curr->children[idx];
    }

    int count = 0;
    char buffer[MAX_LEN];
    strcpy(buffer, prefix);
    printAll(curr, buffer, strlen(prefix), &count);

    if (count == 0)
        printf("There is no prefix \"%s\" in the dictionary.\n", prefix);
}

void releaseSlang(TrieNode* root) {
    char word[MAX_LEN], desc[MAX_LEN];
    do {
        printf("Input a new slang word [Must be more than 1 characters and contains no space]: ");
        scanf(" %[^\n]", word);
    } while (!isValidWord(word));

    do {
        printf("Input a new slang word description [Must be more than 2 words]: ");
        scanf(" %[^\n]", desc);
    } while (wordCount(desc) < 2);

    TrieNode* node = search(root, word);
    insert(root, word, desc);

    if (node)
        printf("\nSuccessfully updated a slang word.\n");

    printf("Press enter to continue...");
    getchar(); getchar();
}

void searchSlang(TrieNode* root) {
    char word[MAX_LEN];
    do {
        printf("Input a slang word to be searched [Must be more than 1 characters and contains no space]: ");
        scanf(" %[^\n]", word);
    } while (!isValidWord(word));

    TrieNode* node = search(root, word);
    if (node) {
        printf("\nSlang word  : %s\n", word);
        printf("Description : %s\n", node->description);
    } else {
        printf("\nThere is no word \"%s\" in the dictionary.", word);
    }
    printf("\nPress enter to continue...");
    getchar(); getchar();
}

void viewAllWords(TrieNode* root) {
    int count = 0;
    char buffer[MAX_LEN];
    printAll(root, buffer, 0, &count);
    if (count == 0) {
        printf("There is no slang word yet in the dictionary.");
    }else{
        printf("List of all slang words in the dictionary:\n");
    }
    printf("\nPress enter to continue...");
    getchar(); getchar();
}

void viewPrefixWords(TrieNode* root) {
    char prefix[MAX_LEN];
    printf("Input a prefix to be searched: ");
    scanf(" %[^\n]", prefix);
    printf("\n");

    printWithPrefix(root, prefix);

    printf("Press enter to continue...");
    getchar(); getchar();
}

void menu() {
    printf("Boogle Slang Dictionary\n");
    printf("========================\n");
    printf("1. Release a new slang word\n");
    printf("2. Search a slang word\n");
    printf("3. View all slang words starting with a certain prefix word\n");
    printf("4. View all slang words\n");
    printf("5. Exit\n");
    printf("Choose >> ");
}

int main() {
    TrieNode* root = createNode();
    int choice;

    do {
        system("clear || cls");
        menu();
        scanf("%d", &choice);
        printf("\n");
        switch (choice) {
            case 1:
                releaseSlang(root);
                break;
            case 2:
                searchSlang(root);
                break;
            case 3:
                viewPrefixWords(root);
                break;
            case 4:
                viewAllWords(root);
                break;
            case 5:
                printf("Thank you... Have a nice day :)\n");
                break;
            default:
                printf("Invalid option.\n");
                break;
        }
    } while (choice != 5);

    return 0;
}
