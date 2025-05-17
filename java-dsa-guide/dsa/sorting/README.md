# Sorting Algorithms in Java

Sorting is one of the most fundamental operations in computer science and is frequently tested in coding interviews. This guide covers the most important sorting algorithms, their implementations in Java, and their time/space complexity analysis.

## Table of Contents
- [Bubble Sort](#bubble-sort)
- [Selection Sort](#selection-sort)
- [Insertion Sort](#insertion-sort)
- [Merge Sort](#merge-sort)
- [Quick Sort](#quick-sort)
- [Heap Sort](#heap-sort)
- [Counting Sort](#counting-sort)
- [Radix Sort](#radix-sort)
- [Java's Built-in Sorting Methods](#javas-built-in-sorting-methods)
- [When to Use Each Algorithm](#when-to-use-each-algorithm)
- [Common Interview Questions](#common-interview-questions)

## Bubble Sort

### Concept
Bubble Sort repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order. The pass through the list is repeated until the list is sorted.

### Implementation

```java
public static void bubbleSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n-1; i++) {
        for (int j = 0; j < n-i-1; j++) {
            if (arr[j] > arr[j+1]) {
                // swap arr[j+1] and arr[j]
                int temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
    }
}
```

### Optimization
We can optimize bubble sort by stopping the algorithm if the inner loop didn't cause any swap.

```java
public static void optimizedBubbleSort(int[] arr) {
    int n = arr.length;
    boolean swapped;
    for (int i = 0; i < n-1; i++) {
        swapped = false;
        for (int j = 0; j < n-i-1; j++) {
            if (arr[j] > arr[j+1]) {
                // swap arr[j+1] and arr[j]
                int temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
                swapped = true;
            }
        }
        // If no swapping occurred in this pass, array is sorted
        if (!swapped) break;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n²) in worst and average case, O(n) in best case (when array is already sorted and using optimized version)
- **Space Complexity**: O(1) - constant extra space
- **Stability**: Stable

## Selection Sort

### Concept
Selection Sort divides the array into a sorted and an unsorted region. It repeatedly selects the smallest (or largest) element from the unsorted region and moves it to the sorted region.

### Implementation

```java
public static void selectionSort(int[] arr) {
    int n = arr.length;
    
    for (int i = 0; i < n-1; i++) {
        // Find the minimum element in unsorted array
        int minIdx = i;
        for (int j = i+1; j < n; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }
        
        // Swap the found minimum element with the first element
        int temp = arr[minIdx];
        arr[minIdx] = arr[i];
        arr[i] = temp;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n²) in all cases (best, average, and worst)
- **Space Complexity**: O(1) - constant extra space
- **Stability**: Not stable

## Insertion Sort

### Concept
Insertion Sort builds the sorted array one item at a time. It takes each element from the unsorted part and inserts it into its correct position in the sorted part.

### Implementation

```java
public static void insertionSort(int[] arr) {
    int n = arr.length;
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        
        // Move elements of arr[0..i-1] that are greater than key
        // to one position ahead of their current position
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        arr[j + 1] = key;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n²) in worst and average case, O(n) in best case (when array is already sorted)
- **Space Complexity**: O(1) - constant extra space
- **Stability**: Stable

## Merge Sort

### Concept
Merge Sort is a divide and conquer algorithm. It divides the input array into two halves, recursively sorts them, and then merges the sorted halves.

### Implementation

```java
public static void mergeSort(int[] arr) {
    mergeSort(arr, 0, arr.length - 1);
}

private static void mergeSort(int[] arr, int left, int right) {
    if (left < right) {
        // Find the middle point
        int mid = left + (right - left) / 2;
        
        // Sort first and second halves
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        
        // Merge the sorted halves
        merge(arr, left, mid, right);
    }
}

private static void merge(int[] arr, int left, int mid, int right) {
    // Find sizes of two subarrays to be merged
    int n1 = mid - left + 1;
    int n2 = right - mid;
    
    // Create temp arrays
    int[] L = new int[n1];
    int[] R = new int[n2];
    
    // Copy data to temp arrays
    for (int i = 0; i < n1; i++)
        L[i] = arr[left + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[mid + 1 + j];
    
    // Merge the temp arrays
    
    // Initial indices of first and second subarrays
    int i = 0, j = 0;
    
    // Initial index of merged subarray
    int k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }
    
    // Copy remaining elements of L[] if any
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }
    
    // Copy remaining elements of R[] if any
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n log n) in all cases (best, average, and worst)
- **Space Complexity**: O(n) - requires additional space for the temporary arrays
- **Stability**: Stable

## Quick Sort

### Concept
Quick Sort is also a divide and conquer algorithm. It picks an element as a pivot and partitions the array around the pivot such that elements smaller than the pivot are to its left and elements greater than the pivot are to its right.

### Implementation

```java
public static void quickSort(int[] arr) {
    quickSort(arr, 0, arr.length - 1);
}

private static void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        // Partition the array and get the pivot index
        int pivotIndex = partition(arr, low, high);
        
        // Recursively sort elements before and after partition
        quickSort(arr, low, pivotIndex - 1);
        quickSort(arr, pivotIndex + 1, high);
    }
}

private static int partition(int[] arr, int low, int high) {
    // Choose the rightmost element as pivot
    int pivot = arr[high];
    
    // Index of smaller element
    int i = low - 1;
    
    for (int j = low; j < high; j++) {
        // If current element is smaller than the pivot
        if (arr[j] < pivot) {
            i++;
            
            // Swap arr[i] and arr[j]
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }
    
    // Swap arr[i+1] and arr[high] (or pivot)
    int temp = arr[i + 1];
    arr[i + 1] = arr[high];
    arr[high] = temp;
    
    return i + 1;
}
```

### Optimization
There are several optimizations for QuickSort:
1. **Choosing a better pivot**: Use median-of-three (first, middle, last element)
2. **Randomized QuickSort**: Choose a random pivot to avoid worst-case scenarios
3. **Hybrid approaches**: Use insertion sort for small subarrays

### Complexity Analysis
- **Time Complexity**: O(n log n) in average and best case, O(n²) in worst case
- **Space Complexity**: O(log n) for the recursive call stack
- **Stability**: Not stable

## Heap Sort

### Concept
Heap Sort uses a binary heap data structure to sort elements. It builds a max-heap from the input data, then repeatedly extracts the maximum element from the heap and rebuilds the heap.

### Implementation

```java
public static void heapSort(int[] arr) {
    int n = arr.length;
    
    // Build heap (rearrange array)
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);
    
    // Extract elements from heap one by one
    for (int i = n - 1; i > 0; i--) {
        // Move current root to end
        int temp = arr[0];
        arr[0] = arr[i];
        arr[i] = temp;
        
        // Call max heapify on the reduced heap
        heapify(arr, i, 0);
    }
}

// To heapify a subtree rooted with node i
private static void heapify(int[] arr, int n, int i) {
    int largest = i; // Initialize largest as root
    int left = 2 * i + 1; // left = 2*i + 1
    int right = 2 * i + 2; // right = 2*i + 2
    
    // If left child is larger than root
    if (left < n && arr[left] > arr[largest])
        largest = left;
    
    // If right child is larger than largest so far
    if (right < n && arr[right] > arr[largest])
        largest = right;
    
    // If largest is not root
    if (largest != i) {
        int swap = arr[i];
        arr[i] = arr[largest];
        arr[largest] = swap;
        
        // Recursively heapify the affected sub-tree
        heapify(arr, n, largest);
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n log n) in all cases (best, average, and worst)
- **Space Complexity**: O(1) - constant extra space (in-place sorting)
- **Stability**: Not stable

## Counting Sort

### Concept
Counting Sort is an integer sorting algorithm that works by counting the number of occurrences of each unique element in the input array, and using this information to reconstruct a sorted array.

### Implementation

```java
public static void countingSort(int[] arr) {
    int n = arr.length;
    
    // Find the maximum value in arr[]
    int max = arr[0];
    for (int i = 1; i < n; i++) {
        if (arr[i] > max)
            max = arr[i];
    }
    
    // Create a count array to store count of individual
    // elements and initialize count array as 0
    int[] count = new int[max + 1];
    
    // Store count of each element
    for (int i = 0; i < n; i++)
        count[arr[i]]++;
    
    // Change count[i] so that count[i] now contains
    // actual position of this element in output array
    for (int i = 1; i <= max; i++)
        count[i] += count[i - 1];
    
    // Build the output character array
    int[] output = new int[n];
    for (int i = n - 1; i >= 0; i--) {
        output[count[arr[i]] - 1] = arr[i];
        count[arr[i]]--;
    }
    
    // Copy the output array to arr[]
    for (int i = 0; i < n; i++)
        arr[i] = output[i];
}
```

### Complexity Analysis
- **Time Complexity**: O(n + k) where n is the size of the input array and k is the range of input
- **Space Complexity**: O(n + k)
- **Stability**: Stable

## Radix Sort

### Concept
Radix Sort sorts integers by processing individual digits. It processes digits from the least significant digit to the most significant digit.

### Implementation

```java
public static void radixSort(int[] arr) {
    // Find the maximum number to know number of digits
    int max = getMax(arr);
    
    // Do counting sort for every digit
    for (int exp = 1; max / exp > 0; exp *= 10)
        countingSortByDigit(arr, exp);
}

private static int getMax(int[] arr) {
    int max = arr[0];
    for (int i = 1; i < arr.length; i++)
        if (arr[i] > max)
            max = arr[i];
    return max;
}

private static void countingSortByDigit(int[] arr, int exp) {
    int n = arr.length;
    int[] output = new int[n]; // output array
    int[] count = new int[10]; // count array for digits 0-9
    
    // Store count of occurrences in count[]
    for (int i = 0; i < n; i++)
        count[(arr[i] / exp) % 10]++;
    
    // Change count[i] so that count[i] now contains
    // actual position of this digit in output[]
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
```

### Complexity Analysis
- **Time Complexity**: O(d * (n + k)) where d is the number of digits, n is the size of the input array and k is the range of input (typically k = 10 for decimal numbers)
- **Space Complexity**: O(n + k)
- **Stability**: Stable

## Java's Built-in Sorting Methods

Java provides built-in sorting methods that are optimized and should be preferred in most practical applications:

1. **Arrays.sort()** - Uses a dual-pivot Quicksort for primitive types, and TimSort (a hybrid of merge sort and insertion sort) for objects

```java
// For primitive arrays
int[] arr = {5, 2, 9, 1, 5, 6};
Arrays.sort(arr); // Sort entire array

// Sort a range of the array (fromIndex inclusive, toIndex exclusive)
Arrays.sort(arr, 2, 5); // Sorts arr[2], arr[3], arr[4]
```

2. **Collections.sort()** - For Lists, uses TimSort

```java
List<Integer> list = new ArrayList<>(Arrays.asList(5, 2, 9, 1, 5, 6));
Collections.sort(list); // Sort the entire list
```

3. **Custom Comparator** - For custom ordering

```java
// Sort in descending order
Integer[] arr = {5, 2, 9, 1, 5, 6};
Arrays.sort(arr, Collections.reverseOrder());

// Sort a list of custom objects
List<Person> people = new ArrayList<>();
// ... add people
Collections.sort(people, (p1, p2) -> p1.getAge() - p2.getAge()); // Sort by age
```

## When to Use Each Algorithm

| Algorithm     | Best Use Case                                   | Avoid When                           |
|---------------|--------------------------------------------------|--------------------------------------|
| Bubble Sort   | Small datasets, nearly sorted data               | Large datasets                       |
| Selection Sort| Small datasets, minimizing writes                | Large datasets, need for stability   |
| Insertion Sort| Small datasets, online sorting (stream of data)  | Large unsorted datasets              |
| Merge Sort    | Guaranteed O(n log n), external sorting          | Memory constraints, small datasets   |
| Quick Sort    | In-memory sorting, average case performance      | Worried about worst-case O(n²)       |
| Heap Sort     | Memory constraints, guaranteed O(n log n)        | Need for stability                   |
| Counting Sort | Small range of integers                          | Large range of values                |
| Radix Sort    | Fixed-length integers or strings                 | Floating point numbers               |

## Common Interview Questions

1. **Implement a specific sorting algorithm (usually quicksort or mergesort)**
2. **Find the kth largest/smallest element in an array** (can be solved using QuickSelect, a variant of QuickSort)
3. **Sort a nearly sorted array efficiently** (Insertion sort works well)
4. **Sort an array with only a few unique elements** (Counting sort is efficient)
5. **Explain when you would use different sorting algorithms**
6. **Custom sort based on specific criteria**
7. **Sort an array where each element is at most k positions away from its correct position** (Use a min-heap for O(n log k) time)
8. **Sort a linked list** (Usually implemented with merge sort due to the sequential access nature of linked lists)

## Practice Problems

1. Implement a function to find the kth largest element in an array
2. Sort an array of 0s, 1s, and 2s (Dutch National Flag problem)
3. Merge k sorted arrays
4. Sort a linked list in O(n log n) time
5. Sort an array according to another array (custom ordering)