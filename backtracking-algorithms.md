Below are three challenging problems along with a brief explanation, an interview‐style question, and complete C solutions with inline comments that explain the code.

---

## 1. N-Queens Problem

**Explanation:**  
The N-Queens problem asks you to place N queens on an N×N chessboard so that no two queens attack each other (i.e. no two queens share the same row, column, or diagonal). A typical solution uses backtracking to try placing a queen in every row and backtrack when a conflict is found.

**Interview Question:**  
*Write a C program to solve the N-Queens problem and print all valid arrangements.*

**C Solution:**
```c
#include <stdio.h>
#include <stdbool.h>
#define N 8  // Change N to solve for a different board size

// Function to print a board configuration
void printSolution(int board[N][N]) {
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++)
            printf("%c ", board[i][j] ? 'Q' : '.');
        printf("\n");
    }
    printf("\n");
}

// Function to check if it's safe to place a queen at board[row][col]
bool isSafe(int board[N][N], int row, int col) {
    int i, j;
    // Check this column on upper rows
    for (i = 0; i < row; i++)
        if (board[i][col])
            return false;

    // Check upper left diagonal
    for (i = row, j = col; i >= 0 && j >= 0; i--, j--)
        if (board[i][j])
            return false;

    // Check upper right diagonal
    for (i = row, j = col; i >= 0 && j < N; i--, j++)
        if (board[i][j])
            return false;

    return true;
}

// Recursive function to solve the N-Queens problem
void solveNQueensUtil(int board[N][N], int row) {
    // Base case: If all queens are placed, print the solution
    if (row == N) {
        printSolution(board);
        return;
    }

    // Try placing a queen in each column of the current row
    for (int col = 0; col < N; col++) {
        if (isSafe(board, row, col)) {
            // Place the queen
            board[row][col] = 1;
            // Recur to place the rest of the queens
            solveNQueensUtil(board, row + 1);
            // Backtrack: Remove the queen from board[row][col]
            board[row][col] = 0;
        }
    }
}

int main() {
    int board[N][N] = { {0} };  // Initialize all cells to 0 (empty)
    printf("All solutions to the %d-Queens Problem:\n\n", N);
    solveNQueensUtil(board, 0);
    return 0;
}
```

---

## 2. Sudoku Solver

**Explanation:**  
A Sudoku puzzle is a 9×9 grid that must be filled so that each row, each column, and each of the 3×3 sub-grids contain all digits from 1 to 9. The solver uses backtracking by trying numbers in empty cells and recursing until a valid solution is found.

**Interview Question:**  
*Write a C program to solve a given 9x9 Sudoku puzzle using backtracking.*

**C Solution:**
```c
#include <stdio.h>
#include <stdbool.h>
#define N 9

// Function to print the Sudoku board
void printBoard(int board[N][N]) {
    for (int row = 0; row < N; row++) {
        for (int col = 0; col < N; col++)
            printf("%d ", board[row][col]);
        printf("\n");
    }
}

// Check if 'num' is not already placed in the current row
bool usedInRow(int board[N][N], int row, int num) {
    for (int col = 0; col < N; col++)
        if (board[row][col] == num)
            return true;
    return false;
}

// Check if 'num' is not already placed in the current column
bool usedInCol(int board[N][N], int col, int num) {
    for (int row = 0; row < N; row++)
        if (board[row][col] == num)
            return true;
    return false;
}

// Check if 'num' is not already placed in the 3x3 box starting at (boxStartRow, boxStartCol)
bool usedInBox(int board[N][N], int boxStartRow, int boxStartCol, int num) {
    for (int row = 0; row < 3; row++)
        for (int col = 0; col < 3; col++)
            if (board[row + boxStartRow][col + boxStartCol] == num)
                return true;
    return false;
}

// Check if it is safe to place 'num' at board[row][col]
bool isSafe(int board[N][N], int row, int col, int num) {
    return !usedInRow(board, row, num) &&
           !usedInCol(board, col, num) &&
           !usedInBox(board, row - row % 3, col - col % 3, num);
}

// Recursive function to solve Sudoku
bool solveSudoku(int board[N][N]) {
    int row, col;
    bool emptyFound = false;

    // Find an empty cell (denoted by 0)
    for (row = 0; row < N; row++) {
        for (col = 0; col < N; col++) {
            if (board[row][col] == 0) {
                emptyFound = true;
                goto FIND_EMPTY_EXIT; // Break out of nested loops
            }
        }
    }
FIND_EMPTY_EXIT:
    // If no empty cell is found, the board is complete
    if (!emptyFound)
        return true;

    // Try numbers 1 to 9 in the empty cell
    for (int num = 1; num <= 9; num++) {
        if (isSafe(board, row, col, num)) {
            board[row][col] = num;  // Tentatively assign num

            // Recur to check if this leads to a solution
            if (solveSudoku(board))
                return true;

            // Backtrack if assignment of num doesn't lead to a solution
            board[row][col] = 0;
        }
    }
    return false; // Trigger backtracking
}

int main() {
    // Example Sudoku board with 0 denoting empty cells
    int board[N][N] = {
        {5,3,0, 0,7,0, 0,0,0},
        {6,0,0, 1,9,5, 0,0,0},
        {0,9,8, 0,0,0, 0,6,0},
        
        {8,0,0, 0,6,0, 0,0,3},
        {4,0,0, 8,0,3, 0,0,1},
        {7,0,0, 0,2,0, 0,0,6},
        
        {0,6,0, 0,0,0, 2,8,0},
        {0,0,0, 4,1,9, 0,0,5},
        {0,0,0, 0,8,0, 0,7,9}
    };
    
    if (solveSudoku(board)) {
        printf("Sudoku solved successfully:\n");
        printBoard(board);
    } else {
        printf("No solution exists for the given Sudoku.\n");
    }
    return 0;
}
```

---

## 3. Generating Permutations and Combinations

### A. Generating Permutations

**Explanation:**  
Generating permutations involves rearranging the elements of an array in every possible order. A common method is to use recursion with swapping. At each recursion level, you swap the current element with each element that comes after it and then recurse.

**Interview Question:**  
*Write a C program to generate all permutations of a given array of integers.*

**C Solution (Permutations):**
```c
#include <stdio.h>

// Utility function to swap two elements in an array
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

// Function to print an array (one permutation)
void printArray(int arr[], int n) {
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    printf("\n");
}

// Recursive function to generate permutations
void permute(int arr[], int l, int r) {
    // Base case: if l == r, a permutation is formed
    if (l == r) {
        printArray(arr, r + 1);
        return;
    }
    
    // Generate permutations by swapping each element with the element at position l
    for (int i = l; i <= r; i++) {
        swap(&arr[l], &arr[i]);         // Swap current element with element at position l
        permute(arr, l + 1, r);           // Recurse for the next position
        swap(&arr[l], &arr[i]);         // Backtrack to restore the original array
    }
}

int main() {
    int arr[] = {1, 2, 3};
    int n = sizeof(arr) / sizeof(arr[0]);
    printf("All permutations of the array are:\n");
    permute(arr, 0, n - 1);
    return 0;
}
```

### B. Generating Combinations

**Explanation:**  
Generating combinations means selecting a subset of items from a larger set without regard to order. The recursive approach builds the combination one element at a time and uses backtracking to explore all possibilities.

**Interview Question:**  
*Write a C program to generate all combinations of r elements from a given array of integers.*

**C Solution (Combinations):**
```c
#include <stdio.h>

// Recursive function to generate all combinations of size r from the array
// arr: input array, data: temporary array to store current combination,
// start: starting index in arr, index: current index in data, r: size of combination, n: size of arr
void combinationUtil(int arr[], int n, int r, int index, int data[], int start) {
    // If the combination is ready, print it
    if (index == r) {
        for (int j = 0; j < r; j++)
            printf("%d ", data[j]);
        printf("\n");
        return;
    }
    
    // Replace index with all possible elements. The condition "n - i >= r - index"
    // ensures that there are enough remaining elements to fill the combination.
    for (int i = start; i < n && (n - i) >= (r - index); i++) {
        data[index] = arr[i];  // Include current element
        combinationUtil(arr, n, r, index + 1, data, i + 1);  // Recurse for next index
    }
}

int main() {
    int arr[] = {1, 2, 3, 4};
    int n = sizeof(arr) / sizeof(arr[0]);
    int r = 2;  // Size of each combination
    int data[r];  // Temporary array to store current combination

    printf("All combinations of size %d from the array are:\n", r);
    combinationUtil(arr, n, r, 0, data, 0);
    return 0;
}
```

---
