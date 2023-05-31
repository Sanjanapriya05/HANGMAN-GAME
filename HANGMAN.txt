#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define MAX_TRIES 5
#define MAX_WORD_LENGTH 20

void clearInputBuffer() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF) {
        // Clear input buffer
    }
}

void displayHangman(int numTries) {
    printf("\n");
    switch (numTries) {
        case 1:
            printf("  ___\n");
            printf(" |   |\n");
            printf(" |\n");
            printf(" |\n");
            printf(" |\n");
            printf(" |\n");
            printf("_|_\n");
            break;
        case 2:
            printf("  ___\n");
            printf(" |   |\n");
            printf(" |   O\n");
            printf(" |\n");
            printf(" |\n");
            printf(" |\n");
            printf("_|_\n");
            break;
        case 3:
            printf("  ___\n");
            printf(" |   |\n");
            printf(" |   O\n");
            printf(" |   |\n");
            printf(" |\n");
            printf(" |\n");
            printf("_|_\n");
            break;
        case 4:
            printf("  ___\n");
            printf(" |   |\n");
            printf(" |   O\n");
            printf(" |  /|\\\n");
            printf(" |\n");
            printf(" |\n");
            printf("_|_\n");
            break;
        case 5:
            printf("  ___\n");
            printf(" |   |\n");
            printf(" |   O\n");
            printf(" |  /|\\\n");
            printf(" |  / \\\n");
            printf(" |\n");
            printf("_|_\n");
            break;
       
        default:
            printf("\n");
            break;
    }
}

void shuffleWords(char wordList[][MAX_WORD_LENGTH], int numWords) {
    srand(time(NULL));
    for (int i = numWords - 1; i > 0; i--) {
        int j = rand() % (i + 1);
        char temp[MAX_WORD_LENGTH];
        strcpy(temp, wordList[i]);
        strcpy(wordList[i], wordList[j]);
        strcpy(wordList[j], temp);
    }
}

void playHangman(char *category) {
    char FruitsWordList[][MAX_WORD_LENGTH] = {
        "apple",
        "banana",
        "cherry",
        "watermelon",
        "mango",
        "guava",
        "grape",
        "papaya",
        "orange",
        "strawberry",
        "pomegranite",
        "kiwi"
    };
    char AnimalsWordList[][MAX_WORD_LENGTH] = {
        "cat",
        "pig",
        "dog",
        "lion",
        "tiger",
        "horse",
        "rabbit",
        "giraffe",
        "sheep",
        "donkey",
        "bear",
        "elephant"
    };

    char *chosenCategory = "";
    char wordList[MAX_WORD_LENGTH][MAX_WORD_LENGTH];
    int numWords = 0;

    if (strcmp(category, "1") == 0) {
        chosenCategory = "Fruits";
        numWords = sizeof(FruitsWordList) / sizeof(FruitsWordList[0]);
        memcpy(wordList, FruitsWordList, sizeof(FruitsWordList));
    } else if (strcmp(category, "2") == 0) {
        chosenCategory = "Animals";
        numWords = sizeof(AnimalsWordList) / sizeof(AnimalsWordList[0]);
        memcpy(wordList,AnimalsWordList, sizeof(AnimalsWordList));
    }

    shuffleWords(wordList, numWords);

    printf("\n--- %s Category ---\n", chosenCategory);

    srand(time(NULL));
    int randomIndex = rand() % numWords;
    char *word = wordList[randomIndex];

    int wordLength = strlen(word);
    int numTries = 0;
    int numCorrectGuesses = 0;
    int found = 0;
    char guessedLetters[MAX_WORD_LENGTH] = {0};
    int clueUsed = 0;

    printf("Welcome to Hangman!\n");

    while (numTries < MAX_TRIES && numCorrectGuesses < wordLength) {
        displayHangman(numTries);

        printf("\nWord: ");
        for (int i = 0; i < wordLength; i++) {
            if (guessedLetters[i]) {
                printf("%c ", word[i]);
            } else {
                printf("_ ");
            }
        }

        printf("\nGuess a letter or type 'clue' for a hint (you have 1 clue): ");
        char input[10];
        scanf("%s", input);
        clearInputBuffer();

        if (strcmp(input, "clue") == 0) {
            if (clueUsed) {
                printf("You have already used your clue!\n");
                continue;
            }

            int randomPos;
            do {
                randomPos = rand() % wordLength;
            } while (guessedLetters[randomPos]);

            guessedLetters[randomPos] = 1;
            numCorrectGuesses++;
            clueUsed = 1;
            printf("Here's a clue: the letter at position %d is '%c'\n", randomPos + 1, word[randomPos]);
            continue;
        }

        if (strchr(guessedLetters, input[0])) {
            printf("You already guessed that letter!\n");
            continue;
        }

        int correct = 0;
        for (int i = 0; i < wordLength; i++) {
            if (word[i] == input[0]) {
                guessedLetters[i] = 1;
                correct = 1;
                numCorrectGuesses++;
            }
        }

        if (correct) {
            printf("Correct guess!\n");
        } else {
            printf("Incorrect guess!\n");
            numTries++;
        }
    }

    displayHangman(numTries);

    printf("\n");

    if (numCorrectGuesses == wordLength) {
        printf("Congratulations! You guessed the word: %s\n", word);
    } else {
        printf("You lost! The word was: %s\n", word);
    }
}

int main() {
    char category[2];

    printf("Welcome to Hangman!\n");
    printf("Select a category:\n");
    printf("1. Fruits\n");
    printf("2. Animals\n");
    printf("Enter the category number: ");
    scanf("%s", category);
    clearInputBuffer();

    playHangman(category);

    return 0;
}