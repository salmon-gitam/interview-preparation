Below are three problems with a brief explanation, an interview‐style question, and complete C solutions with inline comments.

---

## 1. Activity Selection Problem

**Explanation:**  
The Activity Selection Problem asks for the maximum number of activities that can be performed by a single person, given start and finish times for each activity. The greedy approach sorts activities by finish time and then selects each activity that starts after the previously selected activity finishes.

**Interview Question:**  
*Write a C program to select the maximum number of non-overlapping activities from a given list.*

**C Solution:**
```c
#include <stdio.h>
#include <stdlib.h>

// Structure to store an activity's start and finish times
typedef struct {
    int start;
    int finish;
} Activity;

// Comparison function for qsort to sort activities by finish time
int compareActivities(const void *a, const void *b) {
    Activity *act1 = (Activity *) a;
    Activity *act2 = (Activity *) b;
    return act1->finish - act2->finish;
}

int main() {
    // Example list of activities with start and finish times
    Activity activities[] = { {1, 4}, {3, 5}, {0, 6}, {5, 7}, {8, 9}, {5, 9} };
    int n = sizeof(activities) / sizeof(activities[0]);
    
    // Sort activities by their finish time
    qsort(activities, n, sizeof(Activity), compareActivities);
    
    // The first activity is always selected
    int count = 1;
    int lastFinish = activities[0].finish;
    
    printf("Selected activities:\n");
    printf("Activity 1: Start = %d, Finish = %d\n", activities[0].start, activities[0].finish);
    
    // Choose subsequent activities if they start after the last selected activity finishes
    for (int i = 1; i < n; i++) {
        if (activities[i].start >= lastFinish) {
            count++;
            lastFinish = activities[i].finish;
            printf("Activity %d: Start = %d, Finish = %d\n", count, activities[i].start, activities[i].finish);
        }
    }
    
    printf("Total number of activities selected: %d\n", count);
    return 0;
}
```

---

## 2. Huffman Coding

**Explanation:**  
Huffman Coding is a greedy algorithm used for lossless data compression. It builds a binary tree where each leaf node represents a character and its frequency. Characters with lower frequencies have longer codes, while those with higher frequencies have shorter codes.

**Interview Question:**  
*Write a C program to generate Huffman codes for a set of characters given their frequencies.*

**C Solution:**
```c
#include <stdio.h>
#include <stdlib.h>

// A Huffman tree node
typedef struct MinHeapNode {
    char data;                  // One of the input characters
    unsigned freq;              // Frequency of the character
    struct MinHeapNode *left, *right; // Left and right child
} MinHeapNode;

// A MinHeap: collection of pointers to Huffman tree nodes
typedef struct {
    unsigned size;              // Current size of min heap
    unsigned capacity;          // Capacity of min heap
    MinHeapNode** array;        // Array of minheap node pointers
} MinHeap;

// Function to create a new min heap node
MinHeapNode* newNode(char data, unsigned freq) {
    MinHeapNode* temp = (MinHeapNode*) malloc(sizeof(MinHeapNode));
    temp->data = data;
    temp->freq = freq;
    temp->left = temp->right = NULL;
    return temp;
}

// Function to create a min heap of given capacity
MinHeap* createMinHeap(unsigned capacity) {
    MinHeap* minHeap = (MinHeap*) malloc(sizeof(MinHeap));
    minHeap->size = 0;
    minHeap->capacity = capacity;
    minHeap->array = (MinHeapNode**) malloc(minHeap->capacity * sizeof(MinHeapNode*));
    return minHeap;
}

// Utility function to swap two min heap nodes
void swapMinHeapNode(MinHeapNode** a, MinHeapNode** b) {
    MinHeapNode* t = *a;
    *a = *b;
    *b = t;
}

// Standard min heapify function
void minHeapify(MinHeap* minHeap, int idx) {
    int smallest = idx;
    int left = 2 * idx + 1;
    int right = 2 * idx + 2;
    
    if (left < minHeap->size && minHeap->array[left]->freq < minHeap->array[smallest]->freq)
        smallest = left;
    if (right < minHeap->size && minHeap->array[right]->freq < minHeap->array[smallest]->freq)
        smallest = right;
    if (smallest != idx) {
        swapMinHeapNode(&minHeap->array[smallest], &minHeap->array[idx]);
        minHeapify(minHeap, smallest);
    }
}

// Extracts the minimum value node from the heap
MinHeapNode* extractMin(MinHeap* minHeap) {
    MinHeapNode* temp = minHeap->array[0];
    minHeap->array[0] = minHeap->array[minHeap->size - 1];
    --minHeap->size;
    minHeapify(minHeap, 0);
    return temp;
}

// Inserts a new node into the min heap
void insertMinHeap(MinHeap* minHeap, MinHeapNode* minHeapNode) {
    ++minHeap->size;
    int i = minHeap->size - 1;
    
    while (i && minHeapNode->freq < minHeap->array[(i - 1) / 2]->freq) {
        minHeap->array[i] = minHeap->array[(i - 1) / 2];
        i = (i - 1) / 2;
    }
    minHeap->array[i] = minHeapNode;
}

// Builds the min heap (rearranges array)
void buildMinHeap(MinHeap* minHeap) {
    int n = minHeap->size - 1;
    for (int i = (n - 1) / 2; i >= 0; i--) {
        minHeapify(minHeap, i);
    }
}

// Checks if the node is a leaf
int isLeaf(MinHeapNode* root) {
    return !(root->left) && !(root->right);
}

// Creates a min heap of capacity equal to size and builds the heap from given data and freq arrays
MinHeap* createAndBuildMinHeap(char data[], int freq[], int size) {
    MinHeap* minHeap = createMinHeap(size);
    for (int i = 0; i < size; i++)
        minHeap->array[i] = newNode(data[i], freq[i]);
    minHeap->size = size;
    buildMinHeap(minHeap);
    return minHeap;
}

// Builds the Huffman tree and returns the root of the tree
MinHeapNode* buildHuffmanTree(char data[], int freq[], int size) {
    MinHeapNode *left, *right, *top;
    
    // Create a min heap & insert all characters of data[]
    MinHeap* minHeap = createAndBuildMinHeap(data, freq, size);
    
    // Iterate while size of heap doesn't become 1
    while (minHeap->size != 1) {
        // Extract the two nodes with the smallest frequency
        left = extractMin(minHeap);
        right = extractMin(minHeap);
        
        // Create a new internal node with frequency equal to the sum of the two nodes
        top = newNode('$', left->freq + right->freq);
        top->left = left;
        top->right = right;
        
        // Add the new node to the min heap
        insertMinHeap(minHeap, top);
    }
    // The remaining node is the root of the Huffman tree
    return extractMin(minHeap);
}

// Prints the Huffman codes by traversing the tree
void printCodes(MinHeapNode* root, int arr[], int top) {
    // Assign 0 to left edge and recur
    if (root->left) {
        arr[top] = 0;
        printCodes(root->left, arr, top + 1);
    }
    
    // Assign 1 to right edge and recur
    if (root->right) {
        arr[top] = 1;
        printCodes(root->right, arr, top + 1);
    }
    
    // If this is a leaf node, then print the character and its code
    if (isLeaf(root)) {
        printf("%c: ", root->data);
        for (int i = 0; i < top; i++)
            printf("%d", arr[i]);
        printf("\n");
    }
}

// Main function to generate Huffman codes for given data and frequency arrays
void HuffmanCodes(char data[], int freq[], int size) {
    // Build Huffman Tree
    MinHeapNode* root = buildHuffmanTree(data, freq, size);
    int arr[100], top = 0;
    // Print Huffman codes using the Huffman tree built above
    printCodes(root, arr, top);
}

int main() {
    // Example characters and their frequencies
    char arr[] = {'a', 'b', 'c', 'd', 'e', 'f'};
    int freq[] = {5, 9, 12, 13, 16, 45};
    int size = sizeof(arr) / sizeof(arr[0]);
    
    printf("Huffman Codes for given characters and frequencies:\n");
    HuffmanCodes(arr, freq, size);
    return 0;
}
```

---

## 3. Fractional Knapsack

**Explanation:**  
The Fractional Knapsack problem allows breaking items into fractions to maximize the total value in a knapsack with a given capacity. The greedy approach sorts items by their value-to-weight ratio and then picks as much of the highest ratio item as possible until the capacity is filled.

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

// Comparison function for qsort to sort items by descending value-to-weight ratio
int compare(const void *a, const void *b) {
    Item *itemA = (Item *) a;
    Item *itemB = (Item *) b;
    double ratioA = (double)itemA->value / itemA->weight;
    double ratioB = (double)itemB->value / itemB->weight;
    // Sort in descending order
    if(ratioA < ratioB)
        return 1;
    else if(ratioA > ratioB)
        return -1;
    else
        return 0;
}

// Function to solve the Fractional Knapsack problem using a greedy approach
double fractionalKnapsack(Item items[], int n, int capacity) {
    // Sort items based on value-to-weight ratio in descending order
    qsort(items, n, sizeof(Item), compare);
    
    double totalValue = 0.0;
    
    // Process items one by one
    for (int i = 0; i < n && capacity > 0; i++) {
        // If the item can be picked entirely
        if (items[i].weight <= capacity) {
            totalValue += items[i].value;
            capacity -= items[i].weight;
        } else {
            // Pick the fraction of the item that fits in the knapsack
            totalValue += items[i].value * ((double)capacity / items[i].weight);
            capacity = 0;
        }
    }
    return totalValue;
}

int main() {
    // Example items with weights and values
    Item items[] = { {10, 60}, {20, 100}, {30, 120} };
    int n = sizeof(items) / sizeof(items[0]);
    int capacity = 50;  // Knapsack capacity
    
    double maxValue = fractionalKnapsack(items, n, capacity);
    printf("Maximum value achievable in the fractional knapsack is: %.2f\n", maxValue);
    return 0;
}
```

---
