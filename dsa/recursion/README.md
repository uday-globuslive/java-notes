# Recursion in Java

Recursion is a powerful programming technique where a function calls itself to solve a smaller instance of the same problem. It's a fundamental concept in computer science and is particularly important for solving problems in data structures and algorithms.

## Table of Contents
- [Understanding Recursion](#understanding-recursion)
- [Anatomy of a Recursive Function](#anatomy-of-a-recursive-function)
- [Types of Recursion](#types-of-recursion)
- [Common Recursive Patterns](#common-recursive-patterns)
- [Recursion vs Iteration](#recursion-vs-iteration)
- [Common Mistakes and Pitfalls](#common-mistakes-and-pitfalls)
- [Optimization Techniques](#optimization-techniques)
- [Common Interview Problems](#common-interview-problems)
- [Practice Problems](#practice-problems)

## Understanding Recursion

Recursion is based on the concept of **self-similarity**, where a problem can be broken down into smaller versions of the same problem. This approach follows the **divide-and-conquer** paradigm.

### Key Concepts

1. **Base Case**: The simplest instance of a problem that can be solved directly without further recursion.
2. **Recursive Case**: The part where the function calls itself with a simpler or smaller input.
3. **Call Stack**: The memory structure that keeps track of function calls and their local variables.

## Anatomy of a Recursive Function

A properly structured recursive function has:

1. **Base case(s)**: Conditions that stop the recursion
2. **Recursive case(s)**: Where the function calls itself
3. **State change**: Modification of parameters to move toward the base case

```java
public type recursiveFunction(parameters) {
    // Base case(s)
    if (baseCondition) {
        return baseValue;
    }
    
    // Process current step
    // ...
    
    // Recursive case(s) with state change
    return recursiveFunction(modifiedParameters);
}
```

## Types of Recursion

### 1. Direct Recursion

A function calls itself directly.

```java
public static int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```

### 2. Indirect Recursion (Mutual Recursion)

Two or more functions call each other in a cycle.

```java
public static boolean isEven(int n) {
    if (n == 0) return true;
    return isOdd(n - 1);
}

public static boolean isOdd(int n) {
    if (n == 0) return false;
    return isEven(n - 1);
}
```

### 3. Tail Recursion

The recursive call is the last operation in the function, and its result is returned directly.

```java
// Non-tail recursive factorial
public static int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1); // More operations after recursion
}

// Tail recursive factorial
public static int factorialTail(int n, int accumulator) {
    if (n <= 1) return accumulator;
    return factorialTail(n - 1, n * accumulator); // Last operation is recursion
}

// Public interface
public static int factorial(int n) {
    return factorialTail(n, 1);
}
```

### 4. Linear Recursion

The function calls itself once per invocation.

```java
public static int sumArray(int[] arr, int index) {
    if (index >= arr.length) return 0;
    return arr[index] + sumArray(arr, index + 1);
}
```

### 5. Multiple Recursion

The function calls itself multiple times per invocation.

```java
public static int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

## Common Recursive Patterns

### 1. Divide and Conquer

Break a problem into smaller subproblems, solve them recursively, and combine their solutions.

```java
// Merge Sort
public static void mergeSort(int[] arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        
        // Divide
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        
        // Conquer (Combine)
        merge(arr, left, mid, right);
    }
}
```

### 2. Backtracking

Build a solution incrementally and abandon a partial solution if it cannot lead to a valid result.

```java
// Generate all permutations of a string
public static void generatePermutations(char[] chars, int currentIndex) {
    if (currentIndex == chars.length - 1) {
        System.out.println(String.valueOf(chars));
        return;
    }
    
    for (int i = currentIndex; i < chars.length; i++) {
        // Swap characters
        swap(chars, currentIndex, i);
        
        // Recurse on the next position
        generatePermutations(chars, currentIndex + 1);
        
        // Backtrack (undo the swap)
        swap(chars, currentIndex, i);
    }
}

private static void swap(char[] chars, int i, int j) {
    char temp = chars[i];
    chars[i] = chars[j];
    chars[j] = temp;
}
```

### 3. Dynamic Programming with Memoization

Store previously computed results to avoid redundant calculations.

```java
// Fibonacci with memoization
public static int fibMemoized(int n, Integer[] memo) {
    if (memo[n] != null) return memo[n];
    
    if (n <= 1) return n;
    
    memo[n] = fibMemoized(n - 1, memo) + fibMemoized(n - 2, memo);
    return memo[n];
}

// Public interface
public static int fibonacci(int n) {
    Integer[] memo = new Integer[n + 1];
    return fibMemoized(n, memo);
}
```

## Recursion vs Iteration

### Advantages of Recursion

1. **Cleaner code** for problems that are naturally recursive (e.g., tree traversal)
2. **Less code** for complex problems
3. **Easier to understand** for naturally recursive problems

### Disadvantages of Recursion

1. **Memory overhead** due to the call stack
2. **Slower execution** due to function call overhead
3. **Risk of stack overflow** for deep recursion

### When to Use Recursion

1. When the problem naturally breaks down recursively (trees, graphs)
2. When using a recursive algorithm like divide-and-conquer
3. When code clarity is more important than performance

### When to Use Iteration

1. When solving linear problems (arrays, lists)
2. When optimizing for performance or memory usage
3. When recursion depth might cause stack overflow

## Common Mistakes and Pitfalls

1. **Missing base case**: Leads to infinite recursion and stack overflow
2. **Incorrect state change**: May not converge to the base case
3. **Inefficient recursive structure**: Leads to duplicate calculations
4. **Stack overflow**: Especially with deep recursion or large inputs
5. **Misunderstanding the call stack**: Not accounting for how recursion builds up and unwinds

## Optimization Techniques

### 1. Tail Recursion Optimization

Convert recursive functions to tail recursive form to enable compiler optimizations. Modern JVMs may convert tail-recursive functions to iterative ones.

### 2. Memoization

Cache results of expensive function calls to avoid redundant calculations.

```java
// Exponential complexity without memoization
public static int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

// Linear complexity with memoization
public static int fibonacciOptimized(int n) {
    int[] memo = new int[n + 1];
    Arrays.fill(memo, -1);
    return fibMemo(n, memo);
}

private static int fibMemo(int n, int[] memo) {
    if (memo[n] != -1) return memo[n];
    if (n <= 1) return n;
    
    memo[n] = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
    return memo[n];
}
```

### 3. Iteration Conversion

Convert recursive functions to iterative ones when appropriate.

```java
// Recursive factorial
public static int factorialRecursive(int n) {
    if (n <= 1) return 1;
    return n * factorialRecursive(n - 1);
}

// Iterative factorial
public static int factorialIterative(int n) {
    int result = 1;
    for (int i = 2; i <= n; i++) {
        result *= i;
    }
    return result;
}
```

## Common Interview Problems

### 1. Factorial

```java
public static int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```

### 2. Fibonacci Sequence

```java
public static int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

### 3. Power Function

```java
public static int power(int base, int exponent) {
    if (exponent == 0) return 1;
    return base * power(base, exponent - 1);
}
```

### 4. Sum of Array Elements

```java
public static int sum(int[] arr, int index) {
    if (index >= arr.length) return 0;
    return arr[index] + sum(arr, index + 1);
}
```

### 5. Binary Search

```java
public static int binarySearch(int[] arr, int target, int left, int right) {
    if (left > right) return -1;
    
    int mid = left + (right - left) / 2;
    
    if (arr[mid] == target) return mid;
    if (arr[mid] > target) return binarySearch(arr, target, left, mid - 1);
    return binarySearch(arr, target, mid + 1, right);
}
```

### 6. Tower of Hanoi

```java
public static void towerOfHanoi(int n, char source, char auxiliary, char destination) {
    if (n == 1) {
        System.out.println("Move disk 1 from " + source + " to " + destination);
        return;
    }
    
    towerOfHanoi(n - 1, source, destination, auxiliary);
    System.out.println("Move disk " + n + " from " + source + " to " + destination);
    towerOfHanoi(n - 1, auxiliary, source, destination);
}
```

### 7. Generate All Subsets (Power Set)

```java
public static void generateSubsets(int[] nums, List<Integer> current, int index, List<List<Integer>> result) {
    // Add the current subset to the result
    result.add(new ArrayList<>(current));
    
    for (int i = index; i < nums.length; i++) {
        // Include nums[i] in the current subset
        current.add(nums[i]);
        
        // Generate all subsets with nums[i] included
        generateSubsets(nums, current, i + 1, result);
        
        // Backtrack - remove nums[i] to generate subsets without it
        current.remove(current.size() - 1);
    }
}
```

### 8. Palindrome Check

```java
public static boolean isPalindrome(String str, int left, int right) {
    if (left >= right) return true;
    if (str.charAt(left) != str.charAt(right)) return false;
    return isPalindrome(str, left + 1, right - 1);
}
```

### 9. String Reversal

```java
public static String reverse(String str) {
    if (str.length() <= 1) return str;
    return reverse(str.substring(1)) + str.charAt(0);
}
```

### 10. Count Paths in a Grid

```java
public static int countPaths(int m, int n) {
    if (m == 1 || n == 1) return 1;
    return countPaths(m - 1, n) + countPaths(m, n - 1);
}
```

## Practice Problems

1. **Calculate the sum of digits of a positive integer**

```java
public static int sumOfDigits(int n) {
    if (n < 10) return n;
    return n % 10 + sumOfDigits(n / 10);
}
```

2. **Count the number of elements in a linked list**

```java
public static int countNodes(Node head) {
    if (head == null) return 0;
    return 1 + countNodes(head.next);
}
```

3. **Find the length of a string**

```java
public static int stringLength(String str) {
    if (str.equals("")) return 0;
    return 1 + stringLength(str.substring(1));
}
```

4. **Calculate the product of array elements**

```java
public static int product(int[] arr, int index) {
    if (index >= arr.length) return 1;
    return arr[index] * product(arr, index + 1);
}
```

5. **Print all permutations of a string**

```java
public static void permute(String str, int l, int r) {
    if (l == r) {
        System.out.println(str);
    } else {
        for (int i = l; i <= r; i++) {
            str = swap(str, l, i);
            permute(str, l + 1, r);
            str = swap(str, l, i); // backtrack
        }
    }
}

public static String swap(String str, int i, int j) {
    char[] charArray = str.toCharArray();
    char temp = charArray[i];
    charArray[i] = charArray[j];
    charArray[j] = temp;
    return String.valueOf(charArray);
}
```

6. **Compute the greatest common divisor (GCD) of two integers**

```java
public static int gcd(int a, int b) {
    if (b == 0) return a;
    return gcd(b, a % b);
}
```

7. **Find the maximum element in an array**

```java
public static int findMax(int[] arr, int index) {
    if (index == arr.length - 1) return arr[index];
    return Math.max(arr[index], findMax(arr, index + 1));
}
```

8. **Check if an array is sorted**

```java
public static boolean isSorted(int[] arr, int index) {
    if (index >= arr.length - 1) return true;
    if (arr[index] > arr[index + 1]) return false;
    return isSorted(arr, index + 1);
}
```

9. **Print numbers from 1 to n**

```java
public static void printAscending(int n) {
    if (n <= 0) return;
    printAscending(n - 1);
    System.out.print(n + " ");
}
```

10. **Print numbers from n to 1**

```java
public static void printDescending(int n) {
    if (n <= 0) return;
    System.out.print(n + " ");
    printDescending(n - 1);
}
```

Remember, when solving recursive problems:
1. Identify the base case(s)
2. Define the recursive case(s)
3. Ensure progress toward the base case
4. Consider optimizations (memoization, tail recursion)
5. Test with small inputs first