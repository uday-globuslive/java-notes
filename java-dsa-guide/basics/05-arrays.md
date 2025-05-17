# Arrays in Java

Arrays are used to store multiple values of the same type in a single variable. They are a fundamental data structure in Java that allow you to organize and manipulate collections of data efficiently.

## Declaring Arrays

There are several ways to declare an array in Java:

### 1. Declaration with Size

```java
// Syntax: dataType[] arrayName = new dataType[size];
int[] numbers = new int[5]; // Creates an array of 5 integers
```

This creates an array of the specified size, with default values:
- For numeric types (int, long, float, etc.): 0
- For boolean: false
- For char: '\u0000' (null character)
- For objects: null

### 2. Declaration with Initialization

```java
// Syntax: dataType[] arrayName = {value1, value2, value3, ...};
int[] numbers = {10, 20, 30, 40, 50}; // Creates and initializes an array
```

### 3. Declaration Then Initialization

```java
// First declare
int[] numbers;
// Then initialize
numbers = new int[5];
```

## Accessing Array Elements

Array elements are accessed using their index. In Java, array indices start at 0.

```java
int[] numbers = {10, 20, 30, 40, 50};

// Accessing elements
int firstElement = numbers[0]; // Gets 10
int thirdElement = numbers[2]; // Gets 30

// Modifying elements
numbers[1] = 25; // Changes the second element from 20 to 25
```

## Array Length

The length of an array can be determined using the `length` property:

```java
int[] numbers = {10, 20, 30, 40, 50};
int arrayLength = numbers.length; // Returns 5
```

## Iterating Through Arrays

### 1. Using for Loop

```java
int[] numbers = {10, 20, 30, 40, 50};

// Standard for loop
for (int i = 0; i < numbers.length; i++) {
    System.out.println("Element at index " + i + ": " + numbers[i]);
}
```

### 2. Using Enhanced for Loop (for-each)

```java
int[] numbers = {10, 20, 30, 40, 50};

// Enhanced for loop
for (int number : numbers) {
    System.out.println("Element: " + number);
}
```

### 3. Using Java 8 Stream API

```java
int[] numbers = {10, 20, 30, 40, 50};

// Using streams
Arrays.stream(numbers).forEach(number -> System.out.println("Element: " + number));
```

## Multi-dimensional Arrays

Java supports multi-dimensional arrays, which are essentially arrays of arrays.

### Two-dimensional Arrays

```java
// Declaration with size
int[][] matrix = new int[3][4]; // 3 rows, 4 columns

// Declaration with initialization
int[][] matrix = {
    {1, 2, 3, 4},
    {5, 6, 7, 8},
    {9, 10, 11, 12}
};

// Accessing elements
int element = matrix[1][2]; // Gets the element at row 1, column 2 (value: 7)

// Modifying elements
matrix[0][1] = 20; // Changes the element at row 0, column 1 to 20
```

### Iterating Through Two-dimensional Arrays

```java
int[][] matrix = {
    {1, 2, 3, 4},
    {5, 6, 7, 8},
    {9, 10, 11, 12}
};

// Using nested for loops
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println(); // New line after each row
}

// Using enhanced for loops
for (int[] row : matrix) {
    for (int element : row) {
        System.out.print(element + " ");
    }
    System.out.println();
}
```

## Jagged Arrays (Uneven Arrays)

Jagged arrays are multi-dimensional arrays where the member arrays can have different lengths.

```java
// Creating a jagged array
int[][] jaggedArray = new int[3][];
jaggedArray[0] = new int[3];
jaggedArray[1] = new int[5];
jaggedArray[2] = new int[2];

// Initializing a jagged array
int[][] jaggedArray = {
    {1, 2, 3},
    {4, 5, 6, 7, 8},
    {9, 10}
};
```

## Common Array Operations

### 1. Copy an Array

```java
// Using Arrays.copyOf()
int[] original = {1, 2, 3, 4, 5};
int[] copy = Arrays.copyOf(original, original.length);

// Using System.arraycopy()
int[] original = {1, 2, 3, 4, 5};
int[] copy = new int[original.length];
System.arraycopy(original, 0, copy, 0, original.length);

// Using clone()
int[] original = {1, 2, 3, 4, 5};
int[] copy = original.clone();
```

### 2. Sort an Array

```java
int[] numbers = {5, 2, 9, 1, 5, 6};
Arrays.sort(numbers);
// numbers is now {1, 2, 5, 5, 6, 9}
```

### 3. Search in a Sorted Array

```java
int[] numbers = {1, 2, 5, 5, 6, 9};
int index = Arrays.binarySearch(numbers, 5);
// Returns the index of the first occurrence of 5
```

### 4. Fill an Array

```java
int[] numbers = new int[5];
Arrays.fill(numbers, 10);
// numbers is now {10, 10, 10, 10, 10}
```

### 5. Compare Arrays

```java
int[] array1 = {1, 2, 3, 4, 5};
int[] array2 = {1, 2, 3, 4, 5};
boolean areEqual = Arrays.equals(array1, array2);
// Returns true
```

### 6. Convert Array to String

```java
int[] numbers = {1, 2, 3, 4, 5};
String arrayAsString = Arrays.toString(numbers);
// Returns "[1, 2, 3, 4, 5]"
```

## Arrays Utility Class

The `java.util.Arrays` class provides many useful methods for working with arrays.

Some commonly used methods:
- `Arrays.sort(array)`: Sorts the array
- `Arrays.binarySearch(array, value)`: Searches for a value in a sorted array
- `Arrays.fill(array, value)`: Fills the array with the specified value
- `Arrays.copyOf(array, length)`: Copies the array to a new array of the specified length
- `Arrays.equals(array1, array2)`: Compares two arrays for equality
- `Arrays.toString(array)`: Converts the array to a string representation

## Common Array Problems and Solutions

### 1. Finding the Maximum Element

```java
public static int findMax(int[] array) {
    if (array.length == 0) {
        throw new IllegalArgumentException("Array cannot be empty");
    }
    
    int max = array[0];
    for (int i = 1; i < array.length; i++) {
        if (array[i] > max) {
            max = array[i];
        }
    }
    return max;
}
```

### 2. Reversing an Array

```java
public static void reverseArray(int[] array) {
    int left = 0;
    int right = array.length - 1;
    
    while (left < right) {
        // Swap elements
        int temp = array[left];
        array[left] = array[right];
        array[right] = temp;
        
        // Move indices
        left++;
        right--;
    }
}
```

### 3. Merging Two Sorted Arrays

```java
public static int[] mergeSortedArrays(int[] arr1, int[] arr2) {
    int[] result = new int[arr1.length + arr2.length];
    
    int i = 0, j = 0, k = 0;
    
    // Compare elements from both arrays and add the smaller one to result
    while (i < arr1.length && j < arr2.length) {
        if (arr1[i] <= arr2[j]) {
            result[k++] = arr1[i++];
        } else {
            result[k++] = arr2[j++];
        }
    }
    
    // Add remaining elements from arr1, if any
    while (i < arr1.length) {
        result[k++] = arr1[i++];
    }
    
    // Add remaining elements from arr2, if any
    while (j < arr2.length) {
        result[k++] = arr2[j++];
    }
    
    return result;
}
```

### 4. Removing Duplicates from a Sorted Array

```java
public static int removeDuplicates(int[] nums) {
    if (nums.length == 0) {
        return 0;
    }
    
    int i = 0;
    for (int j = 1; j < nums.length; j++) {
        if (nums[j] != nums[i]) {
            i++;
            nums[i] = nums[j];
        }
    }
    
    return i + 1; // Length of array with duplicates removed
}
```

## Common Array-based Data Structures

Arrays form the basis for many advanced data structures. Here are a few:

### 1. ArrayList
A dynamic array that can grow or shrink in size.

```java
import java.util.ArrayList;

ArrayList<Integer> list = new ArrayList<>();
list.add(10);
list.add(20);
list.add(30);

int element = list.get(1); // Gets 20
list.remove(0); // Removes the first element (10)
int size = list.size(); // Gets the size of the list (now 2)
```

### 2. Stack
A Last-In-First-Out (LIFO) data structure.

```java
import java.util.Stack;

Stack<String> stack = new Stack<>();
stack.push("First");
stack.push("Second");
stack.push("Third");

String top = stack.peek(); // Gets "Third" without removing it
String popped = stack.pop(); // Gets and removes "Third"
boolean isEmpty = stack.isEmpty(); // Checks if the stack is empty
```

### 3. Queue
A First-In-First-Out (FIFO) data structure.

```java
import java.util.LinkedList;
import java.util.Queue;

Queue<String> queue = new LinkedList<>();
queue.add("First");
queue.add("Second");
queue.add("Third");

String front = queue.peek(); // Gets "First" without removing it
String removed = queue.poll(); // Gets and removes "First"
boolean isEmpty = queue.isEmpty(); // Checks if the queue is empty
```

## Practice Exercises

1. Write a program to find the second largest element in an array
2. Write a program to rotate an array to the right by k steps
3. Write a program to find all pairs of elements in an array whose sum is equal to a given number
4. Write a program to move all zeros to the end of an array while maintaining the relative order of non-zero elements
5. Write a program to find the intersection of two arrays
6. Write a program to find the majority element in an array (an element that appears more than n/2 times)
7. Write a program to implement a dynamic array that can grow in size

## Next Steps

Proceed to [Object-Oriented Programming](/oop/01-classes-objects.md) to learn how to create and use classes and objects in Java.