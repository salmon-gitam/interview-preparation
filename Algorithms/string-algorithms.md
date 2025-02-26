Below are three classic string problems along with a brief explanation, an interview‐style question, and complete C solutions with inline comments.

---

## 1. Pattern Matching using KMP (Knuth-Morris-Pratt)

**Explanation:**  
The KMP algorithm searches for occurrences of a "pattern" in a "text" efficiently by precomputing an LPS (Longest Prefix Suffix) array. This table tells how far to shift the pattern when a mismatch occurs, avoiding redundant comparisons.

**Interview Question:**  
*Write a C program to find all occurrences of a pattern in a text using the KMP algorithm.*

**C Solution:**
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

// Function to compute the LPS (Longest Prefix Suffix) array for the pattern.
void computeLPSArray(char *pattern, int M, int *lps) {
    int len = 0;  // length of the previous longest prefix suffix
    lps[0] = 0;   // lps[0] is always 0
    int i = 1;

    // Loop calculates lps[i] for i = 1 to M-1
    while (i < M) {
        if (pattern[i] == pattern[len]) {
            len++;
            lps[i] = len;
            i++;
        } else {
            // This is tricky. Consider the example AAACAAAA and i = 7.
            if (len != 0) {
                len = lps[len - 1];
                // Do not increment i here.
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }
}

// KMP search function to find all occurrences of the pattern in the text.
void KMPSearch(char *text, char *pattern) {
    int N = strlen(text);
    int M = strlen(pattern);

    // Create lps[] that will hold the longest prefix suffix values for pattern
    int *lps = (int *)malloc(M * sizeof(int));
    computeLPSArray(pattern, M, lps);

    int i = 0; // index for text[]
    int j = 0; // index for pattern[]
    while (i < N) {
        if (pattern[j] == text[i]) {
            i++;
            j++;
        }
        if (j == M) {
            printf("Pattern found at index %d\n", i - j);
            // Use the last computed lps value to continue searching
            j = lps[j - 1];
        }
        // Mismatch after j matches.
        else if (i < N && pattern[j] != text[i]) {
            if (j != 0)
                j = lps[j - 1];
            else
                i = i + 1;
        }
    }
    free(lps);
}

int main() {
    char text[] = "ABABDABACDABABCABAB";
    char pattern[] = "ABABCABAB";
    printf("Text: %s\n", text);
    printf("Pattern: %s\n\n", pattern);
    
    KMPSearch(text, pattern);
    return 0;
}
```

---

## 2. Longest Palindromic Substring

**Explanation:**  
This problem asks for the longest substring within a given string that reads the same forward and backward. A common method is to expand around each center (which can be one character or between two characters) and track the maximum palindrome found.

**Interview Question:**  
*Write a C program to find the longest palindromic substring in a given string.*

**C Solution:**
```c
#include <stdio.h>
#include <string.h>

// Function to expand around the center and return the length of the palindrome.
int expandAroundCenter(char *s, int left, int right) {
    while (left >= 0 && right < strlen(s) && s[left] == s[right]) {
        left--;
        right++;
    }
    // Return length of palindrome
    return right - left - 1;
}

// Function to find the longest palindromic substring.
void longestPalindromicSubstring(char *s) {
    if (s == NULL || strlen(s) < 1) {
        printf("Empty string provided.\n");
        return;
    }
    
    int start = 0, end = 0;
    int len = strlen(s);
    
    for (int i = 0; i < len; i++) {
        // For odd length palindrome, center is at i
        int len1 = expandAroundCenter(s, i, i);
        // For even length palindrome, center is between i and i+1
        int len2 = expandAroundCenter(s, i, i + 1);
        int maxLen = (len1 > len2) ? len1 : len2;
        
        if (maxLen > end - start) {
            start = i - (maxLen - 1) / 2;
            end = i + maxLen / 2;
        }
    }
    
    printf("Longest Palindromic Substring: ");
    for (int i = start; i <= end; i++) {
        printf("%c", s[i]);
    }
    printf("\n");
}

int main() {
    char s[] = "babad";
    printf("Input String: %s\n", s);
    longestPalindromicSubstring(s);
    return 0;
}
```

---

## 3. Anagram Detection

**Explanation:**  
Two strings are anagrams if they contain the same characters with the same frequency. One simple method is to count the frequency of each character in both strings and compare the counts.

**Interview Question:**  
*Write a C program to check if two given strings are anagrams of each other.*

**C Solution:**
```c
#include <stdio.h>
#include <string.h>
#include <stdbool.h>

#define CHAR_RANGE 256  // Total possible characters (extended ASCII)

// Function to check if two strings are anagrams
bool areAnagrams(char *str1, char *str2) {
    int count[CHAR_RANGE] = {0};
    
    // If lengths differ, they cannot be anagrams
    if (strlen(str1) != strlen(str2))
        return false;
    
    // Increment count for characters in str1 and decrement for str2
    for (int i = 0; str1[i] && str2[i]; i++) {
        count[(int)str1[i]]++;
        count[(int)str2[i]]--;
    }
    
    // If all counts are zero, then str1 and str2 are anagrams
    for (int i = 0; i < CHAR_RANGE; i++) {
        if (count[i] != 0)
            return false;
    }
    return true;
}

int main() {
    char str1[] = "listen";
    char str2[] = "silent";
    
    if (areAnagrams(str1, str2))
        printf("\"%s\" and \"%s\" are anagrams.\n", str1, str2);
    else
        printf("\"%s\" and \"%s\" are NOT anagrams.\n", str1, str2);
    
    return 0;
}
```

---
