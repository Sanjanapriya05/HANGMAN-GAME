# HANGMAN-GAME

HANGMAN is basically a word guessing game where a player attempts to build a missing word by guessing one letter at a time.
The code is a console-based implementation of the classic Hangman game.
It begins by prompting the user to select a category (Fruits or Animals) and then randomly selects a word from the chosen category.
The user is then prompted to guess letters in the word. The user has five tries to guess the word, and can also ask for a clue.
clearInputBuffer() : 
   This function clears the input buffer to prevent invalid input from previous inputs.
displayHangman() :
    This function displays the current state of the hangman based on the number of tries remaining.
shuffleWords() :
    This function shuffles the words in the chosen category to ensure a random word is selected each time the game is played.
playHangman() : 
    This function first selects the word list based on the chosen category and shuffles the words. 
It then prompts the user to guess letters in the word. If the user enters "clue", the function provides a hint
by revealing one of the letters in the word. The user has only one clue. . If the user enters a new letter, the function 
checks if the letter is in the word. If the letter is in the word, the function reveals the letter and informs the user 
that the guess was correct. If the letter is not in the word, the function informs the user that the guess was 
incorrect and decrements the number of tries remaining. The game continues until the user has guessed the word or has 
used up all their tries.
