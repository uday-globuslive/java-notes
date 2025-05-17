# Linked Lists

Linked lists are linear data structures where elements are stored in nodes, and each node points to the next node in the sequence. Unlike arrays, linked lists don't store elements in contiguous memory locations, making insertions and deletions more efficient.

## Types of Linked Lists

### 1. Singly Linked List

In a singly linked list, each node contains data and a reference to the next node in the sequence.

```
[Node 1] -> [Node 2] -> [Node 3] -> null
```

### 2. Doubly Linked List

In a doubly linked list, each node contains data and references to both the next and previous nodes.

```
null <- [Node 1] <-> [Node 2] <-> [Node 3] -> null
```

### 3. Circular Linked List

In a circular linked list, the last node points back to the first node, creating a circular structure.

```
 ______________________________
|                              |
v                              |
[Node 1] -> [Node 2] -> [Node 3]
```

## Basic Structure

### Singly Linked List Node

```java
class Node {
    int data;
    Node next;
    
    public Node(int data) {
        this.data = data;
        this.next = null;
    }
}
```

### Doubly Linked List Node

```java
class Node {
    int data;
    Node next;
    Node prev;
    
    public Node(int data) {
        this.data = data;
        this.next = null;
        this.prev = null;
    }
}
```

## Basic Operations

### 1. Insertion

#### Inserting at the Beginning (Singly Linked List)

```java
public void insertAtBeginning(int data) {
    Node newNode = new Node(data);
    newNode.next = head;
    head = newNode;
}
```

#### Inserting at the End (Singly Linked List)

```java
public void insertAtEnd(int data) {
    Node newNode = new Node(data);
    
    if (head == null) {
        head = newNode;
        return;
    }
    
    Node current = head;
    while (current.next != null) {
        current = current.next;
    }
    
    current.next = newNode;
}
```

#### Inserting at a Specific Position (Singly Linked List)

```java
public void insertAtPosition(int position, int data) {
    // Position is 0-based index
    if (position < 0) {
        throw new IllegalArgumentException("Position cannot be negative");
    }
    
    if (position == 0) {
        insertAtBeginning(data);
        return;
    }
    
    Node newNode = new Node(data);
    Node current = head;
    int currentPosition = 0;
    
    while (current != null && currentPosition < position - 1) {
        current = current.next;
        currentPosition++;
    }
    
    if (current == null) {
        throw new IndexOutOfBoundsException("Position exceeds list length");
    }
    
    newNode.next = current.next;
    current.next = newNode;
}
```

### 2. Deletion

#### Deleting from the Beginning (Singly Linked List)

```java
public void deleteFromBeginning() {
    if (head == null) {
        throw new NoSuchElementException("List is empty");
    }
    
    head = head.next;
}
```

#### Deleting from the End (Singly Linked List)

```java
public void deleteFromEnd() {
    if (head == null) {
        throw new NoSuchElementException("List is empty");
    }
    
    if (head.next == null) {
        head = null;
        return;
    }
    
    Node current = head;
    while (current.next.next != null) {
        current = current.next;
    }
    
    current.next = null;
}
```

#### Deleting a Specific Node (Singly Linked List)

```java
public void deleteNode(int key) {
    if (head == null) {
        return;
    }
    
    if (head.data == key) {
        head = head.next;
        return;
    }
    
    Node current = head;
    while (current.next != null && current.next.data != key) {
        current = current.next;
    }
    
    if (current.next != null) {
        current.next = current.next.next;
    }
}
```

### 3. Traversal

```java
public void printList() {
    Node current = head;
    while (current != null) {
        System.out.print(current.data + " -> ");
        current = current.next;
    }
    System.out.println("null");
}
```

### 4. Search

```java
public boolean search(int key) {
    Node current = head;
    while (current != null) {
        if (current.data == key) {
            return true;
        }
        current = current.next;
    }
    return false;
}
```

### 5. Size

```java
public int size() {
    int count = 0;
    Node current = head;
    while (current != null) {
        count++;
        current = current.next;
    }
    return count;
}
```

## Complete Implementation of a Singly Linked List

```java
public class SinglyLinkedList {
    private Node head;
    
    private static class Node {
        int data;
        Node next;
        
        Node(int data) {
            this.data = data;
            this.next = null;
        }
    }
    
    // Insert at the beginning
    public void insertAtBeginning(int data) {
        Node newNode = new Node(data);
        newNode.next = head;
        head = newNode;
    }
    
    // Insert at the end
    public void insertAtEnd(int data) {
        Node newNode = new Node(data);
        
        if (head == null) {
            head = newNode;
            return;
        }
        
        Node current = head;
        while (current.next != null) {
            current = current.next;
        }
        
        current.next = newNode;
    }
    
    // Insert at a specific position (0-based index)
    public void insertAtPosition(int position, int data) {
        if (position < 0) {
            throw new IllegalArgumentException("Position cannot be negative");
        }
        
        if (position == 0) {
            insertAtBeginning(data);
            return;
        }
        
        Node newNode = new Node(data);
        Node current = head;
        int currentPosition = 0;
        
        while (current != null && currentPosition < position - 1) {
            current = current.next;
            currentPosition++;
        }
        
        if (current == null) {
            throw new IndexOutOfBoundsException("Position exceeds list length");
        }
        
        newNode.next = current.next;
        current.next = newNode;
    }
    
    // Delete from the beginning
    public void deleteFromBeginning() {
        if (head == null) {
            throw new NoSuchElementException("List is empty");
        }
        
        head = head.next;
    }
    
    // Delete from the end
    public void deleteFromEnd() {
        if (head == null) {
            throw new NoSuchElementException("List is empty");
        }
        
        if (head.next == null) {
            head = null;
            return;
        }
        
        Node current = head;
        while (current.next.next != null) {
            current = current.next;
        }
        
        current.next = null;
    }
    
    // Delete a node with a specific value
    public void deleteNode(int key) {
        if (head == null) {
            return;
        }
        
        if (head.data == key) {
            head = head.next;
            return;
        }
        
        Node current = head;
        while (current.next != null && current.next.data != key) {
            current = current.next;
        }
        
        if (current.next != null) {
            current.next = current.next.next;
        }
    }
    
    // Search for a key
    public boolean search(int key) {
        Node current = head;
        while (current != null) {
            if (current.data == key) {
                return true;
            }
            current = current.next;
        }
        return false;
    }
    
    // Get the size of the list
    public int size() {
        int count = 0;
        Node current = head;
        while (current != null) {
            count++;
            current = current.next;
        }
        return count;
    }
    
    // Print the list
    public void printList() {
        Node current = head;
        while (current != null) {
            System.out.print(current.data + " -> ");
            current = current.next;
        }
        System.out.println("null");
    }
    
    // Check if the list is empty
    public boolean isEmpty() {
        return head == null;
    }
    
    // Clear the list
    public void clear() {
        head = null;
    }
    
    // Get the head of the list
    public Node getHead() {
        return head;
    }
}
```

## Complete Implementation of a Doubly Linked List

```java
public class DoublyLinkedList {
    private Node head;
    private Node tail;
    
    private static class Node {
        int data;
        Node next;
        Node prev;
        
        Node(int data) {
            this.data = data;
            this.next = null;
            this.prev = null;
        }
    }
    
    // Insert at the beginning
    public void insertAtBeginning(int data) {
        Node newNode = new Node(data);
        
        if (head == null) {
            head = newNode;
            tail = newNode;
            return;
        }
        
        newNode.next = head;
        head.prev = newNode;
        head = newNode;
    }
    
    // Insert at the end
    public void insertAtEnd(int data) {
        Node newNode = new Node(data);
        
        if (head == null) {
            head = newNode;
            tail = newNode;
            return;
        }
        
        tail.next = newNode;
        newNode.prev = tail;
        tail = newNode;
    }
    
    // Insert at a specific position (0-based index)
    public void insertAtPosition(int position, int data) {
        if (position < 0) {
            throw new IllegalArgumentException("Position cannot be negative");
        }
        
        if (position == 0) {
            insertAtBeginning(data);
            return;
        }
        
        int size = size();
        if (position >= size) {
            insertAtEnd(data);
            return;
        }
        
        Node newNode = new Node(data);
        Node current = head;
        int currentPosition = 0;
        
        while (currentPosition < position - 1) {
            current = current.next;
            currentPosition++;
        }
        
        newNode.next = current.next;
        newNode.prev = current;
        current.next.prev = newNode;
        current.next = newNode;
    }
    
    // Delete from the beginning
    public void deleteFromBeginning() {
        if (head == null) {
            throw new NoSuchElementException("List is empty");
        }
        
        if (head == tail) {
            head = null;
            tail = null;
            return;
        }
        
        head = head.next;
        head.prev = null;
    }
    
    // Delete from the end
    public void deleteFromEnd() {
        if (head == null) {
            throw new NoSuchElementException("List is empty");
        }
        
        if (head == tail) {
            head = null;
            tail = null;
            return;
        }
        
        tail = tail.prev;
        tail.next = null;
    }
    
    // Delete a node with a specific value
    public void deleteNode(int key) {
        if (head == null) {
            return;
        }
        
        if (head.data == key) {
            deleteFromBeginning();
            return;
        }
        
        if (tail.data == key) {
            deleteFromEnd();
            return;
        }
        
        Node current = head.next;
        while (current != null && current.data != key) {
            current = current.next;
        }
        
        if (current != null) {
            current.prev.next = current.next;
            current.next.prev = current.prev;
        }
    }
    
    // Search for a key
    public boolean search(int key) {
        Node current = head;
        while (current != null) {
            if (current.data == key) {
                return true;
            }
            current = current.next;
        }
        return false;
    }
    
    // Get the size of the list
    public int size() {
        int count = 0;
        Node current = head;
        while (current != null) {
            count++;
            current = current.next;
        }
        return count;
    }
    
    // Print the list forward
    public void printForward() {
        Node current = head;
        System.out.print("null <-> ");
        while (current != null) {
            System.out.print(current.data + " <-> ");
            current = current.next;
        }
        System.out.println("null");
    }
    
    // Print the list backward
    public void printBackward() {
        Node current = tail;
        System.out.print("null <-> ");
        while (current != null) {
            System.out.print(current.data + " <-> ");
            current = current.prev;
        }
        System.out.println("null");
    }
    
    // Check if the list is empty
    public boolean isEmpty() {
        return head == null;
    }
    
    // Clear the list
    public void clear() {
        head = null;
        tail = null;
    }
    
    // Get the head of the list
    public Node getHead() {
        return head;
    }
    
    // Get the tail of the list
    public Node getTail() {
        return tail;
    }
}
```

## Common Linked List Problems

### 1. Detecting a Cycle (Floyd's Cycle-Finding Algorithm)

```java
public boolean hasCycle() {
    if (head == null || head.next == null) {
        return false;
    }
    
    Node slow = head;
    Node fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;         // Move one step
        fast = fast.next.next;    // Move two steps
        
        if (slow == fast) {
            return true;
        }
    }
    
    return false;
}
```

### 2. Finding the Middle Node

```java
public Node findMiddle() {
    if (head == null) {
        return null;
    }
    
    Node slow = head;
    Node fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    return slow;
}
```

### 3. Reversing a Linked List

```java
public void reverse() {
    Node prev = null;
    Node current = head;
    Node next = null;
    
    while (current != null) {
        next = current.next;
        current.next = prev;
        prev = current;
        current = next;
    }
    
    head = prev;
}
```

### 4. Merging Two Sorted Lists

```java
public static Node mergeSortedLists(Node list1, Node list2) {
    Node dummy = new Node(0);
    Node current = dummy;
    
    while (list1 != null && list2 != null) {
        if (list1.data <= list2.data) {
            current.next = list1;
            list1 = list1.next;
        } else {
            current.next = list2;
            list2 = list2.next;
        }
        current = current.next;
    }
    
    // Attach the remaining nodes
    if (list1 != null) {
        current.next = list1;
    } else {
        current.next = list2;
    }
    
    return dummy.next;
}
```

### 5. Removing Duplicates

```java
public void removeDuplicates() {
    if (head == null) {
        return;
    }
    
    Set<Integer> seen = new HashSet<>();
    Node current = head;
    Node prev = null;
    
    while (current != null) {
        if (seen.contains(current.data)) {
            prev.next = current.next;
        } else {
            seen.add(current.data);
            prev = current;
        }
        current = current.next;
    }
}
```

### 6. Finding the Nth Node from the End

```java
public Node nthFromEnd(int n) {
    if (head == null || n <= 0) {
        return null;
    }
    
    Node first = head;
    Node second = head;
    
    // Move first pointer n nodes ahead
    for (int i = 0; i < n; i++) {
        if (first == null) {
            return null; // n is greater than the length of the list
        }
        first = first.next;
    }
    
    // Move both pointers until first reaches the end
    while (first != null) {
        first = first.next;
        second = second.next;
    }
    
    return second;
}
```

### 7. Checking if a Linked List is a Palindrome

```java
public boolean isPalindrome() {
    if (head == null || head.next == null) {
        return true;
    }
    
    // Find middle of the list
    Node slow = head;
    Node fast = head;
    
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // Reverse the second half
    Node secondHalf = reverse(slow.next);
    Node firstHalf = head;
    
    // Compare first and second half
    while (secondHalf != null) {
        if (firstHalf.data != secondHalf.data) {
            return false;
        }
        firstHalf = firstHalf.next;
        secondHalf = secondHalf.next;
    }
    
    return true;
}

private Node reverse(Node head) {
    Node prev = null;
    Node current = head;
    Node next;
    
    while (current != null) {
        next = current.next;
        current.next = prev;
        prev = current;
        current = next;
    }
    
    return prev;
}
```

## Time and Space Complexity

### Singly Linked List

| Operation | Time Complexity | Space Complexity |
|-----------|----------------|-----------------|
| Access an element | O(n) | O(1) |
| Insert at beginning | O(1) | O(1) |
| Insert at end (with tail pointer) | O(1) | O(1) |
| Insert at end (without tail pointer) | O(n) | O(1) |
| Insert at position | O(n) | O(1) |
| Delete from beginning | O(1) | O(1) |
| Delete from end (with tail pointer) | O(n) | O(1) |
| Delete from end (without tail pointer) | O(n) | O(1) |
| Search | O(n) | O(1) |

### Doubly Linked List

| Operation | Time Complexity | Space Complexity |
|-----------|----------------|-----------------|
| Access an element | O(n) | O(1) |
| Insert at beginning | O(1) | O(1) |
| Insert at end | O(1) | O(1) |
| Insert at position | O(n) | O(1) |
| Delete from beginning | O(1) | O(1) |
| Delete from end | O(1) | O(1) |
| Delete at position | O(n) | O(1) |
| Search | O(n) | O(1) |

## Advantages and Disadvantages

### Advantages

1. **Dynamic Size**: Linked lists can grow or shrink during runtime without needing to define an initial size.
2. **Efficient Insertions and Deletions**: Operations at the beginning (and end for doubly linked lists) are O(1).
3. **No Memory Wastage**: Memory is allocated as needed.
4. **Implementation Flexibility**: Can implement various data structures like stacks, queues, etc.

### Disadvantages

1. **Random Access**: Accessing an element requires O(n) time, unlike arrays where it's O(1).
2. **Extra Memory**: Requires extra memory for storing pointers/references.
3. **Reverse Traversal**: Singly linked lists can't be traversed backwards efficiently.
4. **Cache Performance**: Linked lists have poor locality of reference compared to arrays.

## Applications

1. **Implementation of Stacks and Queues**: Linked lists are used to implement these abstract data types.
2. **Image Viewer**: Previous and next images can be accessed using doubly linked lists.
3. **Music Player**: Song playlists with previous and next functionality.
4. **Browser History**: Forward and backward navigation.
5. **Undo Functionality**: Maintaining a history of operations.
6. **Memory Management**: Allocation and deallocation of memory.
7. **Polynomial Representation**: Each term can be a node in a linked list.

## Practice Problems

1. **Reverse a linked list in groups of k**: Given a linked list, reverse it in groups of k nodes.
2. **Detect and remove loop**: If a linked list contains a loop, detect and remove it.
3. **Add two numbers**: Add two numbers represented by linked lists.
4. **Clone a linked list with random pointers**: Clone a linked list that has a next pointer and a random pointer.
5. **Flatten a multilevel linked list**: Convert a multilevel linked list into a single level linked list.
6. **LRU Cache implementation**: Implement an LRU (Least Recently Used) cache using a doubly linked list.
7. **Merge k sorted linked lists**: Merge k sorted linked lists into a single sorted linked list.
8. **Swap nodes in pairs**: Given a linked list, swap every two adjacent nodes and return its head.

## Next Steps

Now that you have a solid understanding of linked lists, you can move on to:

1. [Stacks and Queues](/dsa/stacks-queues/README.md): Learn how linked lists are used to implement these data structures.
2. [Trees](/dsa/trees/README.md): Hierarchical data structures that often use node-based representations similar to linked lists.
3. [Graphs](/dsa/graphs/README.md): More complex data structures where nodes can have multiple connections.