# Time and Space Complexity Analysis

Understanding time and space complexity is crucial for analyzing and optimizing algorithms. This guide covers the fundamentals of complexity analysis, Big O notation, common time complexities, and how to analyze algorithms effectively.

## Introduction to Complexity Analysis

Complexity analysis helps us evaluate how an algorithm's performance scales with input size. We focus on two main aspects:

1. **Time Complexity**: How the execution time grows with input size
2. **Space Complexity**: How the memory usage grows with input size

## Big O Notation

Big O notation describes the upper bound of an algorithm's growth rate, focusing on the dominant factors that affect performance as the input size approaches infinity.

### Formal Definition

For functions f(n) and g(n), we say f(n) = O(g(n)) if there exist positive constants c and n₀ such that:

f(n) ≤ c × g(n) for all n ≥ n₀

### Simplified Rules

1. **Ignore Constants**: O(2n) becomes O(n), O(1/2n) becomes O(n)
2. **Ignore Lower-Order Terms**: O(n² + n) becomes O(n²), O(n + log n) becomes O(n)
3. **Focus on Worst Case**: Unless specified otherwise, Big O represents the worst-case scenario

## Common Time Complexities

### O(1) - Constant Time

The algorithm's execution time is independent of the input size.

**Examples**:
- Accessing an array element by index
- Adding an element to the front of a linked list
- Hash table insertions and lookups (average case)

```java
public int getFirstElement(int[] array) {
    return array[0]; // Always takes the same amount of time regardless of array size
}
```

### O(log n) - Logarithmic Time

The algorithm's execution time grows logarithmically with the input size. These algorithms typically reduce the problem size by a factor in each step.

**Examples**:
- Binary search
- Operations on balanced binary search trees
- Certain divide-and-conquer algorithms

```java
public int binarySearch(int[] sortedArray, int target) {
    int left = 0;
    int right = sortedArray.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (sortedArray[mid] == target) {
            return mid;
        } else if (sortedArray[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1;
}
```

### O(n) - Linear Time

The execution time grows linearly with the input size. The algorithm processes each input element a constant number of times.

**Examples**:
- Linear search
- Traversing an array or linked list
- Finding the maximum or minimum in an unsorted array

```java
public int findMax(int[] array) {
    int max = Integer.MIN_VALUE;
    
    for (int value : array) {
        if (value > max) {
            max = value;
        }
    }
    
    return max;
}
```

### O(n log n) - Linearithmic Time

The execution time grows as n log n, which is between linear and quadratic. Often seen in efficient sorting algorithms.

**Examples**:
- Merge sort
- Heap sort
- Quick sort (average case)

```java
public void mergeSort(int[] array, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        
        mergeSort(array, left, mid);
        mergeSort(array, mid + 1, right);
        
        merge(array, left, mid, right);
    }
}
```

### O(n²) - Quadratic Time

The execution time grows quadratically with the input size. Often involves nested loops where each processes the entire input.

**Examples**:
- Bubble sort
- Selection sort
- Insertion sort
- Comparing all pairs of elements in an array

```java
public void bubbleSort(int[] array) {
    int n = array.length;
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (array[j] > array[j + 1]) {
                // Swap array[j] and array[j + 1]
                int temp = array[j];
                array[j] = array[j + 1];
                array[j + 1] = temp;
            }
        }
    }
}
```

### O(2ⁿ) - Exponential Time

The execution time doubles with each additional input element. These algorithms quickly become impractical for large inputs.

**Examples**:
- Recursive Fibonacci with redundant calculations
- Generating all subsets of a set
- Solving the traveling salesman problem using brute force

```java
public int fibonacci(int n) {
    if (n <= 1) {
        return n;
    }
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

### O(n!) - Factorial Time

The execution time grows factorially with the input size, making these algorithms practical only for very small inputs.

**Examples**:
- Generating all permutations of a list
- Solving the traveling salesman problem using brute force (enumerating all possible routes)

```java
public void generatePermutations(char[] chars, int start) {
    if (start == chars.length - 1) {
        System.out.println(String.valueOf(chars));
        return;
    }
    
    for (int i = start; i < chars.length; i++) {
        // Swap characters at positions start and i
        swap(chars, start, i);
        
        // Recursively generate permutations for the rest
        generatePermutations(chars, start + 1);
        
        // Backtrack
        swap(chars, start, i);
    }
}
```

## Complexity Hierarchy

From best to worst:

O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2ⁿ) < O(n!)

![Complexity Growth](https://www.bigocheatsheet.com/img/big-o-complexity.png)

## Space Complexity

Space complexity measures the amount of memory an algorithm uses relative to the input size.

### Common Space Complexities

#### O(1) - Constant Space

The algorithm uses a fixed amount of memory regardless of input size.

```java
public int sum(int[] array) {
    int total = 0; // Uses constant space regardless of array size
    
    for (int value : array) {
        total += value;
    }
    
    return total;
}
```

#### O(n) - Linear Space

The memory usage grows linearly with the input size.

```java
public int[] duplicate(int[] array) {
    int[] result = new int[array.length]; // Space grows with input size
    
    for (int i = 0; i < array.length; i++) {
        result[i] = array[i];
    }
    
    return result;
}
```

#### O(n²) - Quadratic Space

The memory usage grows quadratically with the input size.

```java
public int[][] createAdjacencyMatrix(int n) {
    int[][] matrix = new int[n][n]; // Space grows as n²
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            matrix[i][j] = 0;
        }
    }
    
    return matrix;
}
```

## Analyzing Algorithms

### Steps to Analyze Time Complexity

1. **Identify Basic Operations**: Determine the fundamental operations that contribute to the running time.
2. **Count Operations**: Count how many times these operations are executed.
3. **Express as a Function of Input Size**: Express the count as a function of the input size.
4. **Apply Big O Simplification**: Apply Big O rules to simplify the expression.

### Common Patterns

#### Loops

The time complexity is usually determined by the number of iterations.

```java
// O(n) - Single loop
for (int i = 0; i < n; i++) {
    // Constant time operations
}

// O(n²) - Nested loops
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        // Constant time operations
    }
}

// O(n log n) - Nested loops with changing bounds
for (int i = 0; i < n; i++) {
    for (int j = 1; j < n; j = j * 2) {
        // Constant time operations
    }
}
```

#### Recursive Functions

The time complexity is influenced by the number of recursive calls and the work done in each call.

```java
// O(n) - Linear recursion
public int factorial(int n) {
    if (n <= 1) {
        return 1;
    }
    return n * factorial(n - 1);
}

// O(2ⁿ) - Exponential recursion
public int fibonacci(int n) {
    if (n <= 1) {
        return n;
    }
    return fibonacci(n - 1) + fibonacci(n - 2);
}

// O(log n) - Logarithmic recursion
public int binarySearch(int[] array, int target, int left, int right) {
    if (left > right) {
        return -1;
    }
    
    int mid = left + (right - left) / 2;
    
    if (array[mid] == target) {
        return mid;
    } else if (array[mid] < target) {
        return binarySearch(array, target, mid + 1, right);
    } else {
        return binarySearch(array, target, left, mid - 1);
    }
}
```

#### Divide and Conquer

For divide and conquer algorithms, use the Master Theorem.

For a recurrence relation of the form:
T(n) = aT(n/b) + O(nᵏ)

Where:
- a = number of subproblems
- b = factor by which the input size is reduced
- k = exponent in the work done outside the recursive calls

The time complexity is:
- O(nᵏ) if a < bᵏ
- O(nᵏ log n) if a = bᵏ
- O(nˡᵒᵍₐ b) if a > bᵏ

Example: For merge sort, a = 2, b = 2, k = 1, so the time complexity is O(n log n).

## Time Complexity of Common Data Structure Operations

### Array

| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Access    | O(1)         | O(1)       |
| Search    | O(n)         | O(n)       |
| Insertion | O(n)         | O(n)       |
| Deletion  | O(n)         | O(n)       |

### ArrayList (Java)

| Operation      | Average Case | Worst Case |
|----------------|--------------|------------|
| Access         | O(1)         | O(1)       |
| Search         | O(n)         | O(n)       |
| Insertion (end)| O(1)*        | O(n)       |
| Insertion (middle) | O(n)     | O(n)       |
| Deletion (end) | O(1)         | O(1)       |
| Deletion (middle) | O(n)      | O(n)       |

*Amortized time complexity due to occasional resizing

### LinkedList (Singly Linked List)

| Operation      | Average Case | Worst Case |
|----------------|--------------|------------|
| Access         | O(n)         | O(n)       |
| Search         | O(n)         | O(n)       |
| Insertion (beginning) | O(1)  | O(1)       |
| Insertion (end) | O(n)*       | O(n)       |
| Insertion (middle) | O(n)     | O(n)       |
| Deletion (beginning) | O(1)   | O(1)       |
| Deletion (end) | O(n)         | O(n)       |
| Deletion (middle) | O(n)      | O(n)       |

*O(1) if we maintain a tail pointer

### Stack

| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Push      | O(1)         | O(1)       |
| Pop       | O(1)         | O(1)       |
| Peek      | O(1)         | O(1)       |
| Search    | O(n)         | O(n)       |

### Queue

| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Enqueue   | O(1)         | O(1)       |
| Dequeue   | O(1)         | O(1)       |
| Peek      | O(1)         | O(1)       |
| Search    | O(n)         | O(n)       |

### Hash Table (HashMap in Java)

| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Insert    | O(1)         | O(n)       |
| Delete    | O(1)         | O(n)       |
| Search    | O(1)         | O(n)       |

### Binary Search Tree (Balanced)

| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Insert    | O(log n)     | O(log n)   |
| Delete    | O(log n)     | O(log n)   |
| Search    | O(log n)     | O(log n)   |

### Binary Search Tree (Unbalanced)

| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Insert    | O(log n)     | O(n)       |
| Delete    | O(log n)     | O(n)       |
| Search    | O(log n)     | O(n)       |

### Heap

| Operation      | Average Case | Worst Case |
|----------------|--------------|------------|
| Find Min/Max   | O(1)         | O(1)       |
| Insert         | O(log n)     | O(log n)   |
| Delete Min/Max | O(log n)     | O(log n)   |
| Extract Min/Max| O(log n)     | O(log n)   |
| Heapify        | O(n)         | O(n)       |

## Time Complexity of Common Algorithms

### Sorting Algorithms

| Algorithm      | Best Case   | Average Case | Worst Case  | Space Complexity |
|----------------|-------------|--------------|-------------|------------------|
| Bubble Sort    | O(n)        | O(n²)        | O(n²)       | O(1)             |
| Selection Sort | O(n²)       | O(n²)        | O(n²)       | O(1)             |
| Insertion Sort | O(n)        | O(n²)        | O(n²)       | O(1)             |
| Merge Sort     | O(n log n)  | O(n log n)   | O(n log n)  | O(n)             |
| Quick Sort     | O(n log n)  | O(n log n)   | O(n²)       | O(log n)*        |
| Heap Sort      | O(n log n)  | O(n log n)   | O(n log n)  | O(1)             |
| Counting Sort  | O(n + k)    | O(n + k)     | O(n + k)    | O(n + k)         |
| Radix Sort     | O(nk)       | O(nk)        | O(nk)       | O(n + k)         |

*Average case space complexity due to recursion stack

### Search Algorithms

| Algorithm      | Best Case | Average Case | Worst Case |
|----------------|-----------|--------------|------------|
| Linear Search  | O(1)      | O(n)         | O(n)       |
| Binary Search  | O(1)      | O(log n)     | O(log n)   |
| Depth-First Search | O(V + E) | O(V + E)  | O(V + E)   |
| Breadth-First Search | O(V + E) | O(V + E) | O(V + E)  |

### Graph Algorithms

| Algorithm          | Time Complexity | Space Complexity |
|--------------------|-----------------|------------------|
| Depth-First Search | O(V + E)        | O(V)             |
| Breadth-First Search | O(V + E)      | O(V)             |
| Dijkstra's Algorithm | O(V² + E) or O((V+E) log V)* | O(V) |
| Bellman-Ford Algorithm | O(V × E)    | O(V)             |
| Floyd-Warshall Algorithm | O(V³)     | O(V²)            |
| Prim's Algorithm   | O(V² + E) or O(E log V)* | O(V)    |
| Kruskal's Algorithm| O(E log E)      | O(V)             |
| Topological Sort   | O(V + E)        | O(V)             |

*Depending on the implementation and data structures used

## Real-World Examples of Algorithm Complexity

### O(1) - Constant Time
- Checking if a number is even or odd
- Accessing a value in a hash table (average case)
- Push/pop operations on a stack

### O(log n) - Logarithmic Time
- Binary search in a sorted array
- Balance operations in AVL trees
- Finding an element in a binary search tree

### O(n) - Linear Time
- Finding the maximum element in an unsorted array
- Linear search in an array
- Counting the number of elements in a linked list

### O(n log n) - Linearithmic Time
- Efficient sorting algorithms (merge sort, heap sort)
- Certain divide-and-conquer algorithms
- Building a heap from an array

### O(n²) - Quadratic Time
- Bubble sort, selection sort, insertion sort
- Finding all pairs of elements in an array
- Simple algorithms for the closest pair of points problem

### O(2ⁿ) - Exponential Time
- Solving the Tower of Hanoi puzzle
- Generating all subsets of a set
- Naive recursive solution to the Fibonacci sequence

### O(n!) - Factorial Time
- Generating all permutations of a list
- Solving the traveling salesman problem with brute force
- Finding all possible arrangements of n items

## Best Practices for Optimization

1. **Choose the Right Data Structure**: Different data structures have different performance characteristics for various operations.

2. **Avoid Nested Loops When Possible**: Nested loops often lead to quadratic time complexity.

3. **Use Efficient Algorithms**: Choose algorithms with better time complexity for your problem.

4. **Memoization and Dynamic Programming**: Avoid redundant calculations by storing and reusing results.

5. **Lazy Evaluation**: Defer computations until they're actually needed.

6. **Early Termination**: Exit loops or recursion as soon as the answer is found.

7. **Space-Time Tradeoff**: Sometimes, using more memory can significantly reduce execution time.

8. **Understand Your Input**: Optimize for the expected input characteristics.

## Conclusion

Understanding time and space complexity is essential for writing efficient code. By analyzing algorithms and choosing appropriate data structures, you can ensure your applications scale well with increasing input sizes. 

Remember that Big O notation provides an upper bound on growth rate, and real-world performance depends on various factors including hardware, language implementation, and input characteristics. Always measure actual performance when optimizing critical code.

## Next Steps

- Learn about [Sorting Algorithms](/dsa/sorting/README.md)
- Explore [Dynamic Programming](/dsa/dynamic-programming/README.md)
- Study [Graph Algorithms](/dsa/graphs/README.md)