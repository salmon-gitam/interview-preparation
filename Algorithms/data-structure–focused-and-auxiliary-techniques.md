Below are several advanced data‐structure and algorithm problems along with a brief explanation, an interview‐style question, and complete C solutions with inline comments that explain each key step.

---

## 1. Trie (Prefix Tree) Operations

**Explanation:**  
A Trie (or Prefix Tree) is a tree‐like data structure used to store a dynamic set of strings. It supports fast insert and search operations by breaking words into characters and using each character as a path in the tree.

**Interview Question:**  
*Write a C program to implement a Trie that supports insertion of words and searching for a given word.*

**C Solution:**
```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include <string.h>

#define ALPHABET_SIZE 26

// Define a Trie node structure.
typedef struct TrieNode {
    struct TrieNode* children[ALPHABET_SIZE]; // Pointers to child nodes for each letter.
    bool isEndOfWord;                         // Flag to indicate end of a valid word.
} TrieNode;

// Function to create a new Trie node.
TrieNode* getNode() {
    TrieNode* newNode = (TrieNode*) malloc(sizeof(TrieNode));
    newNode->isEndOfWord = false;
    for (int i = 0; i < ALPHABET_SIZE; i++)
        newNode->children[i] = NULL;
    return newNode;
}

// Function to insert a key (word) into the Trie.
void insert(TrieNode* root, const char* key) {
    TrieNode* curr = root;
    while (*key) {
        int index = *key - 'a'; // Map character to index (assumes lowercase letters).
        if (curr->children[index] == NULL)
            curr->children[index] = getNode(); // Create a node if path doesn't exist.
        curr = curr->children[index];
        key++;
    }
    curr->isEndOfWord = true; // Mark the last node as end of word.
}

// Function to search for a key in the Trie. Returns true if the key exists.
bool search(TrieNode* root, const char* key) {
    TrieNode* curr = root;
    while (*key) {
        int index = *key - 'a';
        if (curr->children[index] == NULL)
            return false;
        curr = curr->children[index];
        key++;
    }
    return curr != NULL && curr->isEndOfWord;
}

int main() {
    // Sample words to insert.
    char keys[][8] = {"the", "a", "there", "answer", "any", "by", "bye", "their"};
    int n = sizeof(keys) / sizeof(keys[0]);
    TrieNode* root = getNode();
    
    // Insert keys into the Trie.
    for (int i = 0; i < n; i++)
        insert(root, keys[i]);
    
    // Search for some words.
    printf("Search results in Trie:\n");
    printf("the: %s\n", search(root, "the") ? "Found" : "Not Found");
    printf("these: %s\n", search(root, "these") ? "Found" : "Not Found");
    printf("answer: %s\n", search(root, "answer") ? "Found" : "Not Found");
    
    return 0;
}
```

---

## 2. Union-Find / Disjoint Set Union (DSU)

**Explanation:**  
The DSU data structure efficiently tracks a set of elements partitioned into disjoint subsets. It supports two key operations: *find* (to determine the set of an element) and *union* (to merge two sets). Techniques like path compression and union by rank optimize these operations.

**Interview Question:**  
*Write a C program to implement the Union-Find structure with path compression and union by rank, and then use it to determine if two elements are in the same set.*

**C Solution:**
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    int parent;
    int rank;
} Subset;

// Find function with path compression.
int find(Subset subsets[], int i) {
    if (subsets[i].parent != i)
        subsets[i].parent = find(subsets, subsets[i].parent);
    return subsets[i].parent;
}

// Union function using union by rank.
void unionSets(Subset subsets[], int x, int y) {
    int xroot = find(subsets, x);
    int yroot = find(subsets, y);
    if (xroot == yroot)
        return;
    if (subsets[xroot].rank < subsets[yroot].rank)
        subsets[xroot].parent = yroot;
    else if (subsets[xroot].rank > subsets[yroot].rank)
        subsets[yroot].parent = xroot;
    else {
        subsets[yroot].parent = xroot;
        subsets[xroot].rank++;
    }
}

int main() {
    int n = 5; // Number of elements.
    Subset subsets[n];
    // Initially, each element is its own parent and has rank 0.
    for (int i = 0; i < n; i++) {
        subsets[i].parent = i;
        subsets[i].rank = 0;
    }
    
    // Perform some unions.
    unionSets(subsets, 0, 1);
    unionSets(subsets, 1, 2);
    unionSets(subsets, 3, 4);
    
    // Check connectivity.
    printf("Are 0 and 2 in the same set? %s\n", (find(subsets, 0) == find(subsets, 2)) ? "Yes" : "No");
    printf("Are 0 and 4 in the same set? %s\n", (find(subsets, 0) == find(subsets, 4)) ? "Yes" : "No");
    
    return 0;
}
```

---

## 3. Segment Trees and Fenwick Trees (Binary Indexed Trees)

### A. Segment Tree

**Explanation:**  
A Segment Tree is a tree data structure used to answer range queries (like sum, minimum, maximum) efficiently and also supports point updates. It divides an array into segments and stores aggregate information for each segment.

**Interview Question:**  
*Write a C program to build a Segment Tree for an array, answer range sum queries, and update an element.*

**C Solution (Segment Tree):**
```c
#include <stdio.h>
#include <stdlib.h>

// Build the segment tree recursively.
void buildSegmentTree(int arr[], int segTree[], int low, int high, int pos) {
    if (low == high) {
        segTree[pos] = arr[low];
        return;
    }
    int mid = (low + high) / 2;
    buildSegmentTree(arr, segTree, low, mid, 2 * pos + 1);
    buildSegmentTree(arr, segTree, mid + 1, high, 2 * pos + 2);
    segTree[pos] = segTree[2 * pos + 1] + segTree[2 * pos + 2]; // Store sum of children.
}

// Query the segment tree for the sum in a given range.
int rangeSumQuery(int segTree[], int qlow, int qhigh, int low, int high, int pos) {
    // Total overlap.
    if (qlow <= low && qhigh >= high)
        return segTree[pos];
    // No overlap.
    if (qlow > high || qhigh < low)
        return 0;
    // Partial overlap.
    int mid = (low + high) / 2;
    return rangeSumQuery(segTree, qlow, qhigh, low, mid, 2 * pos + 1) +
           rangeSumQuery(segTree, qlow, qhigh, mid + 1, high, 2 * pos + 2);
}

// Update an element and all affected nodes in the segment tree.
void updateSegmentTree(int segTree[], int index, int diff, int low, int high, int pos) {
    if (index < low || index > high)
        return;
    segTree[pos] += diff;
    if (low != high) {
        int mid = (low + high) / 2;
        updateSegmentTree(segTree, index, diff, low, mid, 2 * pos + 1);
        updateSegmentTree(segTree, index, diff, mid + 1, high, 2 * pos + 2);
    }
}

int main() {
    int arr[] = {1, 3, 5, 7, 9, 11};
    int n = sizeof(arr) / sizeof(arr[0]);
    int size = 4 * n;  // Sufficient size for segment tree.
    int *segTree = (int*) malloc(size * sizeof(int));
    
    buildSegmentTree(arr, segTree, 0, n - 1, 0);
    printf("Sum of values in range [1, 3]: %d\n", rangeSumQuery(segTree, 1, 3, 0, n - 1, 0));
    
    // Update: Change arr[1] from 3 to 10 (diff = 7).
    int diff = 10 - arr[1];
    arr[1] = 10;
    updateSegmentTree(segTree, 1, diff, 0, n - 1, 0);
    printf("After update, sum of values in range [1, 3]: %d\n", rangeSumQuery(segTree, 1, 3, 0, n - 1, 0));
    
    free(segTree);
    return 0;
}
```

### B. Fenwick Tree (Binary Indexed Tree)

**Explanation:**  
A Fenwick Tree (or BIT) is a data structure that provides efficient methods for cumulative frequency queries and updates. It works with 1-indexed arrays and uses bit manipulation to traverse the tree.

**Interview Question:**  
*Write a C program to construct a Fenwick Tree for an array, perform prefix sum queries, and update an element.*

**C Solution (Fenwick Tree):**
```c
#include <stdio.h>
#include <stdlib.h>

// Update the BIT for index 'index' with value 'val'.
void updateBIT(int BITree[], int n, int index, int val) {
    // BITree uses 1-indexing.
    index = index + 1;
    while (index <= n) {
        BITree[index] += val;
        index += index & (-index); // Move to next index.
    }
}

// Get the prefix sum up to index 'index'.
int getSumBIT(int BITree[], int index) {
    int sum = 0;
    index = index + 1;
    while (index > 0) {
        sum += BITree[index];
        index -= index & (-index);
    }
    return sum;
}

// Construct the BIT from the original array.
int* constructBIT(int arr[], int n) {
    int *BITree = (int*) calloc(n + 1, sizeof(int));
    for (int i = 0; i < n; i++)
        updateBIT(BITree, n, i, arr[i]);
    return BITree;
}

int main() {
    int arr[] = {1, 3, 5, 7, 9, 11};
    int n = sizeof(arr) / sizeof(arr[0]);
    int *BITree = constructBIT(arr, n);
    
    printf("Prefix sum of first 4 elements using BIT: %d\n", getSumBIT(BITree, 3));
    // Update: Increase element at index 3 by 6.
    updateBIT(BITree, n, 3, 6);
    printf("After update, prefix sum of first 4 elements using BIT: %d\n", getSumBIT(BITree, 3));
    
    free(BITree);
    return 0;
}
```

---

## 4. Suffix Trees and Suffix Arrays

**Explanation:**  
A Suffix Array is a space‑efficient alternative to a Suffix Tree. It is an array of starting indices of all suffixes of a given string sorted in lexicographical order.

**Interview Question:**  
*Write a C program to construct a Suffix Array for a given string and print the starting indices of sorted suffixes.*

**C Solution (Suffix Array):**
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

// Structure to hold a suffix and its starting index.
typedef struct {
    int index;
    char *suffix;
} Suffix;

// Comparison function to sort suffixes lexicographically.
int compareSuffix(const void *a, const void *b) {
    Suffix *s1 = (Suffix*) a;
    Suffix *s2 = (Suffix*) b;
    return strcmp(s1->suffix, s2->suffix);
}

// Function to build and print the suffix array for a given string.
void buildSuffixArray(char *txt) {
    int n = strlen(txt);
    Suffix *suffixes = (Suffix*) malloc(n * sizeof(Suffix));
    
    // Create all suffixes.
    for (int i = 0; i < n; i++) {
        suffixes[i].index = i;
        suffixes[i].suffix = txt + i;  // Pointer to suffix starting at index i.
    }
    
    // Sort the suffixes.
    qsort(suffixes, n, sizeof(Suffix), compareSuffix);
    
    printf("Suffix Array for \"%s\":\n", txt);
    for (int i = 0; i < n; i++)
        printf("%d ", suffixes[i].index);
    printf("\n");
    
    free(suffixes);
}

int main() {
    char txt[] = "banana";
    buildSuffixArray(txt);
    return 0;
}
```

---

## 5. Bit Manipulation Techniques

**Explanation:**  
Bit manipulation techniques use bit‑wise operators to perform operations efficiently at the bit level. Common tasks include counting set bits, checking if a number is a power of two, or swapping values without a temporary variable.

**Interview Question:**  
*Write a C program to count the number of set bits in an integer and check if a number is a power of two using bit manipulation.*

**C Solution:**
```c
#include <stdio.h>

// Function to count set bits using Brian Kernighan's algorithm.
int countSetBits(int n) {
    int count = 0;
    while (n) {
        n &= (n - 1);  // Clear the least significant bit set.
        count++;
    }
    return count;
}

// Function to check if a number is a power of two.
int isPowerOfTwo(int n) {
    if (n <= 0) return 0;
    return (n & (n - 1)) == 0;
}

int main() {
    int num = 29;
    printf("Number of set bits in %d: %d\n", num, countSetBits(num));
    num = 16;
    printf("%d is %sa power of 2.\n", num, isPowerOfTwo(num) ? "" : "not ");
    return 0;
}
```

---

## 6. Two-Pointer Technique and Sliding Window Algorithms

### A. Two-Pointer Technique

**Explanation:**  
The two-pointer technique uses two indices to traverse a sorted array to efficiently solve problems such as finding a pair that sums to a target value.

**Interview Question:**  
*Write a C program that uses the two-pointer technique to determine if a sorted array contains a pair of numbers that sum to a given target.*

**C Solution (Two-Pointer):**
```c
#include <stdio.h>
#include <stdbool.h>

// Function to check for a pair with given sum using two-pointer technique.
bool hasPairWithSum(int arr[], int n, int target) {
    int left = 0, right = n - 1;
    while (left < right) {
        int sum = arr[left] + arr[right];
        if (sum == target)
            return true;
        else if (sum < target)
            left++;
        else
            right--;
    }
    return false;
}

int main() {
    int arr[] = {1, 2, 4, 4};
    int n = sizeof(arr) / sizeof(arr[0]);
    int target = 8;
    printf("Pair with sum %d %sfound.\n", target, hasPairWithSum(arr, n, target) ? "" : "not ");
    return 0;
}
```

### B. Sliding Window Algorithm

**Explanation:**  
The sliding window technique efficiently processes a subset (window) of consecutive elements. It is often used to solve problems like finding the maximum sum of a subarray of fixed length.

**Interview Question:**  
*Write a C program to find the maximum sum of any contiguous subarray of size k using the sliding window algorithm.*

**C Solution (Sliding Window):**
```c
#include <stdio.h>
#include <limits.h>

// Function to compute the maximum sum subarray of size k.
int maxSumSubarray(int arr[], int n, int k) {
    if (n < k) return -1;
    int max_sum = 0;
    // Compute sum of first window.
    for (int i = 0; i < k; i++)
        max_sum += arr[i];
    int window_sum = max_sum;
    // Slide the window by one element at a time.
    for (int i = k; i < n; i++) {
        window_sum += arr[i] - arr[i - k];
        if (window_sum > max_sum)
            max_sum = window_sum;
    }
    return max_sum;
}

int main() {
    int arr[] = {1, 4, 2, 10, 23, 3, 1, 0, 20};
    int n = sizeof(arr) / sizeof(arr[0]);
    int k = 4;
    printf("Maximum sum of subarray of size %d is %d.\n", k, maxSumSubarray(arr, n, k));
    return 0;
}
```

---

## 7. Fast Exponentiation (and Modular Exponentiation)

**Explanation:**  
Fast (or binary) exponentiation computes \(x^n\) in O(log n) time by squaring the base and reducing the exponent by half at each step. Modular exponentiation applies the modulus at each multiplication step to keep numbers small.

**Interview Question:**  
*Write a C program to compute x^n using fast exponentiation and then compute x^n mod m using modular exponentiation.*

**C Solution:**
```c
#include <stdio.h>

// Function for fast exponentiation (calculates x^n).
long long fastExponentiation(long long x, long long n) {
    long long result = 1;
    while (n > 0) {
        if (n & 1)
            result *= x;
        x *= x;
        n >>= 1;  // Divide n by 2.
    }
    return result;
}

// Function for modular exponentiation (calculates x^n mod mod).
long long modExponentiation(long long x, long long n, long long mod) {
    long long result = 1;
    x %= mod;  // Update x if it is more than mod.
    while (n > 0) {
        if (n & 1)
            result = (result * x) % mod;
        x = (x * x) % mod;
        n >>= 1;
    }
    return result;
}

int main() {
    long long x = 2, n = 10, mod = 1000000007;
    printf("Fast Exponentiation: %lld^%lld = %lld\n", x, n, fastExponentiation(x, n));
    printf("Modular Exponentiation: %lld^%lld mod %lld = %lld\n", x, n, mod, modExponentiation(x, n, mod));
    return 0;
}
```

---
