# Trees

Trees are hierarchical data structures consisting of nodes with a parent-child relationship. This guide covers various types of trees, their implementations, and common algorithms used with trees in Java.

## Introduction to Trees

A tree is a widely used abstract data type that represents a hierarchical structure with a set of connected nodes. Each node in the tree can be connected to many children, but must be connected to exactly one parent, except for the root node, which has no parent.

### Tree Terminology

- **Node**: Basic element of a tree, which may contain data and references to other nodes
- **Root**: The topmost node of the tree, which has no parent
- **Parent**: A node that has one or more child nodes
- **Child**: A node that has a parent node
- **Leaf**: A node that has no children
- **Internal Node**: A node that has at least one child
- **Siblings**: Nodes that share the same parent
- **Depth**: The length of the path from the root to a node
- **Height**: The length of the longest path from a node to a leaf
- **Subtree**: A tree consisting of a node and all its descendants
- **Degree**: The number of children a node has
- **Path**: A sequence of nodes where each node is adjacent to the next node

## Binary Trees

A binary tree is a tree in which each node has at most two children, referred to as the left child and the right child.

### Binary Tree Node

```java
class TreeNode {
    int data;
    TreeNode left;
    TreeNode right;
    
    public TreeNode(int data) {
        this.data = data;
        this.left = null;
        this.right = null;
    }
}
```

### Types of Binary Trees

1. **Full Binary Tree**: Every node has 0 or 2 children.
2. **Complete Binary Tree**: All levels are completely filled except possibly the last level, which is filled from left to right.
3. **Perfect Binary Tree**: All internal nodes have two children and all leaf nodes are at the same level.
4. **Balanced Binary Tree**: The height of the left and right subtrees of any node differ by at most 1.
5. **Degenerate Tree**: Every internal node has only one child.

### Tree Traversal Algorithms

#### 1. Depth-First Search (DFS)

**a. Pre-order Traversal (Root-Left-Right)**

```java
public void preOrder(TreeNode root) {
    if (root == null) return;
    
    System.out.print(root.data + " ");  // Process root
    preOrder(root.left);                // Traverse left subtree
    preOrder(root.right);               // Traverse right subtree
}
```

**b. In-order Traversal (Left-Root-Right)**

```java
public void inOrder(TreeNode root) {
    if (root == null) return;
    
    inOrder(root.left);                 // Traverse left subtree
    System.out.print(root.data + " ");  // Process root
    inOrder(root.right);                // Traverse right subtree
}
```

**c. Post-order Traversal (Left-Right-Root)**

```java
public void postOrder(TreeNode root) {
    if (root == null) return;
    
    postOrder(root.left);               // Traverse left subtree
    postOrder(root.right);              // Traverse right subtree
    System.out.print(root.data + " ");  // Process root
}
```

#### 2. Breadth-First Search (BFS) / Level-Order Traversal

```java
public void levelOrder(TreeNode root) {
    if (root == null) return;
    
    Queue<TreeNode> queue = new LinkedList<>();
    queue.add(root);
    
    while (!queue.isEmpty()) {
        TreeNode current = queue.poll();
        System.out.print(current.data + " ");
        
        if (current.left != null) {
            queue.add(current.left);
        }
        if (current.right != null) {
            queue.add(current.right);
        }
    }
}
```

### Properties of Binary Trees

1. The maximum number of nodes at level `L` is `2^L`.
2. The maximum number of nodes in a binary tree with height `H` is `2^(H+1) - 1`.
3. In a non-empty binary tree, the number of leaf nodes is always one more than the number of nodes with two children.

## Binary Search Trees (BST)

A Binary Search Tree is a binary tree where for each node:
- All elements in the left subtree are less than the node's value
- All elements in the right subtree are greater than the node's value
- Both the left and right subtrees are also binary search trees

### BST Operations

#### 1. Searching for a Value

```java
public TreeNode search(TreeNode root, int key) {
    // Base case: root is null or key is present at root
    if (root == null || root.data == key) {
        return root;
    }
    
    // Key is greater than root's data, search in right subtree
    if (root.data < key) {
        return search(root.right, key);
    }
    
    // Key is less than root's data, search in left subtree
    return search(root.left, key);
}
```

#### 2. Insertion

```java
public TreeNode insert(TreeNode root, int key) {
    // If the tree is empty, return a new node
    if (root == null) {
        return new TreeNode(key);
    }
    
    // Otherwise, recur down the tree
    if (key < root.data) {
        root.left = insert(root.left, key);
    } else if (key > root.data) {
        root.right = insert(root.right, key);
    }
    
    // Return the unchanged node pointer
    return root;
}
```

#### 3. Deletion

```java
public TreeNode delete(TreeNode root, int key) {
    // Base case: If the tree is empty
    if (root == null) {
        return null;
    }
    
    // Recursive calls for ancestors of node to be deleted
    if (key < root.data) {
        root.left = delete(root.left, key);
        return root;
    } else if (key > root.data) {
        root.right = delete(root.right, key);
        return root;
    }
    
    // We reach here when root is the node to be deleted
    
    // Case 1: Node with only one child or no child
    if (root.left == null) {
        return root.right;
    } else if (root.right == null) {
        return root.left;
    }
    
    // Case 2: Node with two children
    // Find the inorder successor (smallest in the right subtree)
    root.data = minValue(root.right);
    
    // Delete the inorder successor
    root.right = delete(root.right, root.data);
    
    return root;
}

private int minValue(TreeNode root) {
    int minValue = root.data;
    while (root.left != null) {
        minValue = root.left.data;
        root = root.left;
    }
    return minValue;
}
```

### BST Properties

1. In-order traversal of a BST produces a sorted sequence.
2. The time complexity for search, insert and delete operations is O(h) where h is the height of the tree.
3. For a balanced BST, the height is log(n), so operations take O(log n) time.
4. For a skewed BST (essentially a linked list), the height is n, so operations take O(n) time.

## Self-Balancing Binary Search Trees

Self-balancing binary search trees automatically keep their height small in the face of arbitrary insertions and deletions. This ensures that operations remain O(log n).

### AVL Trees

An AVL tree is a self-balancing binary search tree where the difference in heights of the left and right subtrees of any node (the balance factor) is at most 1.

#### Node with Balance Factor

```java
class AVLNode {
    int data;
    AVLNode left;
    AVLNode right;
    int height; // Height of the node
    
    public AVLNode(int data) {
        this.data = data;
        this.height = 1; // New node is initially at height 1
    }
}
```

#### AVL Tree Operations

1. **Get Height and Balance Factor**

```java
private int height(AVLNode node) {
    if (node == null) {
        return 0;
    }
    return node.height;
}

private int balanceFactor(AVLNode node) {
    if (node == null) {
        return 0;
    }
    return height(node.left) - height(node.right);
}
```

2. **Rotations for Balancing**

```java
// Right Rotation
private AVLNode rightRotate(AVLNode y) {
    AVLNode x = y.left;
    AVLNode T2 = x.right;
    
    // Perform rotation
    x.right = y;
    y.left = T2;
    
    // Update heights
    y.height = Math.max(height(y.left), height(y.right)) + 1;
    x.height = Math.max(height(x.left), height(x.right)) + 1;
    
    return x; // New root
}

// Left Rotation
private AVLNode leftRotate(AVLNode x) {
    AVLNode y = x.right;
    AVLNode T2 = y.left;
    
    // Perform rotation
    y.left = x;
    x.right = T2;
    
    // Update heights
    x.height = Math.max(height(x.left), height(x.right)) + 1;
    y.height = Math.max(height(y.left), height(y.right)) + 1;
    
    return y; // New root
}
```

3. **Insertion**

```java
public AVLNode insert(AVLNode node, int key) {
    // 1. Perform standard BST insertion
    if (node == null) {
        return new AVLNode(key);
    }
    
    if (key < node.data) {
        node.left = insert(node.left, key);
    } else if (key > node.data) {
        node.right = insert(node.right, key);
    } else {
        return node; // Duplicate keys not allowed
    }
    
    // 2. Update height of this ancestor node
    node.height = 1 + Math.max(height(node.left), height(node.right));
    
    // 3. Get the balance factor to check if this node became unbalanced
    int balance = balanceFactor(node);
    
    // Left Left Case
    if (balance > 1 && key < node.left.data) {
        return rightRotate(node);
    }
    
    // Right Right Case
    if (balance < -1 && key > node.right.data) {
        return leftRotate(node);
    }
    
    // Left Right Case
    if (balance > 1 && key > node.left.data) {
        node.left = leftRotate(node.left);
        return rightRotate(node);
    }
    
    // Right Left Case
    if (balance < -1 && key < node.right.data) {
        node.right = rightRotate(node.right);
        return leftRotate(node);
    }
    
    return node;
}
```

### Red-Black Trees

A Red-Black Tree is a self-balancing binary search tree with an extra bit for denoting the color of a node, either red or black.

**Properties of Red-Black Trees**:
1. Every node is either red or black.
2. The root is black.
3. Every leaf (NIL node) is black.
4. If a node is red, then both its children are black.
5. For each node, all simple paths from the node to descendant leaves contain the same number of black nodes.

Red-Black Trees are more complex to implement than AVL trees but typically have better performance in practice for most operations.

Java's TreeMap and TreeSet are implemented using Red-Black Trees.

## B-Trees and B+ Trees

B-Trees and B+ Trees are self-balancing tree data structures that maintain sorted data and allow searches, sequential access, insertions, and deletions in logarithmic time. They are optimized for systems that read and write large blocks of data, such as disk-based storage systems.

**Characteristics**:
1. They are designed to work efficiently with disk-based storage.
2. A B-Tree of order m can have at most m children per node.
3. A B-Tree is balanced - all leaf nodes are at the same depth.
4. B+ Trees store all data in the leaf nodes, with the internal nodes containing only keys.

These trees are commonly used in databases and file systems.

## Trie (Prefix Tree)

A Trie, also called a prefix tree, is a tree data structure used to store a dynamic set or associative array where the keys are usually strings.

### Trie Node

```java
class TrieNode {
    private TrieNode[] children;
    private boolean isEndOfWord;
    
    public TrieNode() {
        children = new TrieNode[26]; // Assuming only lowercase English letters
        isEndOfWord = false;
    }
}
```

### Trie Operations

#### 1. Insertion

```java
public void insert(String word) {
    TrieNode current = root;
    
    for (char ch : word.toCharArray()) {
        int index = ch - 'a';
        if (current.children[index] == null) {
            current.children[index] = new TrieNode();
        }
        current = current.children[index];
    }
    
    current.isEndOfWord = true;
}
```

#### 2. Search

```java
public boolean search(String word) {
    TrieNode current = root;
    
    for (char ch : word.toCharArray()) {
        int index = ch - 'a';
        if (current.children[index] == null) {
            return false;
        }
        current = current.children[index];
    }
    
    return current.isEndOfWord;
}
```

#### 3. Prefix Search

```java
public boolean startsWith(String prefix) {
    TrieNode current = root;
    
    for (char ch : prefix.toCharArray()) {
        int index = ch - 'a';
        if (current.children[index] == null) {
            return false;
        }
        current = current.children[index];
    }
    
    return true;
}
```

### Applications of Tries

1. **Autocomplete/Predictive Text**: Suggest words based on a prefix.
2. **Spell Checking**: Verify if a word exists in a dictionary.
3. **IP Routing Tables**: Find the longest matching prefix.
4. **Word Games**: Find all words in a grid of characters (e.g., Boggle).

## Binary Heap

A binary heap is a complete binary tree where each node satisfies the heap property. There are two types of binary heaps:

1. **Min Heap**: The value of each node is greater than or equal to the value of its parent.
2. **Max Heap**: The value of each node is less than or equal to the value of its parent.

Binary heaps are commonly used to implement priority queues.

### Array Representation of a Binary Heap

A binary heap can be efficiently represented using an array:
- For a node at index `i`:
  - Its left child is at index `2*i + 1`
  - Its right child is at index `2*i + 2`
  - Its parent is at index `(i-1)/2`

### Min Heap Implementation

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
    
    private int parent(int i) {
        return (i - 1) / 2;
    }
    
    private int leftChild(int i) {
        return 2 * i + 1;
    }
    
    private int rightChild(int i) {
        return 2 * i + 2;
    }
    
    private void swap(int i, int j) {
        int temp = heap[i];
        heap[i] = heap[j];
        heap[j] = temp;
    }
    
    // Insert a new key
    public void insert(int key) {
        if (size == capacity) {
            throw new IllegalStateException("Heap is full");
        }
        
        // First insert the new key at the end
        int i = size;
        heap[i] = key;
        size++;
        
        // Fix the min heap property if it is violated
        while (i != 0 && heap[parent(i)] > heap[i]) {
            swap(i, parent(i));
            i = parent(i);
        }
    }
    
    // Extract the minimum element
    public int extractMin() {
        if (size <= 0) {
            throw new IllegalStateException("Heap is empty");
        }
        if (size == 1) {
            size--;
            return heap[0];
        }
        
        // Store the minimum value, and remove it from heap
        int root = heap[0];
        heap[0] = heap[size - 1];
        size--;
        heapify(0);
        
        return root;
    }
    
    // Recursive heapify-down procedure
    private void heapify(int i) {
        int l = leftChild(i);
        int r = rightChild(i);
        int smallest = i;
        
        if (l < size && heap[l] < heap[i]) {
            smallest = l;
        }
        if (r < size && heap[r] < heap[smallest]) {
            smallest = r;
        }
        
        if (smallest != i) {
            swap(i, smallest);
            heapify(smallest);
        }
    }
}
```

### Applications of Heaps

1. **Priority Queue**: Java's PriorityQueue is typically implemented as a min-heap.
2. **Heap Sort**: An efficient comparison-based sorting algorithm.
3. **Dijkstra's Algorithm**: For finding shortest paths in a graph.
4. **Prim's Algorithm**: For finding minimum spanning trees.

## Common Tree Problems

### 1. Check if a Binary Tree is a Binary Search Tree

```java
public boolean isBST(TreeNode root) {
    return isBSTUtil(root, Integer.MIN_VALUE, Integer.MAX_VALUE);
}

private boolean isBSTUtil(TreeNode node, int min, int max) {
    // An empty tree is a BST
    if (node == null) {
        return true;
    }
    
    // False if this node violates the min/max constraints
    if (node.data < min || node.data > max) {
        return false;
    }
    
    // Check the subtrees recursively
    return isBSTUtil(node.left, min, node.data - 1) && 
           isBSTUtil(node.right, node.data + 1, max);
}
```

### 2. Find the Lowest Common Ancestor (LCA) in a Binary Tree

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    // Base case
    if (root == null || root == p || root == q) {
        return root;
    }
    
    // Look for keys in left and right subtrees
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);
    
    // If both keys are found in different subtrees, current node is the LCA
    if (left != null && right != null) {
        return root;
    }
    
    // Otherwise, return the non-null node
    return (left != null) ? left : right;
}
```

### 3. Level Order Traversal (Line by Line)

```java
public void levelOrderLineByLine(TreeNode root) {
    if (root == null) {
        return;
    }
    
    Queue<TreeNode> queue = new LinkedList<>();
    queue.add(root);
    
    while (!queue.isEmpty()) {
        int count = queue.size();
        
        for (int i = 0; i < count; i++) {
            TreeNode current = queue.poll();
            System.out.print(current.data + " ");
            
            if (current.left != null) {
                queue.add(current.left);
            }
            if (current.right != null) {
                queue.add(current.right);
            }
        }
        System.out.println(); // New line after each level
    }
}
```

### 4. Check if Two Trees are Identical

```java
public boolean areIdentical(TreeNode root1, TreeNode root2) {
    // Both trees are empty
    if (root1 == null && root2 == null) {
        return true;
    }
    
    // One tree is empty, the other is not
    if (root1 == null || root2 == null) {
        return false;
    }
    
    // Check if current nodes and their subtrees are identical
    return (root1.data == root2.data) && 
           areIdentical(root1.left, root2.left) && 
           areIdentical(root1.right, root2.right);
}
```

### 5. Print All Paths from Root to Leaf

```java
public void printRootToLeafPaths(TreeNode root) {
    int[] path = new int[1000]; // Assume a max depth
    printPaths(root, path, 0);
}

private void printPaths(TreeNode node, int[] path, int pathLen) {
    if (node == null) {
        return;
    }
    
    // Append this node to the path
    path[pathLen] = node.data;
    pathLen++;
    
    // If it's a leaf, print the path
    if (node.left == null && node.right == null) {
        printArray(path, pathLen);
    } else {
        // Otherwise, try both subtrees
        printPaths(node.left, path, pathLen);
        printPaths(node.right, path, pathLen);
    }
}

private void printArray(int[] path, int pathLen) {
    for (int i = 0; i < pathLen; i++) {
        System.out.print(path[i] + " ");
    }
    System.out.println();
}
```

## Time and Space Complexity

### Time Complexity for Common Tree Operations

| Operation             | Average Case (Balanced BST) | Worst Case (Skewed BST) |
|-----------------------|-----------------------------|-------------------------|
| Search                | O(log n)                    | O(n)                    |
| Insert                | O(log n)                    | O(n)                    |
| Delete                | O(log n)                    | O(n)                    |
| Traversal (any order) | O(n)                        | O(n)                    |
| Height                | O(log n)                    | O(n)                    |

### Space Complexity

For most tree algorithms, the space complexity is O(h) where h is the height of the tree, due to the recursion stack. For a balanced tree, this is O(log n), and for a skewed tree, this is O(n).

## Practice Problems

1. Implement a binary search tree with insertion, deletion, and search operations.
2. Find the diameter of a binary tree (the length of the longest path between any two nodes).
3. Check if a binary tree is height-balanced.
4. Serialize and deserialize a binary tree.
5. Convert a binary search tree to a sorted doubly linked list in-place.
6. Find the kth smallest element in a BST.
7. Implement a Trie with insert, search, and startsWith operations.
8. Build a tree from its inorder and preorder traversals.
9. Implement the A* algorithm using a priority queue (heap).
10. Create an AVL tree with auto-balancing.

## Next Steps

Continue to [Graphs](/dsa/graphs/README.md) to learn about another important data structure.

## References

- Cormen, T. H., Leiserson, C. E., Rivest, R. L., & Stein, C. (2009). Introduction to Algorithms (3rd ed.). MIT Press.
- Sedgewick, R., & Wayne, K. (2011). Algorithms (4th ed.). Addison-Wesley Professional.
- Java Documentation: [https://docs.oracle.com/en/java/](https://docs.oracle.com/en/java/)