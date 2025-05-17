# Stacks and Queues

Stacks and queues are fundamental linear data structures that follow specific rules for adding and removing elements. They serve as the building blocks for more complex data structures and algorithms, and understanding them is crucial for solving many programming problems.

## Stacks

A stack is a Last-In-First-Out (LIFO) data structure where elements are added and removed from the same end, called the "top" of the stack.

Think of a stack as a pile of books or plates, where you can only add or remove items from the top.

### Basic Operations

1. **push**: Add an element to the top of the stack
2. **pop**: Remove the element from the top of the stack
3. **peek/top**: View the element at the top without removing it
4. **isEmpty**: Check if the stack is empty
5. **size**: Get the number of elements in the stack

### Implementation Using Arrays

```java
public class ArrayStack {
    private int maxSize;
    private int[] stackArray;
    private int top;
    
    public ArrayStack(int size) {
        maxSize = size;
        stackArray = new int[maxSize];
        top = -1; // Stack is initially empty
    }
    
    public void push(int value) {
        if (isFull()) {
            throw new StackOverflowError("Stack is full");
        }
        stackArray[++top] = value;
    }
    
    public int pop() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        return stackArray[top--];
    }
    
    public int peek() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        return stackArray[top];
    }
    
    public boolean isEmpty() {
        return (top == -1);
    }
    
    public boolean isFull() {
        return (top == maxSize - 1);
    }
    
    public int size() {
        return top + 1;
    }
}
```

### Implementation Using Linked List

```java
public class LinkedStack {
    private Node top;
    private int size;
    
    private class Node {
        int data;
        Node next;
        
        Node(int data) {
            this.data = data;
        }
    }
    
    public LinkedStack() {
        top = null;
        size = 0;
    }
    
    public void push(int value) {
        Node newNode = new Node(value);
        newNode.next = top;
        top = newNode;
        size++;
    }
    
    public int pop() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        int value = top.data;
        top = top.next;
        size--;
        return value;
    }
    
    public int peek() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        return top.data;
    }
    
    public boolean isEmpty() {
        return top == null;
    }
    
    public int size() {
        return size;
    }
}
```

### Using Java's Stack Class

```java
import java.util.Stack;

public class StackExample {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();
        
        // Push elements
        stack.push(10);
        stack.push(20);
        stack.push(30);
        
        // Peek at the top element
        System.out.println("Top element: " + stack.peek()); // 30
        
        // Pop elements
        System.out.println("Popped: " + stack.pop()); // 30
        System.out.println("Popped: " + stack.pop()); // 20
        
        // Check if stack is empty
        System.out.println("Is empty? " + stack.isEmpty()); // false
        
        // Get the size
        System.out.println("Size: " + stack.size()); // 1
    }
}
```

**Note**: Java's `Stack` class extends `Vector`, which isn't ideal for stack operations. For modern applications, consider using `Deque` interface implementations like `ArrayDeque`.

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class ModernStackExample {
    public static void main(String[] args) {
        Deque<Integer> stack = new ArrayDeque<>();
        
        // Push elements
        stack.push(10);
        stack.push(20);
        stack.push(30);
        
        // Peek at the top element
        System.out.println("Top element: " + stack.peek()); // 30
        
        // Pop elements
        System.out.println("Popped: " + stack.pop()); // 30
        System.out.println("Popped: " + stack.pop()); // 20
        
        // Check if stack is empty
        System.out.println("Is empty? " + stack.isEmpty()); // false
        
        // Get the size
        System.out.println("Size: " + stack.size()); // 1
    }
}
```

### Time Complexity

| Operation | Array-based | Linked List-based |
|-----------|-------------|------------------|
| push | O(1) | O(1) |
| pop | O(1) | O(1) |
| peek | O(1) | O(1) |
| isEmpty | O(1) | O(1) |
| size | O(1) | O(1) |

### Common Applications of Stacks

1. **Function Call Management**: Programming languages use stacks to manage function calls and local variables (call stack).
2. **Expression Evaluation**: Evaluating postfix/prefix expressions.
3. **Syntax Parsing**: Compilers and calculators use stacks to parse expressions.
4. **Backtracking Algorithms**: Stacks are used in backtracking algorithms like maze solving, puzzle solving, etc.
5. **Undo Mechanism**: Applications use stacks to implement undo functionality.
6. **Browser History**: The back button in browsers uses a stack to track visited pages.

### Common Stack Problems

#### 1. Balanced Parentheses

```java
public boolean isBalanced(String expression) {
    Stack<Character> stack = new Stack<>();
    
    for (char c : expression.toCharArray()) {
        if (c == '(' || c == '{' || c == '[') {
            stack.push(c);
        } else if (c == ')' || c == '}' || c == ']') {
            if (stack.isEmpty()) {
                return false;
            }
            
            char top = stack.pop();
            if ((c == ')' && top != '(') || 
                (c == '}' && top != '{') || 
                (c == ']' && top != '[')) {
                return false;
            }
        }
    }
    
    return stack.isEmpty();
}
```

#### 2. Evaluate Postfix Expression

```java
public int evaluatePostfix(String expression) {
    Stack<Integer> stack = new Stack<>();
    
    for (char c : expression.toCharArray()) {
        if (Character.isDigit(c)) {
            stack.push(c - '0'); // Convert char to int
        } else {
            int val1 = stack.pop();
            int val2 = stack.pop();
            
            switch (c) {
                case '+':
                    stack.push(val2 + val1);
                    break;
                case '-':
                    stack.push(val2 - val1);
                    break;
                case '*':
                    stack.push(val2 * val1);
                    break;
                case '/':
                    stack.push(val2 / val1);
                    break;
            }
        }
    }
    
    return stack.pop();
}
```

#### 3. Next Greater Element

```java
public int[] nextGreaterElement(int[] arr) {
    int n = arr.length;
    int[] result = new int[n];
    Stack<Integer> stack = new Stack<>();
    
    // Initialize result array with -1 (no greater element)
    Arrays.fill(result, -1);
    
    for (int i = 0; i < n; i++) {
        // Keep popping elements from stack while they are smaller than the current element
        while (!stack.isEmpty() && arr[stack.peek()] < arr[i]) {
            result[stack.pop()] = arr[i];
        }
        
        // Push current index to stack
        stack.push(i);
    }
    
    return result;
}
```

## Queues

A queue is a First-In-First-Out (FIFO) data structure where elements are added at one end (the "rear" or "tail") and removed from the other end (the "front" or "head").

Think of a queue as a line of people waiting for a service, where the first person to join the line is the first one to be served.

### Basic Operations

1. **enqueue**: Add an element to the rear of the queue
2. **dequeue**: Remove the element from the front of the queue
3. **peek/front**: View the element at the front without removing it
4. **isEmpty**: Check if the queue is empty
5. **size**: Get the number of elements in the queue

### Implementation Using Arrays

```java
public class ArrayQueue {
    private int maxSize;
    private int[] queueArray;
    private int front;
    private int rear;
    private int currentSize;
    
    public ArrayQueue(int size) {
        maxSize = size;
        queueArray = new int[maxSize];
        front = 0;
        rear = -1;
        currentSize = 0;
    }
    
    public void enqueue(int value) {
        if (isFull()) {
            throw new IllegalStateException("Queue is full");
        }
        
        // Circular queue implementation
        rear = (rear + 1) % maxSize;
        queueArray[rear] = value;
        currentSize++;
    }
    
    public int dequeue() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        
        int value = queueArray[front];
        front = (front + 1) % maxSize;
        currentSize--;
        return value;
    }
    
    public int peek() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        
        return queueArray[front];
    }
    
    public boolean isEmpty() {
        return (currentSize == 0);
    }
    
    public boolean isFull() {
        return (currentSize == maxSize);
    }
    
    public int size() {
        return currentSize;
    }
}
```

### Implementation Using Linked List

```java
public class LinkedQueue {
    private Node front;
    private Node rear;
    private int size;
    
    private class Node {
        int data;
        Node next;
        
        Node(int data) {
            this.data = data;
            this.next = null;
        }
    }
    
    public LinkedQueue() {
        front = null;
        rear = null;
        size = 0;
    }
    
    public void enqueue(int value) {
        Node newNode = new Node(value);
        
        if (isEmpty()) {
            front = newNode;
        } else {
            rear.next = newNode;
        }
        
        rear = newNode;
        size++;
    }
    
    public int dequeue() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        
        int value = front.data;
        front = front.next;
        
        if (front == null) {
            rear = null; // Queue is now empty
        }
        
        size--;
        return value;
    }
    
    public int peek() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        
        return front.data;
    }
    
    public boolean isEmpty() {
        return (front == null);
    }
    
    public int size() {
        return size;
    }
}
```

### Using Java's Queue Interface

```java
import java.util.LinkedList;
import java.util.Queue;

public class QueueExample {
    public static void main(String[] args) {
        Queue<Integer> queue = new LinkedList<>();
        
        // Add elements
        queue.add(10);
        queue.add(20);
        queue.add(30);
        
        // Peek at the front element
        System.out.println("Front element: " + queue.peek()); // 10
        
        // Remove elements
        System.out.println("Removed: " + queue.remove()); // 10
        System.out.println("Removed: " + queue.remove()); // 20
        
        // Check if queue is empty
        System.out.println("Is empty? " + queue.isEmpty()); // false
        
        // Get the size
        System.out.println("Size: " + queue.size()); // 1
        
        // Add with offer (doesn't throw exception if queue is bounded)
        queue.offer(40);
        
        // Remove with poll (returns null if queue is empty)
        System.out.println("Polled: " + queue.poll()); // 30
        System.out.println("Polled: " + queue.poll()); // 40
    }
}
```

### Circular Queue

A circular queue is an improvement over the simple array-based queue implementation that makes better use of the available space by connecting the end of the queue back to the beginning.

```java
public class CircularQueue {
    private int maxSize;
    private int[] queueArray;
    private int front;
    private int rear;
    private int currentSize;
    
    public CircularQueue(int size) {
        maxSize = size;
        queueArray = new int[maxSize];
        front = 0;
        rear = -1;
        currentSize = 0;
    }
    
    public void enqueue(int value) {
        if (isFull()) {
            throw new IllegalStateException("Queue is full");
        }
        
        rear = (rear + 1) % maxSize;
        queueArray[rear] = value;
        currentSize++;
    }
    
    public int dequeue() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        
        int value = queueArray[front];
        front = (front + 1) % maxSize;
        currentSize--;
        return value;
    }
    
    public int peek() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        
        return queueArray[front];
    }
    
    public boolean isEmpty() {
        return (currentSize == 0);
    }
    
    public boolean isFull() {
        return (currentSize == maxSize);
    }
    
    public int size() {
        return currentSize;
    }
}
```

### Double-Ended Queue (Deque)

A double-ended queue, or deque (pronounced "deck"), is a queue that allows insertion and removal of elements from both ends.

```java
public class ArrayDeque {
    private int maxSize;
    private int[] dequeArray;
    private int front;
    private int rear;
    private int currentSize;
    
    public ArrayDeque(int size) {
        maxSize = size;
        dequeArray = new int[maxSize];
        front = 0;
        rear = -1;
        currentSize = 0;
    }
    
    public void addFront(int value) {
        if (isFull()) {
            throw new IllegalStateException("Deque is full");
        }
        
        // Move front one position back (circularly)
        front = (front - 1 + maxSize) % maxSize;
        dequeArray[front] = value;
        currentSize++;
    }
    
    public void addRear(int value) {
        if (isFull()) {
            throw new IllegalStateException("Deque is full");
        }
        
        // Move rear one position forward (circularly)
        rear = (rear + 1) % maxSize;
        dequeArray[rear] = value;
        currentSize++;
    }
    
    public int removeFront() {
        if (isEmpty()) {
            throw new NoSuchElementException("Deque is empty");
        }
        
        int value = dequeArray[front];
        front = (front + 1) % maxSize;
        currentSize--;
        return value;
    }
    
    public int removeRear() {
        if (isEmpty()) {
            throw new NoSuchElementException("Deque is empty");
        }
        
        int value = dequeArray[rear];
        rear = (rear - 1 + maxSize) % maxSize;
        currentSize--;
        return value;
    }
    
    public int peekFront() {
        if (isEmpty()) {
            throw new NoSuchElementException("Deque is empty");
        }
        
        return dequeArray[front];
    }
    
    public int peekRear() {
        if (isEmpty()) {
            throw new NoSuchElementException("Deque is empty");
        }
        
        return dequeArray[rear];
    }
    
    public boolean isEmpty() {
        return (currentSize == 0);
    }
    
    public boolean isFull() {
        return (currentSize == maxSize);
    }
    
    public int size() {
        return currentSize;
    }
}
```

### Using Java's Deque Interface

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class DequeExample {
    public static void main(String[] args) {
        Deque<Integer> deque = new ArrayDeque<>();
        
        // Add elements to the front
        deque.addFirst(10);
        deque.addFirst(20);
        
        // Add elements to the rear
        deque.addLast(30);
        deque.addLast(40);
        
        // Print the deque
        System.out.println(deque); // [20, 10, 30, 40]
        
        // Peek at both ends
        System.out.println("Front element: " + deque.peekFirst()); // 20
        System.out.println("Rear element: " + deque.peekLast()); // 40
        
        // Remove elements from both ends
        System.out.println("Removed from front: " + deque.removeFirst()); // 20
        System.out.println("Removed from rear: " + deque.removeLast()); // 40
        
        // Check the deque after removals
        System.out.println(deque); // [10, 30]
        
        // Size
        System.out.println("Size: " + deque.size()); // 2
    }
}
```

### Priority Queue

A priority queue is a special type of queue where elements are served based on their priority rather than their arrival order.

```java
import java.util.PriorityQueue;

public class PriorityQueueExample {
    public static void main(String[] args) {
        // Default is min-heap (smallest element has highest priority)
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        
        minHeap.add(30);
        minHeap.add(10);
        minHeap.add(20);
        
        System.out.println("Min heap elements:");
        while (!minHeap.isEmpty()) {
            System.out.println(minHeap.poll()); // 10, 20, 30
        }
        
        // For max-heap (largest element has highest priority)
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
        
        maxHeap.add(30);
        maxHeap.add(10);
        maxHeap.add(20);
        
        System.out.println("\nMax heap elements:");
        while (!maxHeap.isEmpty()) {
            System.out.println(maxHeap.poll()); // 30, 20, 10
        }
    }
}
```

### Time Complexity of Queue Operations

| Operation | Array-based | Linked List-based | Priority Queue |
|-----------|-------------|------------------|----------------|
| enqueue/add | O(1) | O(1) | O(log n) |
| dequeue/remove | O(1) | O(1) | O(log n) |
| peek | O(1) | O(1) | O(1) |
| isEmpty | O(1) | O(1) | O(1) |
| size | O(1) | O(1) | O(1) |

### Common Applications of Queues

1. **Job Scheduling**: Operating systems use queues for process scheduling.
2. **Breadth-First Search (BFS)**: Graph traversal algorithms use queues.
3. **Print Spooling**: Print jobs are queued and processed in order.
4. **Request Handling**: Web servers use queues to handle incoming requests.
5. **Buffers**: Streaming media, keyboard inputs, etc. use buffers (queues).
6. **Event Handling**: Event-driven programming uses event queues.

### Common Queue Problems

#### 1. Implement a Queue Using Stacks

```java
public class QueueUsingStacks {
    private Stack<Integer> stack1; // For enqueue operation
    private Stack<Integer> stack2; // For dequeue operation
    
    public QueueUsingStacks() {
        stack1 = new Stack<>();
        stack2 = new Stack<>();
    }
    
    // Add an element to the queue
    public void enqueue(int value) {
        stack1.push(value);
    }
    
    // Remove an element from the queue
    public int dequeue() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        
        // If stack2 is empty, transfer all elements from stack1
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        
        return stack2.pop();
    }
    
    // Get the front element
    public int peek() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        
        // If stack2 is empty, transfer all elements from stack1
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        
        return stack2.peek();
    }
    
    // Check if the queue is empty
    public boolean isEmpty() {
        return stack1.isEmpty() && stack2.isEmpty();
    }
    
    // Get the size of the queue
    public int size() {
        return stack1.size() + stack2.size();
    }
}
```

#### 2. Sliding Window Maximum

Find the maximum element in each sliding window of size k in an array.

```java
public int[] maxSlidingWindow(int[] nums, int k) {
    if (nums == null || nums.length == 0 || k <= 0) {
        return new int[0];
    }
    
    int n = nums.length;
    int[] result = new int[n - k + 1];
    Deque<Integer> deque = new ArrayDeque<>(); // Stores indices
    
    for (int i = 0; i < n; i++) {
        // Remove elements outside the current window
        while (!deque.isEmpty() && deque.peekFirst() < i - k + 1) {
            deque.pollFirst();
        }
        
        // Remove smaller elements as they won't be the maximum
        while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
            deque.pollLast();
        }
        
        // Add current element's index
        deque.offerLast(i);
        
        // Add maximum to result if we have a complete window
        if (i >= k - 1) {
            result[i - k + 1] = nums[deque.peekFirst()];
        }
    }
    
    return result;
}
```

#### 3. Implement a Stack Using Queues

```java
public class StackUsingQueues {
    private Queue<Integer> q1;
    private Queue<Integer> q2;
    private int top;
    
    public StackUsingQueues() {
        q1 = new LinkedList<>();
        q2 = new LinkedList<>();
    }
    
    // Push element onto stack
    public void push(int x) {
        q1.add(x);
        top = x;
    }
    
    // Pop element from stack
    public int pop() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        
        // Move all elements except the last from q1 to q2
        while (q1.size() > 1) {
            top = q1.remove();
            q2.add(top);
        }
        
        // Remove and return the last element from q1 (stack top)
        int result = q1.remove();
        
        // Swap q1 and q2
        Queue<Integer> temp = q1;
        q1 = q2;
        q2 = temp;
        
        return result;
    }
    
    // Get the top element
    public int top() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        
        return top;
    }
    
    // Check if the stack is empty
    public boolean isEmpty() {
        return q1.isEmpty();
    }
    
    // Get the size of the stack
    public int size() {
        return q1.size();
    }
}
```

## Practice Problems

### Stack Problems

1. **Evaluate infix expressions**: Convert infix expression to postfix and evaluate.
2. **Min Stack**: Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
3. **Stock Span Problem**: Calculate the span of stock's price for all days.
4. **Largest Rectangle in Histogram**: Find the largest rectangular area possible in a given histogram.
5. **Implement a Queue using a Stack**: Implement a queue using a single stack.
6. **Decode String**: Decode a string encoded with a pattern like "3[a]2[bc]" to "aaabcbc".
7. **Simplify Path**: Simplify a Unix-style file path.

### Queue Problems

1. **Generate Binary Numbers**: Generate binary numbers from 1 to n using a queue.
2. **Implement a Stack using a Queue**: Implement a stack using a single queue.
3. **First non-repeating character in a stream**: Find the first non-repeating character from a stream of characters.
4. **Rotting Oranges**: Minimum time required for all oranges to rot.
5. **Task Scheduler**: Arrange tasks with cooling period.
6. **Design a Circular Deque**: Implement a circular deque with all operations.
7. **Find the Winner of a Circular Game**: Find the winner in a circular game where people are eliminated.

## Next Steps

Now that you understand stacks and queues, you can explore more advanced data structures:

1. [Trees](/dsa/trees/README.md): Learn about hierarchical data structures.
2. [Graphs](/dsa/graphs/README.md): Explore non-linear data structures for modeling relationships.
3. [Hash Tables](/dsa/arrays/hash-tables.md): Understand efficient key-value storage.