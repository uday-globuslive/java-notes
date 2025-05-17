# Searching Algorithms in Java

Searching algorithms are fundamental in computer science and are frequently tested in coding interviews. This guide covers the most important searching algorithms, their implementations in Java, and their time/space complexity analysis.

## Table of Contents
- [Linear Search](#linear-search)
- [Binary Search](#binary-search)
- [Jump Search](#jump-search)
- [Interpolation Search](#interpolation-search)
- [Exponential Search](#exponential-search)
- [Ternary Search](#ternary-search)
- [Applications of Search Algorithms](#applications-of-search-algorithms)
- [Common Interview Questions](#common-interview-questions)

## Linear Search

### Concept
Linear Search is the simplest searching algorithm. It traverses an array sequentially and checks each element until the target element is found or the array ends.

### Implementation

```java
public static int linearSearch(int[] arr, int x) {
    int n = arr.length;
    for (int i = 0; i < n; i++) {
        if (arr[i] == x)
            return i;
    }
    return -1; // Element not found
}
```

### Complexity Analysis
- **Time Complexity**: O(n) in worst case
- **Space Complexity**: O(1)
- **Best for**: Small arrays or unsorted arrays

## Binary Search

### Concept
Binary Search is an efficient algorithm for finding an element in a sorted array. It repeatedly divides the search space in half until the element is found or the search space is empty.

### Implementation (Iterative)

```java
public static int binarySearch(int[] arr, int x) {
    int left = 0, right = arr.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        // Check if x is present at mid
        if (arr[mid] == x)
            return mid;
        
        // If x greater, ignore left half
        if (arr[mid] < x)
            left = mid + 1;
        // If x smaller, ignore right half
        else
            right = mid - 1;
    }
    
    // Element not found
    return -1;
}
```

### Implementation (Recursive)

```java
public static int binarySearchRecursive(int[] arr, int x, int left, int right) {
    if (right >= left) {
        int mid = left + (right - left) / 2;
        
        // If the element is present at the middle
        if (arr[mid] == x)
            return mid;
        
        // If element is smaller than mid, search in left subarray
        if (arr[mid] > x)
            return binarySearchRecursive(arr, x, left, mid - 1);
        
        // Else search in right subarray
        return binarySearchRecursive(arr, x, mid + 1, right);
    }
    
    // Element not found
    return -1;
}

// Helper method to start the recursive binary search
public static int binarySearchRecursive(int[] arr, int x) {
    return binarySearchRecursive(arr, x, 0, arr.length - 1);
}
```

### Finding First and Last Occurrence

```java
// Find the first occurrence of a number in a sorted array
public static int findFirstOccurrence(int[] arr, int x) {
    int left = 0, right = arr.length - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        // If found, look in the left subarray for earlier occurrences
        if (arr[mid] == x) {
            result = mid;
            right = mid - 1;
        }
        // If x is greater, search in right subarray
        else if (arr[mid] < x)
            left = mid + 1;
        // If x is smaller, search in left subarray
        else
            right = mid - 1;
    }
    
    return result;
}

// Find the last occurrence of a number in a sorted array
public static int findLastOccurrence(int[] arr, int x) {
    int left = 0, right = arr.length - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        // If found, look in the right subarray for later occurrences
        if (arr[mid] == x) {
            result = mid;
            left = mid + 1;
        }
        // If x is greater, search in right subarray
        else if (arr[mid] < x)
            left = mid + 1;
        // If x is smaller, search in left subarray
        else
            right = mid - 1;
    }
    
    return result;
}
```

### Complexity Analysis
- **Time Complexity**: O(log n)
- **Space Complexity**: O(1) for iterative, O(log n) for recursive (due to call stack)
- **Best for**: Sorted arrays

## Jump Search

### Concept
Jump Search is an algorithm that works on sorted arrays. It jumps ahead by fixed steps and then performs a linear search when a potential match is found.

### Implementation

```java
public static int jumpSearch(int[] arr, int x) {
    int n = arr.length;
    
    // Finding block size to be jumped
    int step = (int) Math.floor(Math.sqrt(n));
    
    // Finding the block where element may be present
    int prev = 0;
    while (arr[Math.min(step, n) - 1] < x) {
        prev = step;
        step += (int) Math.floor(Math.sqrt(n));
        if (prev >= n)
            return -1;
    }
    
    // Linear search for x in the block
    while (arr[prev] < x) {
        prev++;
        
        // If we reached next block or end of array, element is not present
        if (prev == Math.min(step, n))
            return -1;
    }
    
    // If element is found
    if (arr[prev] == x)
        return prev;
    
    return -1;
}
```

### Complexity Analysis
- **Time Complexity**: O(√n)
- **Space Complexity**: O(1)
- **Best for**: Sorted arrays when binary search might be overkill but linear search is too slow

## Interpolation Search

### Concept
Interpolation Search is an improved variant of binary search, where the value being searched is used to calculate the probable position. It works best when the elements in the array are uniformly distributed.

### Implementation

```java
public static int interpolationSearch(int[] arr, int x) {
    int low = 0, high = arr.length - 1;
    
    while (low <= high && x >= arr[low] && x <= arr[high]) {
        if (low == high) {
            if (arr[low] == x) return low;
            return -1;
        }
        
        // Calculate the probable position using interpolation formula
        int pos = low + (((high - low) / (arr[high] - arr[low])) * (x - arr[low]));
        
        if (arr[pos] == x)
            return pos;
        
        if (arr[pos] < x)
            low = pos + 1;
        else
            high = pos - 1;
    }
    return -1;
}
```

### Complexity Analysis
- **Time Complexity**: O(log log n) average case, O(n) worst case
- **Space Complexity**: O(1)
- **Best for**: Sorted arrays with uniformly distributed values

## Exponential Search

### Concept
Exponential Search is a combination of jumping ahead (by doubling the jump size) to find a range where the element is present, and then using binary search within that range.

### Implementation

```java
public static int exponentialSearch(int[] arr, int x) {
    int n = arr.length;
    
    // If element is present at first position
    if (arr[0] == x)
        return 0;
    
    // Find range for binary search by doubling i
    int i = 1;
    while (i < n && arr[i] <= x)
        i = i * 2;
    
    // Call binary search for the found range
    return binarySearchRecursive(arr, x, i / 2, Math.min(i, n - 1));
}
```

### Complexity Analysis
- **Time Complexity**: O(log n)
- **Space Complexity**: O(1) (if using iterative binary search)
- **Best for**: Unbounded searches or when the size of the array is unknown

## Ternary Search

### Concept
Ternary Search is similar to binary search, but it divides the array into three parts instead of two. It's mainly used for unimodal functions to find the maximum or minimum.

### Implementation

```java
public static int ternarySearch(int[] arr, int x, int left, int right) {
    if (right >= left) {
        // Find the mid1 and mid2
        int mid1 = left + (right - left) / 3;
        int mid2 = right - (right - left) / 3;
        
        // Check if key is present at any mid
        if (arr[mid1] == x)
            return mid1;
        
        if (arr[mid2] == x)
            return mid2;
        
        // Since key is not present at mid, check in which region it is present
        if (x < arr[mid1])
            // The key lies in between left and mid1
            return ternarySearch(arr, x, left, mid1 - 1);
        else if (x > arr[mid2])
            // The key lies in between mid2 and right
            return ternarySearch(arr, x, mid2 + 1, right);
        else
            // The key lies in between mid1 and mid2
            return ternarySearch(arr, x, mid1 + 1, mid2 - 1);
    }
    
    // Key not found
    return -1;
}

// Helper method
public static int ternarySearch(int[] arr, int x) {
    return ternarySearch(arr, x, 0, arr.length - 1);
}
```

### Complexity Analysis
- **Time Complexity**: O(log n base 3), which is theoretically better than binary search but practically often slower due to more comparisons
- **Space Complexity**: O(log n) for recursive implementation
- **Best for**: Finding the maximum or minimum of a unimodal function

## Applications of Search Algorithms

1. **Database Systems**: Binary search is used in database indexing.
2. **Search Engines**: Various search algorithms are used for information retrieval.
3. **Machine Learning**: KNN (K-Nearest Neighbors) uses searching algorithms.
4. **Spell Checkers**: Use search algorithms to find the closest match for a misspelled word.
5. **Route Finding**: GPS and map applications use search algorithms to find the shortest path.

## Search in Java's Collections Framework

Java provides built-in search methods:

1. **Arrays.binarySearch()**: For searching in sorted arrays

```java
int[] arr = {2, 4, 6, 8, 10, 12, 14};
int index = Arrays.binarySearch(arr, 8); // Returns 3
```

2. **Collections.binarySearch()**: For searching in sorted Lists

```java
List<Integer> list = Arrays.asList(2, 4, 6, 8, 10, 12, 14);
int index = Collections.binarySearch(list, 8); // Returns 3
```

3. **HashMap/HashSet**: For O(1) lookups (hash-based searching)

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("banana", 2);
boolean containsKey = map.containsKey("apple"); // Returns true
```

## Common Interview Questions

1. **Implement binary search (both iterative and recursive)**
2. **Find the first and last occurrence of an element in a sorted array**
3. **Find the number of occurrences of an element in a sorted array**
4. **Search in a rotated sorted array**
5. **Find a peak element in an array**
6. **Search in a 2D sorted matrix**
7. **Find the square root of an integer using binary search**
8. **Find the smallest letter greater than target in a sorted array**
9. **Find the element that appears once in a sorted array where all other elements appear twice**
10. **Find the minimum element in a rotated sorted array**

## Practice Problems

1. **Implement binary search in a rotated sorted array**

```java
public static int searchInRotatedArray(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target)
            return mid;
        
        // If left half is sorted
        if (arr[left] <= arr[mid]) {
            // Check if target is in left half
            if (arr[left] <= target && target < arr[mid])
                right = mid - 1;
            else
                left = mid + 1;
        } 
        // If right half is sorted
        else {
            // Check if target is in right half
            if (arr[mid] < target && target <= arr[right])
                left = mid + 1;
            else
                right = mid - 1;
        }
    }
    
    return -1; // Element not found
}
```

2. **Find the minimum element in a rotated sorted array**

```java
public static int findMinimumInRotatedArray(int[] arr) {
    int left = 0;
    int right = arr.length - 1;
    
    // If array is not rotated
    if (arr[left] < arr[right]) 
        return arr[left];
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        // If the mid element is greater than its next element, then mid+1 is the minimum
        if (mid < right && arr[mid] > arr[mid + 1])
            return arr[mid + 1];
        
        // If the mid element is less than its previous element, then mid is the minimum
        if (mid > left && arr[mid] < arr[mid - 1])
            return arr[mid];
        
        // Decide whether to go left or right
        if (arr[mid] > arr[left])
            left = mid + 1;
        else
            right = mid - 1;
    }
    
    return arr[left]; // For edge case when array has only one element
}
```

3. **Find the square root of an integer using binary search**

```java
public static int sqrt(int x) {
    if (x == 0 || x == 1)
        return x;
    
    long left = 1;
    long right = x;
    long result = 0;
    
    while (left <= right) {
        long mid = left + (right - left) / 2;
        
        // Check if mid*mid == x
        if (mid * mid == x)
            return (int) mid;
        
        // If mid*mid < x, try higher values
        if (mid * mid < x) {
            left = mid + 1;
            result = mid; // Update result to the floor value
        } else {
            // If mid*mid > x, try lower values
            right = mid - 1;
        }
    }
    
    return (int) result;
}
```