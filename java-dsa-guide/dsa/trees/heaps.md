# Heaps

A heap is a specialized tree-based data structure that satisfies the heap property. This guide covers the types of heaps, their implementations, operations, and applications in Java.

## Introduction to Heaps

A heap is a complete binary tree where each node satisfies a specific ordering property relative to its children. There are two main types of heaps:

1. **Min Heap**: For every node, the value of the node is less than or equal to the values of its children. The root contains the minimum element in the heap.
2. **Max Heap**: For every node, the value of the node is greater than or equal to the values of its children. The root contains the maximum element in the heap.

## Properties of Heaps

1. **Complete Binary Tree**: A binary tree in which all levels are completely filled except possibly the last level, which is filled from left to right.
2. **Heap Property**: Follows either min-heap or max-heap ordering.
3. **Efficient Implementation**: Heaps can be efficiently represented as arrays, which saves memory and allows for efficient operations.

## Array Representation of Heaps

In an array representation of a binary heap:
- The root element is at index 0
- For a node at index `i`:
  - Its left child is at index `2*i + 1`
  - Its right child is at index `2*i + 2`
  - Its parent is at index `(i-1)/2` (integer division)

This representation doesn't require explicit pointers between nodes, making it memory-efficient.

```
        0
       / \
      1   2
     / \ / \
    3  4 5  6
```

This heap can be represented as the array: `[0, 1, 2, 3, 4, 5, 6]`

## Min Heap Implementation

```java
public class MinHeap {
    private int[] heap;
    private int size;
    private int capacity;
    
    public MinHeap(int capacity) {
        this.capacity = capacity;
        this.size = 0;
        heap = new int[capacity];
    }
    
    // Helper methods to get indices
    private int parent(int i) { return (i - 1) / 2; }
    private int leftChild(int i) { return 2 * i + 1; }
    private int rightChild(int i) { return 2 * i + 2; }
    
    // Helper method to swap elements
    private void swap(int i, int j) {
        int temp = heap[i];
        heap[i] = heap[j];
        heap[j] = temp;
    }
    
    // Check if the heap is empty
    public boolean isEmpty() {
        return size == 0;
    }
    
    // Check if the heap is full
    public boolean isFull() {
        return size == capacity;
    }
    
    // Get the minimum element (root) without removing it
    public int peek() {
        if (isEmpty()) {
            throw new IllegalStateException("Heap is empty");
        }
        return heap[0];
    }
    
    // Insert a new key
    public void insert(int key) {
        if (isFull()) {
            throw new IllegalStateException("Heap is full");
        }
        
        // Insert the new key at the end
        int i = size;
        heap[i] = key;
        size++;
        
        // Fix the min heap property if it is violated (heapify-up)
        while (i != 0 && heap[parent(i)] > heap[i]) {
            swap(i, parent(i));
            i = parent(i);
        }
    }
    
    // Extract the minimum element
    public int extractMin() {
        if (isEmpty()) {
            throw new IllegalStateException("Heap is empty");
        }
        
        // Store the minimum value (root)
        int root = heap[0];
        
        // If there's only one element
        if (size == 1) {
            size--;
            return root;
        }
        
        // Replace root with the last element and remove the last element
        heap[0] = heap[size - 1];
        size--;
        
        // Heapify the root (heapify-down)
        heapify(0);
        
        return root;
    }
    
    // Heapify-down procedure to maintain heap property starting from index i
    private void heapify(int i) {
        int smallest = i;        // Initialize smallest as root
        int left = leftChild(i); // Left child
        int right = rightChild(i); // Right child
        
        // If left child is smaller than root
        if (left < size && heap[left] < heap[smallest]) {
            smallest = left;
        }
        
        // If right child is smaller than smallest so far
        if (right < size && heap[right] < heap[smallest]) {
            smallest = right;
        }
        
        // If smallest is not root
        if (smallest != i) {
            swap(i, smallest);
            // Recursively heapify the affected sub-tree
            heapify(smallest);
        }
    }
    
    // Decrease key value at index i to a new value (new_val must be <= heap[i])
    public void decreaseKey(int i, int newVal) {
        if (i >= size || i < 0) {
            throw new IndexOutOfBoundsException("Invalid index");
        }
        
        if (newVal > heap[i]) {
            throw new IllegalArgumentException("New key is larger than current key");
        }
        
        heap[i] = newVal;
        
        // Fix the min heap property if it is violated (heapify-up)
        while (i != 0 && heap[parent(i)] > heap[i]) {
            swap(i, parent(i));
            i = parent(i);
        }
    }
    
    // Delete key at index i
    public void delete(int i) {
        if (i >= size || i < 0) {
            throw new IndexOutOfBoundsException("Invalid index");
        }
        
        // Decrease the key to the minimum possible value
        decreaseKey(i, Integer.MIN_VALUE);
        
        // Extract the minimum
        extractMin();
    }
    
    // Build a min heap from an array (heapify all non-leaf nodes from bottom up)
    public void buildHeap(int[] array) {
        if (array.length > capacity) {
            throw new IllegalArgumentException("Array size exceeds heap capacity");
        }
        
        // Copy array to heap
        System.arraycopy(array, 0, heap, 0, array.length);
        size = array.length;
        
        // Start from last non-leaf node and heapify all nodes in reverse order
        for (int i = (size / 2) - 1; i >= 0; i--) {
            heapify(i);
        }
    }
    
    // Print the heap
    public void printHeap() {
        System.out.print("Min Heap: ");
        for (int i = 0; i < size; i++) {
            System.out.print(heap[i] + " ");
        }
        System.out.println();
    }
}
```

## Max Heap Implementation

A max heap is very similar to a min heap, but with the opposite ordering property. Here's a simple implementation:

```java
public class MaxHeap {
    private int[] heap;
    private int size;
    private int capacity;
    
    public MaxHeap(int capacity) {
        this.capacity = capacity;
        this.size = 0;
        heap = new int[capacity];
    }
    
    // Helper methods
    private int parent(int i) { return (i - 1) / 2; }
    private int leftChild(int i) { return 2 * i + 1; }
    private int rightChild(int i) { return 2 * i + 2; }
    private void swap(int i, int j) {
        int temp = heap[i];
        heap[i] = heap[j];
        heap[j] = temp;
    }
    
    // Basic operations
    public boolean isEmpty() { return size == 0; }
    public boolean isFull() { return size == capacity; }
    
    // Get the maximum element (root) without removing it
    public int peek() {
        if (isEmpty()) {
            throw new IllegalStateException("Heap is empty");
        }
        return heap[0];
    }
    
    // Insert a new key
    public void insert(int key) {
        if (isFull()) {
            throw new IllegalStateException("Heap is full");
        }
        
        // Insert the new key at the end
        int i = size;
        heap[i] = key;
        size++;
        
        // Fix the max heap property if it is violated (heapify-up)
        while (i != 0 && heap[parent(i)] < heap[i]) {
            swap(i, parent(i));
            i = parent(i);
        }
    }
    
    // Extract the maximum element
    public int extractMax() {
        if (isEmpty()) {
            throw new IllegalStateException("Heap is empty");
        }
        
        int root = heap[0];
        
        if (size == 1) {
            size--;
            return root;
        }
        
        heap[0] = heap[size - 1];
        size--;
        heapify(0);
        
        return root;
    }
    
    // Heapify-down procedure for max heap
    private void heapify(int i) {
        int largest = i;
        int left = leftChild(i);
        int right = rightChild(i);
        
        if (left < size && heap[left] > heap[largest]) {
            largest = left;
        }
        
        if (right < size && heap[right] > heap[largest]) {
            largest = right;
        }
        
        if (largest != i) {
            swap(i, largest);
            heapify(largest);
        }
    }
    
    // Other operations similar to MinHeap, but with reversed comparisons
    
    // Print the heap
    public void printHeap() {
        System.out.print("Max Heap: ");
        for (int i = 0; i < size; i++) {
            System.out.print(heap[i] + " ");
        }
        System.out.println();
    }
}
```

## Priority Queue in Java

Java provides a built-in implementation of a priority queue through the `PriorityQueue` class, which is a min-heap by default.

```java
import java.util.PriorityQueue;

public class PriorityQueueExample {
    public static void main(String[] args) {
        // Min Priority Queue (default)
        PriorityQueue<Integer> minPQ = new PriorityQueue<>();
        
        // Add elements
        minPQ.add(10);
        minPQ.add(30);
        minPQ.add(20);
        minPQ.add(5);
        
        // Print the elements (in order of priority)
        System.out.println("Min Priority Queue:");
        while (!minPQ.isEmpty()) {
            System.out.print(minPQ.poll() + " ");
        }
        System.out.println();
        
        // Max Priority Queue (using a custom comparator)
        PriorityQueue<Integer> maxPQ = new PriorityQueue<>((a, b) -> b - a);
        
        // Add elements
        maxPQ.add(10);
        maxPQ.add(30);
        maxPQ.add(20);
        maxPQ.add(5);
        
        // Print the elements (in order of priority)
        System.out.println("Max Priority Queue:");
        while (!maxPQ.isEmpty()) {
            System.out.print(maxPQ.poll() + " ");
        }
        System.out.println();
    }
}
```

## Heap Operations and Time Complexity

| Operation | Time Complexity | Description |
|-----------|----------------|-------------|
| Build Heap | O(n) | Create a heap from an array of elements |
| Insert | O(log n) | Add a new element to the heap |
| Extract Min/Max | O(log n) | Remove and return the minimum/maximum element |
| Peek | O(1) | Get the minimum/maximum element without removing it |
| Decrease Key | O(log n) | Decrease a key value at a specified index |
| Delete | O(log n) | Delete a key at a specified index |
| Heapify | O(log n) | Maintain the heap property starting from a specific node |

## Applications of Heaps

### 1. Priority Queue

Heaps are the most efficient implementation for a priority queue, which is used in various algorithms:

- **Dijkstra's Algorithm** for finding the shortest path in a graph
- **Prim's Algorithm** for finding minimum spanning trees
- **A* Search Algorithm** for pathfinding
- **Event-driven simulation**, where events are processed based on their time priority

### 2. Heap Sort

Heap sort is a comparison-based sorting algorithm that uses a heap data structure:

```java
public void heapSort(int[] arr) {
    int n = arr.length;
    
    // Build max heap
    for (int i = n / 2 - 1; i >= 0; i--) {
        heapify(arr, n, i);
    }
    
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

// Heapify a subtree rooted at node i
void heapify(int[] arr, int n, int i) {
    int largest = i; // Initialize largest as root
    int left = 2 * i + 1;
    int right = 2 * i + 2;
    
    // If left child is larger than root
    if (left < n && arr[left] > arr[largest]) {
        largest = left;
    }
    
    // If right child is larger than largest so far
    if (right < n && arr[right] > arr[largest]) {
        largest = right;
    }
    
    // If largest is not root
    if (largest != i) {
        int swap = arr[i];
        arr[i] = arr[largest];
        arr[largest] = swap;
        
        // Recursively heapify the affected subtree
        heapify(arr, n, largest);
    }
}
```

### 3. Finding kth Smallest/Largest Element

We can use a heap to efficiently find the kth smallest or largest element in an array:

```java
// Find kth smallest element using a max heap
public int findKthSmallest(int[] arr, int k) {
    if (k <= 0 || k > arr.length) {
        throw new IllegalArgumentException("Invalid k value");
    }
    
    // Create a max heap of size k
    PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
    
    for (int i = 0; i < arr.length; i++) {
        if (maxHeap.size() < k) {
            maxHeap.add(arr[i]);
        } else if (arr[i] < maxHeap.peek()) {
            maxHeap.poll(); // Remove the largest element
            maxHeap.add(arr[i]);
        }
    }
    
    return maxHeap.peek();
}

// Find kth largest element using a min heap
public int findKthLargest(int[] arr, int k) {
    if (k <= 0 || k > arr.length) {
        throw new IllegalArgumentException("Invalid k value");
    }
    
    // Create a min heap of size k
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    
    for (int i = 0; i < arr.length; i++) {
        if (minHeap.size() < k) {
            minHeap.add(arr[i]);
        } else if (arr[i] > minHeap.peek()) {
            minHeap.poll(); // Remove the smallest element
            minHeap.add(arr[i]);
        }
    }
    
    return minHeap.peek();
}
```

### 4. Median of a Stream

Finding the median of a stream of integers can be efficiently implemented using two heaps:

```java
class MedianFinder {
    private PriorityQueue<Integer> minHeap; // Stores larger half of elements
    private PriorityQueue<Integer> maxHeap; // Stores smaller half of elements
    
    public MedianFinder() {
        minHeap = new PriorityQueue<>();
        maxHeap = new PriorityQueue<>((a, b) -> b - a); // Max heap
    }
    
    public void addNum(int num) {
        // Add to max heap first
        maxHeap.offer(num);
        
        // Balance the heaps
        minHeap.offer(maxHeap.poll());
        
        // Ensure max heap has equal or one more element than min heap
        if (minHeap.size() > maxHeap.size()) {
            maxHeap.offer(minHeap.poll());
        }
    }
    
    public double findMedian() {
        if (maxHeap.size() > minHeap.size()) {
            // Odd number of elements, max heap has one extra
            return maxHeap.peek();
        } else {
            // Even number of elements
            return (maxHeap.peek() + minHeap.peek()) / 2.0;
        }
    }
}
```

### 5. Merge K Sorted Lists

Using a min-heap to efficiently merge k sorted lists:

```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; }
}

public ListNode mergeKLists(ListNode[] lists) {
    // Create a min heap based on node values
    PriorityQueue<ListNode> minHeap = new PriorityQueue<>(
        (a, b) -> a.val - b.val);
    
    // Add the first node of each list to the heap
    for (ListNode list : lists) {
        if (list != null) {
            minHeap.offer(list);
        }
    }
    
    // Create a dummy head for the result list
    ListNode dummy = new ListNode(0);
    ListNode tail = dummy;
    
    // Build the merged list
    while (!minHeap.isEmpty()) {
        // Get the smallest node
        ListNode current = minHeap.poll();
        
        // Add it to the result list
        tail.next = current;
        tail = tail.next;
        
        // Add the next node from the same list if exists
        if (current.next != null) {
            minHeap.offer(current.next);
        }
    }
    
    return dummy.next;
}
```

## Heap Variants

### 1. Fibonacci Heap

A more complex heap variant that provides faster amortized running times for many operations. It's more efficient than a binary heap for algorithms that involve decreasing keys frequently.

### 2. Binomial Heap

A heap similar to a binary heap but that provides more efficient merging of heaps. It consists of a collection of binomial trees.

### 3. Pairing Heap

A self-adjusting heap that has good practical performance but with complex theoretic bounds.

### 4. D-ary Heap

A generalization of the binary heap where each node has d children instead of 2. For d > 2, this reduces the height of the tree, which can speed up operations like extract-min/max but makes other operations slower.

## Practice Problems

1. **Implement Heap Sort**: Write a function to sort an array using a heap.
2. **K Closest Points to Origin**: Given an array of points in a 2D plane, find the k closest points to the origin.
3. **Top K Frequent Elements**: Find the k most frequent elements in an array.
4. **Merge K Sorted Arrays**: Merge k sorted arrays into a single sorted array.
5. **Sliding Window Median**: Find the median of all sliding windows of size k in an array.
6. **Implement a Double-Ended Priority Queue**: A queue where you can efficiently find and remove both the minimum and maximum elements.
7. **Minimum Cost to Connect Sticks**: Connect n sticks of different lengths minimizing the total cost.
8. **Ugly Number II**: Find the nth ugly number (numbers with only 2, 3, 5 as prime factors).
9. **Design a Twitter**: Design a simplified version of Twitter with follow and unfollow functionality and a news feed showing the 10 most recent tweets.

## Next Steps

Now that you understand heaps, you can explore other advanced data structures like [Graphs](/dsa/graphs/README.md) or algorithms that frequently use heaps such as [Greedy Algorithms](/dsa/greedy-algorithms/README.md).

## References

- Cormen, T. H., Leiserson, C. E., Rivest, R. L., & Stein, C. (2009). Introduction to Algorithms (3rd ed.). MIT Press.
- Sedgewick, R., & Wayne, K. (2011). Algorithms (4th ed.). Addison-Wesley Professional.
- Java Documentation: [PriorityQueue](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/PriorityQueue.html)