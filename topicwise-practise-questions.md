Below are 10 distinct problem statements for each of the listed C topics. These questions are designed to help you practice and master various aspects of C programming.

---

### Basic (Primitive) Data Types

1. **Arithmetic Operations:** Write a program that reads two integers from the user and prints their sum, difference, product, and quotient.
2. **Data Size Display:** Create a program that prints the size (in bytes) of each basic data type (int, char, float, double, void pointer).
3. **Type Conversion:** Write a program that reads an integer and a float, converts them into each other (using explicit casting), and displays the results.
4. **Character Manipulation:** Write a program that takes a lowercase letter as input and prints its uppercase equivalent using ASCII arithmetic.
5. **Area Calculation:** Develop a program to calculate the area of a circle using a double variable for the radius and a constant value for π.
6. **Temperature Conversion:** Write a program that converts a temperature from Celsius to Fahrenheit using float variables.
7. **Integer Overflow Demonstration:** Write a program that shows the result of adding 1 to the maximum value of an int.
8. **Simple Calculator:** Create a basic calculator that performs addition, subtraction, multiplication, and division on two numbers using basic data types.
9. **Modulo Operation:** Write a program to check if a given integer is even or odd using the modulus operator.
10. **Input/Output Exercise:** Write a program that uses scanf to read a character, an integer, and a float from the user and then prints them using printf.

---

### Modified/Derived Integral Types

1. **Extended Range Calculation:** Write a program that calculates the factorial of a number using an unsigned long long to handle larger results.
2. **Short and Long Comparison:** Write a program that reads an integer and prints its value using short, int, and long data types, explaining the differences.
3. **Unsigned Arithmetic:** Write a program to demonstrate arithmetic with unsigned int variables, including a case that shows underflow.
4. **Range Limits:** Create a program that prints the maximum and minimum values of short, int, and long data types (using limits.h macros).
5. **Large Number Operations:** Write a program that multiplies two large long long integers and prints the result.
6. **Bitwise Shift Exercise:** Write a program to demonstrate left and right bitwise shifts on an unsigned integer.
7. **Type Promotion:** Write a program that shows how an unsigned int interacts with a signed int in arithmetic expressions.
8. **Casting to Unsigned:** Write a program that casts a negative integer to an unsigned integer and displays the result.
9. **Comparing Data Types:** Write a program that compares a long and an unsigned int to illustrate implicit type conversions.
10. **Memory Size Check:** Write a program to print the size (in bytes) of short int, int, long int, and long long int.

---

### User-Defined Data Types

1. **Structure for Student Records:** Create a struct for a student (name, roll number, marks) and write a program to display a student’s information.
2. **Union Usage:** Write a program that uses a union to store either an integer or a float, and then prints the stored value.
3. **Enumeration for Days:** Create an enum for the days of the week and write a program to print the day corresponding to a given enum value.
4. **Nested Structures:** Write a program that defines a structure for an address and nests it within a structure for a person.
5. **Sorting Structures:** Write a program that creates an array of structures representing employees and sorts them by their salary.
6. **Complex Number Operations:** Define a structure for complex numbers and write functions to add and multiply them.
7. **Typedef for Simplification:** Use typedef to create an alias for a struct representing a point in 2D and write a program to compute the distance between two points.
8. **Linked List Node:** Write a program that defines a structure for a linked list node, then creates and traverses a simple linked list.
9. **Union vs. Struct:** Write a program to demonstrate the memory usage differences between a struct and a union.
10. **Enum in Switch:** Write a program that uses an enum for traffic lights and employs a switch statement to simulate light changes.

---

### Derived Data Types

1. **Array Reversal:** Write a program to reverse an array of integers.
2. **Max Element in Array:** Write a program to find the maximum element in an array.
3. **String Copy Using Pointers:** Write a program to copy one string into another without using library functions.
4. **2D Array Summation:** Write a program to compute the sum of all elements in a two-dimensional array.
5. **Function Pointer Usage:** Write a program that uses a function pointer to select between addition and subtraction functions.
6. **Multidimensional Array Printing:** Write a program to initialize and print a 3×3 matrix.
7. **Dynamic Array Allocation:** Write a program to dynamically allocate an array of integers and then free the memory.
8. **Sorting Strings:** Write a program to sort an array of strings using function pointers for the comparison function.
9. **Array as Function Argument:** Write a program that passes an array to a function to compute the average of its elements.
10. **Pointer to Array:** Write a program that demonstrates pointer arithmetic by iterating through an array using a pointer.

---

### Operators

1. **Operator Precedence:** Write a program to demonstrate the order of operations in a complex arithmetic expression.
2. **Relational Comparison:** Write a program that compares two numbers using relational operators and prints the results.
3. **Logical Expression Evaluation:** Write a program to evaluate a complex logical expression and display whether it is true or false.
4. **Bitwise Operations:** Write a program that demonstrates bitwise AND, OR, XOR, and NOT operations on two integers.
5. **Shift Operators:** Write a program that uses the left shift and right shift operators to multiply and divide an integer by 2.
6. **Compound Assignment:** Write a program to demonstrate the use of compound assignment operators (e.g., +=, -=, *=).
7. **Conditional Operator:** Write a program that uses the ternary operator to check if a number is positive, negative, or zero.
8. **Modulus Operator:** Write a program that uses the modulus operator to check if a number is even or odd.
9. **Sizeof Operator:** Write a program to print the sizes of various data types using the sizeof operator.
10. **Mixed-Type Expression:** Write a program that computes an expression involving different data types and demonstrates implicit type conversion.

---

### Control Structures

1. **If-Else Decision:** Write a program that reads a number and uses if-else statements to determine if it is positive, negative, or zero.
2. **Switch Case for Weekdays:** Write a program that uses a switch statement to print the name of a weekday based on an input number.
3. **For Loop Summation:** Write a program that uses a for loop to calculate the sum of the first 100 natural numbers.
4. **While Loop Factorial:** Write a program that calculates the factorial of a number using a while loop.
5. **Do-While Input Validation:** Write a program that uses a do-while loop to repeatedly ask for user input until a valid number is entered.
6. **Nested Loops Multiplication Table:** Write a program that prints the multiplication table for numbers 1 to 10 using nested loops.
7. **Loop Control with Break:** Write a program that searches for a number in an array and uses break to exit the loop when found.
8. **Loop Control with Continue:** Write a program that prints all numbers from 1 to 20, skipping multiples of 3 using continue.
9. **Switch with Fall-Through:** Write a program that demonstrates fall-through behavior in a switch-case construct.
10. **Conditional Operator:** Write a program that uses the ternary operator to decide which message to print based on a condition.

---

### Functions

1. **Sum Function:** Write a function that takes two integers as parameters and returns their sum.
2. **Recursive Factorial:** Write a recursive function to compute the factorial of a given number.
3. **Fibonacci Function:** Write a function to compute the nth Fibonacci number recursively.
4. **Swap Function:** Write a function that swaps two numbers using pointers.
5. **String Length:** Write a function that calculates the length of a string without using library functions.
6. **Prime Check:** Write a function that determines whether a number is prime.
7. **Array Average:** Write a function that takes an array and its size, then returns the average of its elements.
8. **Power Function:** Write a recursive function to compute the power of a number.
9. **Palindrome Check:** Write a function that checks whether a given string is a palindrome.
10. **Function Pointer Demonstration:** Write a program that uses a function pointer to call one of two functions based on user input.

---

### Pointers and Memory Management

1. **Dynamic Array Allocation:** Write a program that dynamically allocates an array of integers, fills it with values, and then frees the memory.
2. **Pointer Arithmetic:** Write a program that demonstrates pointer arithmetic by iterating over an array.
3. **Double Pointer Example:** Write a program that uses a pointer to a pointer to modify a variable’s value.
4. **Memory Leak Demonstration:** Write a program that allocates memory but intentionally does not free it, then explain the consequences.
5. **Malloc vs. Calloc:** Write a program that compares the behavior of malloc and calloc when allocating memory for an array.
6. **Realloc Usage:** Write a program that dynamically resizes an array using realloc.
7. **Linked List Implementation:** Write a program that creates a simple singly linked list using dynamic memory allocation.
8. **Freeing Nested Pointers:** Write a program that allocates memory for a 2D array dynamically and then frees it correctly.
9. **Pointer Function Parameters:** Write a program that passes an array to a function using pointers and prints its contents.
10. **Swapping Pointers:** Write a program that swaps two pointer variables by passing their addresses to a function.

---

### Preprocessor Directives

1. **Constant Definition:** Write a program that uses #define to declare a constant value and uses it in calculations.
2. **Header Inclusion:** Write a program that includes a custom header file using #include and demonstrates a function from that header.
3. **Conditional Compilation:** Write a program that uses #ifdef and #endif to include debug messages only in debug mode.
4. **Include Guard:** Write a header file with include guards (#ifndef/#define/#endif) to prevent multiple inclusions.
5. **Macro Function:** Write a program that defines a macro function using #define to compute the square of a number.
6. **File and Line Macros:** Write a program that prints __FILE__ and __LINE__ to display the current file name and line number.
7. **Using #pragma:** Write a program that uses #pragma once in a header file.
8. **Error Directive:** Write a program that uses #error to generate a compile-time error when a certain condition is met.
9. **Undef Directive:** Write a program that uses #undef to remove a previously defined macro.
10. **Conditional Macro Expansion:** Write a program that uses #if and #elif to choose between different macro definitions based on a constant value.

---

### Type Qualifiers and Storage Classes

1. **Const Variable:** Write a program that declares a constant variable using const and attempts (and fails) to modify it.
2. **Volatile Variable:** Write a program that declares a volatile variable and explains how it might change unexpectedly.
3. **Static Local Variable:** Write a program that demonstrates the persistence of a static local variable across multiple function calls.
4. **Extern Variable:** Write a multi-file program where one file declares a global variable with extern and another file defines it.
5. **Register Keyword:** Write a program that declares a variable with the register keyword and explains its intended use.
6. **Restrict Pointer:** Write a program that uses the restrict qualifier with a pointer to improve optimization.
7. **Auto vs. Static:** Write a program comparing the behavior of auto (local) variables versus static variables inside a loop.
8. **Const Pointers:** Write a program that demonstrates the difference between a pointer to const data and a const pointer.
9. **Storage Class Impact:** Write a program that shows how the storage class affects the scope and lifetime of variables.
10. **Multi-file Storage Classes:** Write a multi-file project that demonstrates the use of static and extern storage classes.

---

### Comments and Code Organization

1. **Single-line Comments:** Write a program that uses single-line comments to explain each statement.
2. **Multi-line Comments:** Write a program that includes multi-line comments describing the purpose of a function.
3. **Section Headers:** Write a program that organizes code into sections using comment blocks to separate different functionalities.
4. **Documentation Comments:** Write a program that documents each function with comments detailing its parameters and return value.
5. **Inline Explanations:** Write a program that includes inline comments explaining complex expressions.
6. **TODO Comments:** Write a program with TODO comments indicating areas that require future improvements.
7. **Author Information:** Write a program that includes a header comment with the author’s name, date, and version.
8. **Function Outline:** Write a program that uses comments to outline the logic of the main function before implementation.
9. **Code Block Comments:** Write a program that uses block comments to group related variable declarations.
10. **Readable Formatting:** Write a program that is well-organized with clear indentation and comments that enhance readability.

---

### Miscellaneous Topics

1. **Type Casting:** Write a program that demonstrates both implicit and explicit type casting between different data types.
2. **Formatted Output:** Write a program that uses printf to display an integer, float, character, and string with proper formatting.
3. **Input with scanf:** Write a program that uses scanf to safely read an integer and a string from the user.
4. **Error Checking:** Write a program that checks the return value of scanf and prints an error message if input fails.
5. **Using perror:** Write a program that attempts to open a file and uses perror to print an error message if the file cannot be opened.
6. **Assert for Debugging:** Write a program that uses assert to verify a critical condition within a function.
7. **Conditional Debugging:** Write a program that uses conditional compilation (e.g., #ifdef DEBUG) to include debugging print statements.
8. **String to Integer Conversion:** Write a program that uses the atoi function to convert a string to an integer and handles conversion errors.
9. **Variadic Functions:** Write a program that demonstrates the creation and use of a variadic function similar to printf.
10. **Input Validation:** Write a program that validates user input (for example, ensuring a number falls within a certain range) and prints an appropriate error message if it does not.

---

These 120 distinct problem statements cover a wide range of data types and syntax topics in C. Each problem is crafted to help you focus on a specific aspect of the language without any repetition. Happy coding!
