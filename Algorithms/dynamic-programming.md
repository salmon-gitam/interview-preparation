Below are six common dynamic programming (DP) problems along with a brief explanation, an interview‐style question, and a complete C solution with inline comments that explain each step.

---

## 1. Fibonacci Sequence (DP Example)

**Explanation:**  
The Fibonacci sequence is defined such that each number is the sum of the two preceding ones. Using DP (bottom-up), we store intermediate results in an array to avoid recomputation.

**Interview Question:**  
*Write a C program to compute the nth Fibonacci number using dynamic programming.*

**C Solution:**
```c
#include <stdio.h>

// Function to compute the nth Fibonacci number using DP (bottom-up approach)
int fibonacci(int n) {
    // If n is 0 or 1, return n directly
    if(n <= 1)
        return n;
        
    // Create an array to store Fibonacci numbers up to n
    int dp[n+1];
    dp[0] = 0;  // Base case: Fibonacci(0) = 0
    dp[1] = 1;  // Base case: Fibonacci(1) = 1

    // Build the DP table from 2 to n
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2];  // Sum of two previous Fibonacci numbers
    }
    
    // Return the nth Fibonacci number
    return dp[n];
}

int main() {
    int n = 10;  // Example: find the 10th Fibonacci number
    printf("Fibonacci number at position %d is %d\n", n, fibonacci(n));
    return 0;
}
```

---

## 2. Longest Common Subsequence (LCS)

**Explanation:**  
LCS finds the longest subsequence common to two sequences. The DP solution builds a table where each cell represents the length of the LCS of the two substrings ending at those positions.

**Interview Question:**  
*Write a C program to compute the length of the longest common subsequence (LCS) between two strings.*

**C Solution:**
```c
#include <stdio.h>
#include <string.h>

// Function to compute the length of the LCS of strings X and Y
int lcs(char *X, char *Y) {
    int m = strlen(X);
    int n = strlen(Y);
    // Create a DP table of size (m+1) x (n+1)
    int dp[m+1][n+1];

    // Build the table in bottom-up manner
    for (int i = 0; i <= m; i++) {
        for (int j = 0; j <= n; j++) {
            // If one of the strings is empty, LCS is 0
            if (i == 0 || j == 0)
                dp[i][j] = 0;
            // If characters match, add 1 to the result from previous indices
            else if (X[i-1] == Y[j-1])
                dp[i][j] = dp[i-1][j-1] + 1;
            // Otherwise, take the maximum from either dropping one character from X or Y
            else
                dp[i][j] = (dp[i-1][j] > dp[i][j-1]) ? dp[i-1][j] : dp[i][j-1];
        }
    }
    // The bottom-right cell contains the length of LCS for X and Y
    return dp[m][n];
}

int main() {
    char X[] = "AGGTAB";
    char Y[] = "GXTXAYB";
    printf("Length of LCS is %d\n", lcs(X, Y));
    return 0;
}
```

---

## 3. Longest Increasing Subsequence (LIS)

**Explanation:**  
The Longest Increasing Subsequence problem finds the length of the longest subsequence of an array where the elements are in sorted (increasing) order. The DP approach uses an array where each element stores the length of the LIS ending at that index.

**Interview Question:**  
*Write a C program to compute the length of the longest increasing subsequence (LIS) in an array.*

**C Solution:**
```c
#include <stdio.h>

// Function to find the length of the longest increasing subsequence in an array
int lis(int arr[], int n) {
    // dp[i] stores the length of the LIS ending at index i
    int dp[n];
    
    // Every element is at least an LIS of length 1
    for (int i = 0; i < n; i++)
        dp[i] = 1;
    
    // Build the dp array by comparing each pair of elements
    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++) {
            // If arr[i] is greater than arr[j] and extending the subsequence ending at j gives a longer sequence
            if (arr[i] > arr[j] && dp[i] < dp[j] + 1)
                dp[i] = dp[j] + 1;
        }
    }
    
    // Find the maximum value in dp[] which is the length of the LIS
    int max = dp[0];
    for (int i = 1; i < n; i++) {
        if (dp[i] > max)
            max = dp[i];
    }
    return max;
}

int main() {
    int arr[] = {10, 22, 9, 33, 21, 50, 41, 60};
    int n = sizeof(arr) / sizeof(arr[0]);
    printf("Length of Longest Increasing Subsequence is %d\n", lis(arr, n));
    return 0;
}
```

---

## 4. Knapsack Problem

### a. 0/1 Knapsack

**Explanation:**  
In the 0/1 Knapsack problem, each item can be included entirely or excluded. A DP table is built where each entry represents the maximum value achievable with a given capacity and a subset of items.

**Interview Question:**  
*Write a C program to solve the 0/1 Knapsack problem using dynamic programming.*

**C Solution:**
```c
#include <stdio.h>

// Function to return the maximum of two integers
int max(int a, int b) {
    return (a > b) ? a : b;
}

// Function to solve the 0/1 Knapsack problem using DP
int knapSack(int W, int wt[], int val[], int n) {
    // Create a DP table with dimensions (n+1) x (W+1)
    int dp[n+1][W+1];

    // Build the DP table in bottom-up manner
    for (int i = 0; i <= n; i++) {
        for (int w = 0; w <= W; w++) {
            // Base case: no items or capacity is 0
            if (i == 0 || w == 0)
                dp[i][w] = 0;
            // If the current item's weight is less than or equal to current capacity
            else if (wt[i-1] <= w)
                dp[i][w] = max(val[i-1] + dp[i-1][w - wt[i-1]], dp[i-1][w]);
            else
                dp[i][w] = dp[i-1][w];  // Cannot include the item
        }
    }
    // The bottom-right cell contains the maximum value that can be achieved
    return dp[n][W];
}

int main() {
    int val[] = {60, 100, 120}; // Values of items
    int wt[] = {10, 20, 30};    // Weights of items
    int W = 50;               // Knapsack capacity
    int n = sizeof(val) / sizeof(val[0]);
    printf("Maximum value in 0/1 Knapsack = %d\n", knapSack(W, wt, val, n));
    return 0;
}
```

### b. Fractional Knapsack

**Explanation:**  
In the Fractional Knapsack problem, items can be broken into smaller parts. A greedy algorithm is used by sorting items based on their value-to-weight ratio and then taking as much of the highest ratio item as possible until the knapsack is full.

**Interview Question:**  
*Write a C program to solve the Fractional Knapsack problem using a greedy approach.*

**C Solution:**
```c
#include <stdio.h>
#include <stdlib.h>

// Structure to represent an item with weight and value
typedef struct {
    int weight;
    int value;
} Item;

// Comparison function for qsort to sort items by descending value/weight ratio
int compare(const void* a, const void* b) {
    Item* itemA = (Item*) a;
    Item* itemB = (Item*) b;
    double r1 = (double)itemA->value / itemA->weight;
    double r2 = (double)itemB->value / itemB->weight;
    return (r2 > r1) - (r2 < r1);
}

// Function to solve the Fractional Knapsack problem using a greedy approach
double fractionalKnapsack(int W, Item items[], int n) {
    // Sort items based on value-to-weight ratio in descending order
    qsort(items, n, sizeof(Item), compare);
    
    double totalValue = 0.0;
    // Process items in sorted order
    for (int i = 0; i < n; i++) {
        if (W == 0)
            break;  // Knapsack is full
        // If the entire item can be included, do so
        if (items[i].weight <= W) {
            totalValue += items[i].value;
            W -= items[i].weight;
        } else {
            // Include fractional part of the item
            totalValue += items[i].value * ((double)W / items[i].weight);
            W = 0;
        }
    }
    return totalValue;
}

int main() {
    Item items[] = { {10, 60}, {20, 100}, {30, 120} };
    int n = sizeof(items) / sizeof(items[0]);
    int W = 50;  // Knapsack capacity
    printf("Maximum value in Fractional Knapsack = %.2f\n", fractionalKnapsack(W, items, n));
    return 0;
}
```

---

## 5. Coin Change Problem

**Explanation:**  
This problem asks for the minimum number of coins required to make a given amount using coins of specified denominations. The DP solution uses a table where each entry represents the minimum coins needed for that amount.

**Interview Question:**  
*Write a C program to compute the minimum number of coins required to make change for a given amount using dynamic programming.*

**C Solution:**
```c
#include <stdio.h>
#include <limits.h>

// Function to compute the minimum number of coins required for amount V
int coinChange(int coins[], int n, int V) {
    int dp[V+1];
    
    // Initialize the dp array with a value greater than any possible answer
    for (int i = 0; i <= V; i++) {
        dp[i] = V + 1;
    }
    dp[0] = 0;  // Zero coins are needed to make amount 0
    
    // Compute minimum coins for all amounts from 1 to V
    for (int i = 1; i <= V; i++) {
        for (int j = 0; j < n; j++) {
            if (coins[j] <= i) {
                // If including coin[j] leads to a solution with fewer coins, update dp[i]
                if (dp[i - coins[j]] + 1 < dp[i])
                    dp[i] = dp[i - coins[j]] + 1;
            }
        }
    }
    
    // If dp[V] is still greater than V, change cannot be made
    return dp[V] > V ? -1 : dp[V];
}

int main() {
    int coins[] = {1, 2, 5};
    int n = sizeof(coins) / sizeof(coins[0]);
    int V = 11;  // Target amount
    int result = coinChange(coins, n, V);
    
    if(result != -1)
        printf("Minimum number of coins required: %d\n", result);
    else
        printf("Change cannot be made for the given amount.\n");
    
    return 0;
}
```

---

## 6. Edit Distance (Levenshtein Distance)

**Explanation:**  
Edit Distance (Levenshtein Distance) measures the minimum number of operations (insertions, deletions, substitutions) required to convert one string into another. A DP table is built where each cell represents the edit distance between prefixes of the strings.

**Interview Question:**  
*Write a C program to compute the edit distance between two strings using dynamic programming.*

**C Solution:**
```c
#include <stdio.h>
#include <string.h>

// Utility function to find the minimum of three integers
int min3(int a, int b, int c) {
    int min = a;
    if (b < min) min = b;
    if (c < min) min = c;
    return min;
}

// Function to compute the edit distance between strings X and Y using DP
int editDistance(char *X, char *Y) {
    int m = strlen(X);
    int n = strlen(Y);
    
    // Create a DP table of size (m+1) x (n+1)
    int dp[m+1][n+1];
    
    // Initialize the DP table
    for (int i = 0; i <= m; i++) {
        for (int j = 0; j <= n; j++) {
            if (i == 0)
                dp[i][j] = j;  // If X is empty, insert all characters of Y
            else if (j == 0)
                dp[i][j] = i;  // If Y is empty, delete all characters of X
            else if (X[i-1] == Y[j-1])
                dp[i][j] = dp[i-1][j-1];  // Characters match; no operation needed
            else
                dp[i][j] = 1 + min3(dp[i-1][j],    // Deletion
                                    dp[i][j-1],    // Insertion
                                    dp[i-1][j-1]); // Substitution
        }
    }
    return dp[m][n];
}

int main() {
    char X[] = "kitten";
    char Y[] = "sitting";
    printf("Edit distance between '%s' and '%s' is %d\n", X, Y, editDistance(X, Y));
    return 0;
}
```

---
