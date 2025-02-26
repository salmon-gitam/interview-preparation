Below are eight common sorting algorithms, each with a brief explanation, a sample interview‐style question, and a complete C language solution.

---

### 1. Bubble Sort

**Explanation:**  
Bubble Sort repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order. This “bubbling” of the largest element to the end continues until the entire array is sorted.

**Interview Question:**  
*Write a C program to sort an array using Bubble Sort.*

**C Solution:**
```c
#include <stdio.h>

void bubbleSort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        // Last i elements are already sorted
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j+1]) {
                // Swap arr[j] and arr[j+1]
                int temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
    }
}

int main() {
    int arr[] = {64, 34, 25, 12, 22, 11, 90};
    int n = sizeof(arr) / sizeof(arr[0]);
    bubbleSort(arr, n);
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    return 0;
}
```

---

### 2. Selection Sort

**Explanation:**  
Selection Sort divides the array into a sorted and unsorted part. It repeatedly selects the minimum element from the unsorted part and moves it to the end of the sorted part.

**Interview Question:**  
*Write a C program to sort an array using Selection Sort.*

**C Solution:**
```c
#include <stdio.h>

void selectionSort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int min_idx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[min_idx])
                min_idx = j;
        }
        // Swap the found minimum element with arr[i]
        int temp = arr[i];
        arr[i] = arr[min_idx];
        arr[min_idx] = temp;
    }
}

int main() {
    int arr[] = {29, 10, 14, 37, 13};
    int n = sizeof(arr) / sizeof(arr[0]);
    selectionSort(arr, n);
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    return 0;
}
```

---

### 3. Insertion Sort

**Explanation:**  
Insertion Sort builds the sorted array one element at a time by repeatedly taking the next element and inserting it into the correct position in the already sorted portion.

**Interview Question:**  
*Write a C program to sort an array using Insertion Sort.*

**C Solution:**
```c
#include <stdio.h>

void insertionSort(int arr[], int n) {
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        // Move elements greater than key one position ahead
        while (j >= 0 && arr[j] > key) {
            arr[j+1] = arr[j];
            j--;
        }
        arr[j+1] = key;
    }
}

int main() {
    int arr[] = {12, 11, 13, 5, 6};
    int n = sizeof(arr) / sizeof(arr[0]);
    insertionSort(arr, n);
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    return 0;
}
```

---

### 4. Merge Sort

**Explanation:**  
Merge Sort is a divide-and-conquer algorithm that divides the array into halves, recursively sorts each half, and then merges the sorted halves to produce the final sorted array.

**Interview Question:**  
*Write a C program to sort an array using Merge Sort.*

**C Solution:**
```c
#include <stdio.h>
#include <stdlib.h>

void merge(int arr[], int l, int m, int r) {
    int i, j, k;
    int n1 = m - l + 1;
    int n2 = r - m;
    
    int *L = (int*)malloc(n1 * sizeof(int));
    int *R = (int*)malloc(n2 * sizeof(int));
    
    for (i = 0; i < n1; i++)
        L[i] = arr[l + i];
    for (j = 0; j < n2; j++)
        R[j] = arr[m + 1 + j];
        
    i = 0; j = 0; k = l;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j])
            arr[k++] = L[i++];
        else
            arr[k++] = R[j++];
    }
    
    while (i < n1)
        arr[k++] = L[i++];
    while (j < n2)
        arr[k++] = R[j++];
        
    free(L);
    free(R);
}

void mergeSort(int arr[], int l, int r) {
    if (l < r) {
        int m = l + (r - l) / 2;
        mergeSort(arr, l, m);
        mergeSort(arr, m + 1, r);
        merge(arr, l, m, r);
    }
}

int main() {
    int arr[] = {12, 11, 13, 5, 6, 7};
    int n = sizeof(arr) / sizeof(arr[0]);
    mergeSort(arr, 0, n - 1);
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    return 0;
}
```

---

### 5. Quick Sort

**Explanation:**  
Quick Sort selects a pivot element and partitions the array so that elements less than the pivot are on the left and elements greater than the pivot are on the right. It then recursively sorts the subarrays.

**Interview Question:**  
*Write a C program to sort an array using Quick Sort.*

**C Solution:**
```c
#include <stdio.h>

void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int partition(int arr[], int low, int high) {
    int pivot = arr[high]; // pivot element
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(&arr[i], &arr[j]);
        }
    }
    swap(&arr[i + 1], &arr[high]);
    return i + 1;
}

void quickSort(int arr[], int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

int main() {
    int arr[] = {10, 7, 8, 9, 1, 5};
    int n = sizeof(arr) / sizeof(arr[0]);
    quickSort(arr, 0, n - 1);
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    return 0;
}
```

---

### 6. Heap Sort

**Explanation:**  
Heap Sort transforms the array into a max heap (a complete binary tree where each parent is greater than its children), then repeatedly extracts the maximum element (root) and rebuilds the heap until the array is sorted.

**Interview Question:**  
*Write a C program to sort an array using Heap Sort.*

**C Solution:**
```c
#include <stdio.h>

void heapify(int arr[], int n, int i) {
    int largest = i;       // Initialize largest as root
    int l = 2 * i + 1;     // left child index
    int r = 2 * i + 2;     // right child index

    if (l < n && arr[l] > arr[largest])
        largest = l;
    if (r < n && arr[r] > arr[largest])
        largest = r;
    
    if (largest != i) {
        int temp = arr[i];
        arr[i] = arr[largest];
        arr[largest] = temp;
        heapify(arr, n, largest);
    }
}

void heapSort(int arr[], int n) {
    // Build heap (rearrange array)
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);
    
    // One by one extract an element from heap
    for (int i = n - 1; i >= 0; i--) {
        // Move current root to end
        int temp = arr[0];
        arr[0] = arr[i];
        arr[i] = temp;
        // call max heapify on the reduced heap
        heapify(arr, i, 0);
    }
}

int main() {
    int arr[] = {12, 11, 13, 5, 6, 7};
    int n = sizeof(arr) / sizeof(arr[0]);
    heapSort(arr, n);
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    return 0;
}
```

---

### 7. Radix Sort

**Explanation:**  
Radix Sort processes each digit of the numbers, starting from the least significant digit to the most significant digit. It often uses Counting Sort as a subroutine to sort the numbers based on each digit.

**Interview Question:**  
*Write a C program to sort an array of non-negative integers using Radix Sort.*

**C Solution:**
```c
#include <stdio.h>

int getMax(int arr[], int n) {
    int max = arr[0];
    for (int i = 1; i < n; i++)
        if (arr[i] > max)
            max = arr[i];
    return max;
}

void countSort(int arr[], int n, int exp) {
    int output[n];
    int count[10] = {0};

    // Store count of occurrences in count[]
    for (int i = 0; i < n; i++)
        count[(arr[i] / exp) % 10]++;

    // Change count[i] so that count[i] now contains actual position of this digit in output[]
    for (int i = 1; i < 10; i++)
        count[i] += count[i - 1];

    // Build the output array
    for (int i = n - 1; i >= 0; i--) {
        output[count[(arr[i] / exp) % 10] - 1] = arr[i];
        count[(arr[i] / exp) % 10]--;
    }

    // Copy the output array to arr[]
    for (int i = 0; i < n; i++)
        arr[i] = output[i];
}

void radixSort(int arr[], int n) {
    int m = getMax(arr, n);
    // Do counting sort for every digit. Note: exp is 10^i
    for (int exp = 1; m/exp > 0; exp *= 10)
        countSort(arr, n, exp);
}

int main() {
    int arr[] = {170, 45, 75, 90, 802, 24, 2, 66};
    int n = sizeof(arr) / sizeof(arr[0]);
    radixSort(arr, n);
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    return 0;
}
```

---

### 8. Counting Sort

**Explanation:**  
Counting Sort works by counting the occurrences of each distinct element in the array. It then uses these counts to compute the positions of each element in the sorted output, making it very efficient when the range of input data is not significantly larger than the number of elements.

**Interview Question:**  
*Write a C program to sort an array of non-negative integers using Counting Sort.*

**C Solution:**
```c
#include <stdio.h>
#include <stdlib.h>

void countingSort(int arr[], int n) {
    int i;
    // Find the maximum element in arr[]
    int max = arr[0];
    for (i = 1; i < n; i++)
        if (arr[i] > max)
            max = arr[i];
    
    // Allocate count array and initialize to zero
    int *count = (int*)calloc(max + 1, sizeof(int));
    
    // Store count of each element
    for (i = 0; i < n; i++)
        count[arr[i]]++;
    
    // Overwrite arr[] with sorted elements
    int index = 0;
    for (i = 0; i <= max; i++) {
        while (count[i]-- > 0)
            arr[index++] = i;
    }
    
    free(count);
}

int main() {
    int arr[] = {4, 2, 2, 8, 3, 3, 1};
    int n = sizeof(arr) / sizeof(arr[0]);
    countingSort(arr, n);
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    return 0;
}
```

---
