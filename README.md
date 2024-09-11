#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define SIZE 4 // 4x4 grid

void initializeGrid(int grid[SIZE][SIZE]);
void printGrid(int grid[SIZE][SIZE]);
int isSolved(int grid[SIZE][SIZE]);
void swap(int *a, int *b);
int moveTile(int grid[SIZE][SIZE], char move);

int main() {
    int grid[SIZE][SIZE];
    char move;
    
    initializeGrid(grid);

    printf("Welcome to the 15 Puzzle Game!\n");
    printf("Arrange the numbers from 1 to 15 by moving tiles.\n");
    printf("Use W (up), A (left), S (down), D (right) to move tiles.\n\n");

    while (!isSolved(grid)) {
        printGrid(grid);
        printf("Enter your move: ");
        scanf(" %c", &move);
        if (!moveTile(grid, move)) {
            printf("Invalid move. Try again.\n");
        }
    }

    printf("\nCongratulations! You solved the puzzle!\n");
    return 0;
}

// Function to initialize the grid with numbers 1 to 15 and 0 as the empty space
void initializeGrid(int grid[SIZE][SIZE]) {
    int numbers[SIZE * SIZE], i, j;
    srand(time(NULL));

    // Generate a shuffled list of numbers from 0 to 15
    for (i = 0; i < SIZE * SIZE; i++) {
        numbers[i] = i;
    }
    
    for (i = SIZE * SIZE - 1; i > 0; i--) {
        int j = rand() % (i + 1);
        swap(&numbers[i], &numbers[j]);
    }

    // Fill the grid with shuffled numbers
    for (i = 0; i < SIZE; i++) {
        for (j = 0; j < SIZE; j++) {
            grid[i][j] = numbers[i * SIZE + j];
        }
    }
}

// Function to print the current grid state
void printGrid(int grid[SIZE][SIZE]) {
    printf("\n");
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            if (grid[i][j] == 0) {
                printf("   "); // Empty space
            } else {
                printf("%2d ", grid[i][j]);
            }
        }
        printf("\n");
    }
    printf("\n");
}

// Function to check if the puzzle is solved
int isSolved(int grid[SIZE][SIZE]) {
    int correctNumber = 1;
    
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            if (grid[i][j] != 0 && grid[i][j] != correctNumber) {
                return 0; // Not solved
            }
            correctNumber++;
        }
    }
    
    return 1; // Solved
}

// Function to swap two numbers
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

// Function to move a tile in the given direction
int moveTile(int grid[SIZE][SIZE], char move) {
    int emptyRow, emptyCol;

    // Find the empty space (represented by 0)
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            if (grid[i][j] == 0) {
                emptyRow = i;
                emptyCol = j;
                break;
            }
        }
    }

    // Move the empty space based on user input
    switch (move) {
        case 'w': // Move up
        case 'W':
            if (emptyRow < SIZE - 1) {
                swap(&grid[emptyRow][emptyCol], &grid[emptyRow + 1][emptyCol]);
                return 1;
            }
            break;
        case 'a': // Move left
        case 'A':
            if (emptyCol < SIZE - 1) {
                swap(&grid[emptyRow][emptyCol], &grid[emptyRow][emptyCol + 1]);
                return 1;
            }
            break;
        case 's': // Move down
        case 'S':
            if (emptyRow > 0) {
                swap(&grid[emptyRow][emptyCol], &grid[emptyRow - 1][emptyCol]);
                return 1;
            }
            break;
        case 'd': // Move right
        case 'D':
            if (emptyCol > 0) {
                swap(&grid[emptyRow][emptyCol], &grid[emptyRow][emptyCol - 1]);
                return 1;
            }
            break;
        default:
            return 0;
    }

    return 0; // Invalid move
}
