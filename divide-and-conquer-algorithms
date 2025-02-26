Below are four classic problems with a brief explanation, an interview‐style question, and complete C solutions with inline comments that explain each key step.

---

## 1. Merge Sort

**Explanation:**  
Merge Sort is a divide-and-conquer algorithm that splits an array into halves, recursively sorts each half, and then merges the sorted halves into a fully sorted array.

**Interview Question:**  
*Write a C program to sort an array using Merge Sort.*

**C Solution:**
```c
#include <stdio.h>
#include <stdlib.h>

// Merge function to combine two sorted subarrays into one sorted array.
void merge(int arr[], int left, int mid, int right) {
    int i = left, j = mid + 1, k = 0;
    int n = right - left + 1;  // Total number of elements in the range.
    
    // Allocate temporary array to hold merged result.
    int *temp = (int *)malloc(n * sizeof(int));
    
    // Merge elements from both subarrays in sorted order.
    while(i <= mid && j <= right) {
        if(arr[i] <= arr[j])
            temp[k++] = arr[i++];
        else
            temp[k++] = arr[j++];
    }
    
    // Copy any remaining elements from the left subarray.
    while(i <= mid)
        temp[k++] = arr[i++];
    
    // Copy any remaining elements from the right subarray.
    while(j <= right)
        temp[k++] = arr[j++];
    
    // Copy merged elements back into the original array.
    for(i = left, k = 0; i <= right; i++, k++)
        arr[i] = temp[k];
    
    free(temp);
}

// Recursive Merge Sort function.
void mergeSort(int arr[], int left, int right) {
    if(left < right) {
        int mid = left + (right - left) / 2; // Find the midpoint.
        mergeSort(arr, left, mid);           // Sort the left half.
        mergeSort(arr, mid + 1, right);        // Sort the right half.
        merge(arr, left, mid, right);          // Merge the sorted halves.
    }
}

int main() {
    int arr[] = {38, 27, 43, 3, 9, 82, 10};
    int n = sizeof(arr) / sizeof(arr[0]);
    
    mergeSort(arr, 0, n - 1);  // Sort the array.
    
    printf("Sorted array using Merge Sort:\n");
    for(int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    printf("\n");
    
    return 0;
}
```

---

## 2. Quick Sort

**Explanation:**  
Quick Sort is a divide-and-conquer algorithm that selects a pivot element and partitions the array so that elements less than the pivot come before it and elements greater come after it. It then recursively sorts the partitions.

**Interview Question:**  
*Write a C program to sort an array using Quick Sort.*

**C Solution:**
```c
#include <stdio.h>

// Utility function to swap two integers.
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

// Partition function that places the pivot element at the correct position
// and ensures that elements on the left are less than the pivot and those on the right are greater.
int partition(int arr[], int low, int high) {
    int pivot = arr[high];  // Choose the last element as the pivot.
    int i = low - 1;        // Index of the smaller element.
    
    for (int j = low; j < high; j++) {
        // If current element is less than the pivot, swap it.
        if (arr[j] < pivot) {
            i++;
            swap(&arr[i], &arr[j]);
        }
    }
    // Place the pivot element in its correct sorted position.
    swap(&arr[i + 1], &arr[high]);
    return i + 1;  // Return the pivot index.
}

// Recursive Quick Sort function.
void quickSort(int arr[], int low, int high) {
    if (low < high) {
        // Partition the array and get the pivot index.
        int pi = partition(arr, low, high);
        
        // Recursively sort elements before and after partition.
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

int main() {
    int arr[] = {10, 7, 8, 9, 1, 5};
    int n = sizeof(arr) / sizeof(arr[0]);
    
    quickSort(arr, 0, n - 1);  // Sort the array.
    
    printf("Sorted array using Quick Sort:\n");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    printf("\n");
    
    return 0;
}
```

---

## 3. Binary Search

**Explanation:**  
Binary Search is an efficient algorithm for finding an element in a sorted array. It repeatedly divides the search interval in half by comparing the target with the middle element.

**Interview Question:**  
*Write a C program to search for a target element in a sorted array using Binary Search.*

**C Solution:**
```c
#include <stdio.h>

// Function to perform binary search on a sorted array.
// Returns the index of the target element if found, otherwise returns -1.
int binarySearch(int arr[], int low, int high, int target) {
    while (low <= high) {
        int mid = low + (high - low) / 2;  // Calculate middle index to avoid overflow.
        
        // Check if target is present at mid.
        if (arr[mid] == target)
            return mid;
        // If target is greater, ignore left half.
        else if (arr[mid] < target)
            low = mid + 1;
        // If target is smaller, ignore right half.
        else
            high = mid - 1;
    }
    return -1;  // Target not found.
}

int main() {
    int arr[] = {1, 3, 5, 7, 9, 11};
    int n = sizeof(arr) / sizeof(arr[0]);
    int target = 7;
    
    int result = binarySearch(arr, 0, n - 1, target);
    
    if (result != -1)
        printf("Element %d found at index %d using Binary Search.\n", target, result);
    else
        printf("Element %d not found in the array.\n", target);
    
    return 0;
}
```

---

## 4. Strassen’s Matrix Multiplication

**Explanation:**  
Strassen’s Matrix Multiplication is an advanced algorithm that multiplies two matrices faster than the conventional method. For simplicity, this example implements Strassen’s algorithm for multiplying two 2x2 matrices. It reduces the number of multiplications by computing 7 intermediary products and combining them to form the final product matrix.

**Interview Question:**  
*Write a C program to multiply two 2x2 matrices using Strassen’s Matrix Multiplication algorithm.*

**C Solution:**
```c
#include <stdio.h>

// Function to multiply two 2x2 matrices using Strassen's algorithm.
void strassen2x2(int A[2][2], int B[2][2], int C[2][2]) {
    // Calculate the 7 products using Strassen's formulas.
    int M1 = (A[0][0] + A[1][1]) * (B[0][0] + B[1][1]);
    int M2 = (A[1][0] + A[1][1]) * B[0][0];
    int M3 = A[0][0] * (B[0][1] - B[1][1]);
    int M4 = A[1][1] * (B[1][0] - B[0][0]);
    int M5 = (A[0][0] + A[0][1]) * B[1][1];
    int M6 = (A[1][0] - A[0][0]) * (B[0][0] + B[0][1]);
    int M7 = (A[0][1] - A[1][1]) * (B[1][0] + B[1][1]);
    
    // Combine the intermediary products to form the resulting matrix.
    C[0][0] = M1 + M4 - M5 + M7;
    C[0][1] = M3 + M5;
    C[1][0] = M2 + M4;
    C[1][1] = M1 - M2 + M3 + M6;
}

int main() {
    // Define two 2x2 matrices.
    int A[2][2] = { {1, 2}, {3, 4} };
    int B[2][2] = { {5, 6}, {7, 8} };
    int C[2][2];  // Matrix to store the result.
    
    // Multiply matrices A and B using Strassen's algorithm.
    strassen2x2(A, B, C);
    
    // Print the result matrix.
    printf("Result of Strassen's Matrix Multiplication (2x2):\n");
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 2; j++)
            printf("%d ", C[i][j]);
        printf("\n");
    }
    
    return 0;
}
```

---
