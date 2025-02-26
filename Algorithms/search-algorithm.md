Below are two common search algorithms along with a brief explanation, a sample interview-style question, and a complete C language solution with inline comments explaining the code.

---

### 1. Linear Search

**Explanation:**  
Linear Search examines each element in an array one by one until it finds the target value or reaches the end of the array. It works on unsorted lists and has a time complexity of O(n).

**Interview Question:**  
*Write a C program to perform a linear search on an array.*

**C Solution:**
```c
#include <stdio.h>

// Function to perform linear search in the array
// Returns the index of the target element if found; otherwise, returns -1.
int linearSearch(int arr[], int n, int target) {
    // Loop through each element of the array
    for (int i = 0; i < n; i++) {
        // Check if the current element equals the target
        if (arr[i] == target)
            return i; // Return the index if found
    }
    return -1; // Return -1 if target is not found in the array
}

int main() {
    // Example array
    int arr[] = {10, 20, 30, 40, 50};
    // Calculate the number of elements in the array
    int n = sizeof(arr) / sizeof(arr[0]);
    // Define the target value to search for
    int target = 30;
    
    // Call the linearSearch function and store the result
    int index = linearSearch(arr, n, target);
    
    // Check if the target was found and print the appropriate message
    if (index != -1)
        printf("Element %d found at index %d.\n", target, index);
    else
        printf("Element %d not found in the array.\n", target);
    
    return 0;
}
```

---

### 2. Binary Search

**Explanation:**  
Binary Search works on sorted arrays by repeatedly dividing the search interval in half. It compares the target with the middle element; if they are not equal, it decides which half of the array to continue searching. Its time complexity is O(log n).

**Interview Question:**  
*Write a C program to perform a binary search on a sorted array.*

**C Solution:**
```c
#include <stdio.h>

// Function to perform binary search in a sorted array
// Returns the index of the target element if found; otherwise, returns -1.
int binarySearch(int arr[], int left, int right, int target) {
    while (left <= right) {
        // Calculate the middle index to avoid overflow
        int mid = left + (right - left) / 2;
        
        // Check if the middle element is the target
        if (arr[mid] == target)
            return mid;
        
        // If the target is greater than the middle element, ignore the left half
        if (arr[mid] < target)
            left = mid + 1;
        else
            // If the target is less than the middle element, ignore the right half
            right = mid - 1;
    }
    // Target element was not found in the array
    return -1;
}

int main() {
    // Sorted array for binary search
    int arr[] = {10, 20, 30, 40, 50, 60, 70};
    // Calculate the number of elements in the array
    int n = sizeof(arr) / sizeof(arr[0]);
    // Define the target value to search for
    int target = 40;
    
    // Call the binarySearch function and store the result
    int index = binarySearch(arr, 0, n - 1, target);
    
    // Check if the target was found and print the appropriate message
    if (index != -1)
        printf("Element %d found at index %d.\n", target, index);
    else
        printf("Element %d not found in the array.\n", target);
    
    return 0;
}
```

---
