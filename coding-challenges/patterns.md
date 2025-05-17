# Common Patterns for Solving Coding Problems

This guide outlines the most common patterns used to solve coding interview problems. Understanding these patterns will help you identify solution approaches for new problems based on similarities to problems you've seen before.

## 1. Sliding Window Pattern

The sliding window pattern is used to perform operations on a specific window size of an array or string.

### When to use:
- Problems involving contiguous subarrays or substrings
- Finding maximum/minimum/average of all subarrays of a specific size
- Finding longest/shortest substring with specific properties

### Identification:
- Problem involves arrays or strings
- Looking for some subrange in the array/string
- Min, max, longest, shortest, contained

### Examples:
1. **Maximum Sum Subarray of Size K**
2. **Longest Substring with K Distinct Characters**
3. **Fruits into Baskets**
4. **Longest Substring Without Repeating Characters**
5. **Minimum Size Subarray Sum**

### Implementation Template:

```java
public int slidingWindowTemplate(String s, ...) {
    // Create a window [left, right]
    int left = 0, right = 0;
    // Define variables to track window state
    // ...
    
    // Result variable
    int result = 0;
    
    // Expand the window
    while (right < s.length()) {
        // Add the right character to window state
        // ...
        
        // Shrink the window if needed
        while (/* window needs shrinking */) {
            // Remove the left character from window state
            // ...
            
            // Move left pointer
            left++;
        }
        
        // Update result based on current window
        result = Math.max(result, right - left + 1);
        
        // Expand window
        right++;
    }
    
    return result;
}
```

### Example Problem (Maximum Sum Subarray of Size K):

```java
public int maxSubArraySum(int[] arr, int k) {
    int maxSum = 0;
    int windowSum = 0;
    int windowStart = 0;
    
    for (int windowEnd = 0; windowEnd < arr.length; windowEnd++) {
        // Add the next element to the window
        windowSum += arr[windowEnd];
        
        // When we hit the window size, find max sum and slide the window
        if (windowEnd >= k - 1) {
            maxSum = Math.max(maxSum, windowSum);
            // Remove the element going out of the window
            windowSum -= arr[windowStart];
            // Slide the window forward
            windowStart++;
        }
    }
    
    return maxSum;
}
```

## 2. Two Pointers or Iterators Pattern

The two pointers pattern uses two pointers to iterate through a data structure.

### When to use:
- Problems involving sorted arrays (or Linked Lists)
- Searching for pairs in a sorted array
- Comparing strings or arrays
- Removing duplicates

### Identification:
- Sorted arrays
- Target sum
- Palindromes
- Comparing two strings/arrays

### Examples:
1. **Two Sum (sorted array)**
2. **Remove Duplicates**
3. **Squaring a Sorted Array**
4. **Triplet Sum to Zero**
5. **Pair with Target Sum**
6. **Dutch National Flag Problem (Sort Colors)**

### Implementation Template:

```java
public int[] twoPointerTemplate(int[] array, ...) {
    int left = 0, right = array.length - 1;
    int[] result = ...;
    
    while (left < right) {
        // Do some calculation with array[left] and array[right]
        
        if (/* condition */) {
            left++;
        } else {
            right--;
        }
    }
    
    return result;
}
```

### Example Problem (Two Sum Sorted):

```java
public int[] twoSum(int[] numbers, int target) {
    int left = 0;
    int right = numbers.length - 1;
    
    while (left < right) {
        int sum = numbers[left] + numbers[right];
        
        if (sum == target) {
            return new int[] {left + 1, right + 1}; // 1-based indexing
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    
    return new int[] {-1, -1}; // No solution found
}
```

## 3. Fast and Slow Pointers Pattern

The fast and slow pointers approach uses two pointers moving at different speeds.

### When to use:
- Problems involving cyclic linked lists or arrays
- Finding middle elements
- Finding if a linked list has a cycle
- Finding cycle length or start

### Identification:
- Linked list problems
- Detecting cycles
- Finding middle element

### Examples:
1. **Linked List Cycle**
2. **Middle of the Linked List**
3. **Palindrome Linked List**
4. **Cycle in a Circular Array**
5. **Happy Number**

### Implementation Template:

```java
public boolean fastSlowPointerTemplate(ListNode head, ...) {
    ListNode slow = head;
    ListNode fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;          // Move slow pointer one step
        fast = fast.next.next;     // Move fast pointer two steps
        
        if (slow == fast) {
            // Cycle detected
            return true;
        }
    }
    
    // No cycle
    return false;
}
```

### Example Problem (Linked List Cycle):

```java
public boolean hasCycle(ListNode head) {
    if (head == null || head.next == null) {
        return false;
    }
    
    ListNode slow = head;
    ListNode fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;          // Move one step
        fast = fast.next.next;     // Move two steps
        
        if (slow == fast) {
            return true;  // Cycle detected
        }
    }
    
    return false;  // No cycle
}
```

## 4. Merge Intervals Pattern

The merge intervals pattern deals with overlapping intervals.

### When to use:
- Problems involving intervals
- Finding overlapping intervals
- Merging intervals
- Inserting intervals

### Identification:
- Interval problems
- Start and end points
- Overlapping ranges

### Examples:
1. **Merge Intervals**
2. **Insert Interval**
3. **Intervals Intersection**
4. **Conflicting Appointments**
5. **Minimum Meeting Rooms**

### Implementation Template:

```java
public List<Interval> mergeIntervalsTemplate(List<Interval> intervals, ...) {
    if (intervals.size() <= 1) {
        return intervals;
    }
    
    // Sort intervals by start time
    Collections.sort(intervals, (a, b) -> a.start - b.start);
    
    List<Interval> result = new ArrayList<>();
    Interval current = intervals.get(0);
    
    for (int i = 1; i < intervals.size(); i++) {
        Interval next = intervals.get(i);
        
        if (current.end >= next.start) {
            // Overlapping intervals, merge them
            current.end = Math.max(current.end, next.end);
        } else {
            // Non-overlapping interval, add to result
            result.add(current);
            current = next;
        }
    }
    
    // Add the last interval
    result.add(current);
    
    return result;
}
```

### Example Problem (Merge Intervals):

```java
public int[][] merge(int[][] intervals) {
    if (intervals.length <= 1) {
        return intervals;
    }
    
    // Sort by start time
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
    
    List<int[]> result = new ArrayList<>();
    int[] currentInterval = intervals[0];
    result.add(currentInterval);
    
    for (int[] interval : intervals) {
        // Get the end of the previous interval
        int currentEnd = currentInterval[1];
        
        // Get the start and end of the next interval
        int nextStart = interval[0];
        int nextEnd = interval[1];
        
        // If overlapping, merge
        if (currentEnd >= nextStart) {
            currentInterval[1] = Math.max(currentEnd, nextEnd);
        } else {
            // Not overlapping, add to result
            currentInterval = interval;
            result.add(currentInterval);
        }
    }
    
    return result.toArray(new int[result.size()][]);
}
```

## 5. Cyclic Sort Pattern

The cyclic sort pattern sorts an array containing numbers in a given range.

### When to use:
- Problems involving arrays containing numbers in a given range
- Problems asking to find missing/duplicate/smallest number

### Identification:
- Array with numbers in range [1...n] or [0...n]
- Finding missing/duplicate numbers

### Examples:
1. **Cyclic Sort**
2. **Find the Missing Number**
3. **Find All Missing Numbers**
4. **Find the Duplicate Number**
5. **Find All Duplicate Numbers**

### Implementation Template:

```java
public void cyclicSortTemplate(int[] nums, ...) {
    int i = 0;
    while (i < nums.length) {
        // Find the correct position for nums[i]
        int correctPosition = nums[i] - 1; // Or some mapping function
        
        // If the element is not at its correct position, swap
        if (nums[i] != nums[correctPosition]) {
            // Swap
            int temp = nums[i];
            nums[i] = nums[correctPosition];
            nums[correctPosition] = temp;
        } else {
            i++;
        }
    }
}
```

### Example Problem (Find the Missing Number):

```java
public int findMissingNumber(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        if (nums[i] < nums.length && nums[i] != nums[nums[i]]) {
            // Swap
            int temp = nums[i];
            nums[i] = nums[temp];
            nums[temp] = temp;
        } else {
            i++;
        }
    }
    
    // Find the first number missing from its index
    for (i = 0; i < nums.length; i++) {
        if (nums[i] != i) {
            return i;
        }
    }
    
    return nums.length;
}
```

## 6. In-place Reversal of a Linked List Pattern

This pattern reverses a linked list in-place.

### When to use:
- Problems involving reversing linked lists
- Problems involving reversing subparts of linked lists

### Identification:
- Linked list reversal
- Reversing a sublist
- Reordering a linked list

### Examples:
1. **Reverse a Linked List**
2. **Reverse a Sub-list**
3. **Reverse Every K-element Sub-list**
4. **Reverse Alternating K-element Sub-list**

### Implementation Template:

```java
public ListNode reverseLinkedListTemplate(ListNode head, ...) {
    ListNode current = head;
    ListNode previous = null;
    ListNode next = null;
    
    while (current != null) {
        // Store next node
        next = current.next;
        
        // Reverse current node's pointer
        current.next = previous;
        
        // Move pointers one position forward
        previous = current;
        current = next;
    }
    
    // Return new head
    return previous;
}
```

### Example Problem (Reverse a Linked List):

```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode current = head;
    ListNode next = null;
    
    while (current != null) {
        // Store next
        next = current.next;
        
        // Reverse current node's pointer
        current.next = prev;
        
        // Move pointers one position ahead
        prev = current;
        current = next;
    }
    
    // prev is the new head
    return prev;
}
```

## 7. Tree BFS Pattern

The Tree Breadth-First Search pattern traverses a tree level by level.

### When to use:
- Problems involving tree level traversal
- Finding the minimum depth of a tree
- Level-order traversal
- Connecting level-order siblings

### Identification:
- Level-by-level traversal
- Level averages
- Level order traversal
- Zigzag traversal

### Examples:
1. **Binary Tree Level Order Traversal**
2. **Zigzag Traversal**
3. **Level Averages in a Binary Tree**
4. **Minimum Depth of a Binary Tree**
5. **Connect Level Order Siblings**

### Implementation Template:

```java
public List<List<Integer>> treeBFSTemplate(TreeNode root, ...) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) {
        return result;
    }
    
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    
    while (!queue.isEmpty()) {
        int levelSize = queue.size();
        List<Integer> currentLevel = new ArrayList<>();
        
        for (int i = 0; i < levelSize; i++) {
            TreeNode currentNode = queue.poll();
            
            // Process current node
            currentLevel.add(currentNode.val);
            
            // Add children to the queue
            if (currentNode.left != null) {
                queue.offer(currentNode.left);
            }
            if (currentNode.right != null) {
                queue.offer(currentNode.right);
            }
        }
        
        result.add(currentLevel);
    }
    
    return result;
}
```

### Example Problem (Binary Tree Level Order Traversal):

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    
    if (root == null) {
        return result;
    }
    
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    
    while (!queue.isEmpty()) {
        int levelSize = queue.size();
        List<Integer> currentLevel = new ArrayList<>();
        
        for (int i = 0; i < levelSize; i++) {
            TreeNode currentNode = queue.poll();
            currentLevel.add(currentNode.val);
            
            if (currentNode.left != null) {
                queue.offer(currentNode.left);
            }
            if (currentNode.right != null) {
                queue.offer(currentNode.right);
            }
        }
        
        result.add(currentLevel);
    }
    
    return result;
}
```

## 8. Tree DFS Pattern

The Tree Depth-First Search pattern explores a tree as far as possible along a branch.

### When to use:
- Problems involving tree path exploration
- Path sums
- Tree diameter
- Binary tree paths

### Identification:
- Path finding
- Sum of paths
- Tree traversal (preorder, inorder, postorder)
- Tree height or depth

### Examples:
1. **Binary Tree Path Sum**
2. **All Paths for a Sum**
3. **Sum of Path Numbers**
4. **Path with Maximum Sum**
5. **Diameter of a Binary Tree**

### Implementation Template:

```java
public void treeDFSTemplate(TreeNode root, ...) {
    if (root == null) {
        return;
    }
    
    // Process current node (preorder)
    // ...
    
    // Traverse left subtree
    treeDFSTemplate(root.left, ...);
    
    // Process current node (inorder)
    // ...
    
    // Traverse right subtree
    treeDFSTemplate(root.right, ...);
    
    // Process current node (postorder)
    // ...
}
```

### Example Problem (Path Sum):

```java
public boolean hasPathSum(TreeNode root, int targetSum) {
    if (root == null) {
        return false;
    }
    
    // If leaf node, check if target sum is reached
    if (root.left == null && root.right == null) {
        return root.val == targetSum;
    }
    
    // Check left and right subtrees
    return hasPathSum(root.left, targetSum - root.val) || 
           hasPathSum(root.right, targetSum - root.val);
}
```

## 9. Two Heaps Pattern

The Two Heaps pattern uses two heaps (min-heap and max-heap) to solve problems.

### When to use:
- Problems involving finding medians in streams
- Problems involving finding a balance point

### Identification:
- Finding median
- Finding a balance point
- Finding schedules

### Examples:
1. **Find the Median of a Number Stream**
2. **Sliding Window Median**
3. **Maximize Capital**
4. **Find Right Interval**

### Implementation Template:

```java
class TwoHeapsTemplate {
    PriorityQueue<Integer> maxHeap; // For smaller half of numbers
    PriorityQueue<Integer> minHeap; // For larger half of numbers
    
    public TwoHeapsTemplate() {
        maxHeap = new PriorityQueue<>((a, b) -> b - a); // Max heap
        minHeap = new PriorityQueue<>(); // Min heap
    }
    
    public void add(int num) {
        // Logic to add a number and maintain balance
        if (maxHeap.isEmpty() || maxHeap.peek() >= num) {
            maxHeap.add(num);
        } else {
            minHeap.add(num);
        }
        
        // Balance the heaps
        if (maxHeap.size() > minHeap.size() + 1) {
            minHeap.add(maxHeap.poll());
        } else if (maxHeap.size() < minHeap.size()) {
            maxHeap.add(minHeap.poll());
        }
    }
    
    public double findMedian() {
        if (maxHeap.size() == minHeap.size()) {
            return (maxHeap.peek() + minHeap.peek()) / 2.0;
        }
        return maxHeap.peek();
    }
}
```

### Example Problem (Find the Median of a Number Stream):

```java
class MedianFinder {
    PriorityQueue<Integer> maxHeap; // For smaller half
    PriorityQueue<Integer> minHeap; // For larger half

    public MedianFinder() {
        maxHeap = new PriorityQueue<>((a, b) -> b - a); // Max heap
        minHeap = new PriorityQueue<>(); // Min heap
    }
    
    public void addNum(int num) {
        if (maxHeap.isEmpty() || maxHeap.peek() >= num) {
            maxHeap.add(num);
        } else {
            minHeap.add(num);
        }
        
        // Balance the heaps
        if (maxHeap.size() > minHeap.size() + 1) {
            minHeap.add(maxHeap.poll());
        } else if (maxHeap.size() < minHeap.size()) {
            maxHeap.add(minHeap.poll());
        }
    }
    
    public double findMedian() {
        if (maxHeap.size() == minHeap.size()) {
            return (maxHeap.peek() + minHeap.peek()) / 2.0;
        }
        return maxHeap.peek();
    }
}
```

## 10. Subsets Pattern

The Subsets pattern generates all possible subsets, permutations, or combinations.

### When to use:
- Problems involving generating permutations, combinations, or subsets
- Problems involving exploring all possibilities

### Identification:
- Finding permutations
- Finding subsets
- Finding combinations
- Power sets

### Examples:
1. **Subsets**
2. **Permutations**
3. **Combinations**
4. **Letter Combinations of a Phone Number**
5. **Generate Parentheses**

### Implementation Template:

```java
public List<List<Integer>> subsetsTemplate(int[] nums, ...) {
    List<List<Integer>> result = new ArrayList<>();
    result.add(new ArrayList<>()); // Start with empty subset
    
    for (int num : nums) {
        int n = result.size();
        
        for (int i = 0; i < n; i++) {
            List<Integer> set = new ArrayList<>(result.get(i));
            set.add(num);
            result.add(set);
        }
    }
    
    return result;
}
```

### Example Problem (Subsets):

```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    result.add(new ArrayList<>()); // Start with empty subset
    
    for (int num : nums) {
        int n = result.size();
        
        for (int i = 0; i < n; i++) {
            List<Integer> set = new ArrayList<>(result.get(i));
            set.add(num);
            result.add(set);
        }
    }
    
    return result;
}
```

## 11. Modified Binary Search Pattern

The Modified Binary Search pattern applies binary search with modifications.

### When to use:
- Problems involving searching in a sorted array, list, or matrix
- Finding a specific element or condition

### Identification:
- Sorted arrays
- Finding a specific element
- Finding a rotation point
- Finding a peak element

### Examples:
1. **Binary Search**
2. **Find Smallest Letter Greater Than Target**
3. **Find First and Last Position of Element in Sorted Array**
4. **Search in Rotated Sorted Array**
5. **Search in a Sorted Infinite Array**

### Implementation Template:

```java
public int modifiedBinarySearchTemplate(int[] arr, int key, ...) {
    int left = 0;
    int right = arr.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (/* condition */) {
            return mid; // Or another value based on the problem
        }
        
        if (/* condition to search left */) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    
    return -1; // Or another value based on the problem
}
```

### Example Problem (Search in Rotated Sorted Array):

```java
public int search(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            return mid;
        }
        
        // Check if left half is sorted
        if (nums[left] <= nums[mid]) {
            // Check if target is in left half
            if (nums[left] <= target && target < nums[mid]) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        } else { // Right half is sorted
            // Check if target is in right half
            if (nums[mid] < target && target <= nums[right]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
    }
    
    return -1;
}
```

## 12. Topological Sort Pattern

The Topological Sort pattern is used for sorting directed graphs.

### When to use:
- Problems involving dependencies
- Problems involving directed acyclic graphs (DAGs)
- Finding an order of processing tasks

### Identification:
- Dependencies
- Directed graphs
- Course prerequisites
- Task scheduling

### Examples:
1. **Course Schedule**
2. **Course Schedule II**
3. **Alien Dictionary**
4. **Task Scheduling**
5. **Minimum Height Trees**

### Implementation Template:

```java
public int[] topologicalSortTemplate(int vertices, int[][] edges, ...) {
    // Build the graph
    Map<Integer, List<Integer>> graph = new HashMap<>();
    int[] inDegree = new int[vertices];
    
    for (int i = 0; i < vertices; i++) {
        graph.put(i, new ArrayList<>());
    }
    
    for (int[] edge : edges) {
        int parent = edge[0], child = edge[1];
        graph.get(parent).add(child);
        inDegree[child]++;
    }
    
    // Find sources (vertices with 0 in-degree)
    Queue<Integer> sources = new LinkedList<>();
    for (int i = 0; i < vertices; i++) {
        if (inDegree[i] == 0) {
            sources.add(i);
        }
    }
    
    // Sort the vertices
    List<Integer> sortedOrder = new ArrayList<>();
    while (!sources.isEmpty()) {
        int vertex = sources.poll();
        sortedOrder.add(vertex);
        
        // Reduce in-degree of children and add to sources if in-degree becomes 0
        for (int child : graph.get(vertex)) {
            inDegree[child]--;
            if (inDegree[child] == 0) {
                sources.add(child);
            }
        }
    }
    
    // Check for cycles
    if (sortedOrder.size() != vertices) {
        return new int[0]; // Cycle detected
    }
    
    // Convert to array
    int[] result = new int[vertices];
    for (int i = 0; i < vertices; i++) {
        result[i] = sortedOrder.get(i);
    }
    
    return result;
}
```

### Example Problem (Course Schedule II):

```java
public int[] findOrder(int numCourses, int[][] prerequisites) {
    // Build the graph
    Map<Integer, List<Integer>> graph = new HashMap<>();
    int[] inDegree = new int[numCourses];
    
    for (int i = 0; i < numCourses; i++) {
        graph.put(i, new ArrayList<>());
    }
    
    for (int[] prereq : prerequisites) {
        int course = prereq[0], prerequisite = prereq[1];
        graph.get(prerequisite).add(course);
        inDegree[course]++;
    }
    
    // Find courses with no prerequisites
    Queue<Integer> sources = new LinkedList<>();
    for (int i = 0; i < numCourses; i++) {
        if (inDegree[i] == 0) {
            sources.add(i);
        }
    }
    
    // Process courses in topological order
    List<Integer> sortedOrder = new ArrayList<>();
    while (!sources.isEmpty()) {
        int course = sources.poll();
        sortedOrder.add(course);
        
        // After taking the course, remove it as a prerequisite
        for (int nextCourse : graph.get(course)) {
            inDegree[nextCourse]--;
            if (inDegree[nextCourse] == 0) {
                sources.add(nextCourse);
            }
        }
    }
    
    // Check if all courses can be taken
    if (sortedOrder.size() != numCourses) {
        return new int[0]; // Cycle detected, impossible to finish all courses
    }
    
    // Convert to array
    int[] result = new int[numCourses];
    for (int i = 0; i < numCourses; i++) {
        result[i] = sortedOrder.get(i);
    }
    
    return result;
}
```

## 13. Dynamic Programming Patterns

Dynamic Programming (DP) patterns solve complex problems by breaking them down into simpler subproblems.

### Key DP Patterns:

#### 1. 0/1 Knapsack Pattern

Used when you need to make decisions to either include an item or not.

**Examples**:
- 0/1 Knapsack
- Subset Sum
- Equal Subset Sum Partition
- Minimum Subset Sum Difference

**Template**:
```java
public int knapsack(int[] weights, int[] values, int capacity) {
    int n = weights.length;
    int[][] dp = new int[n + 1][capacity + 1];
    
    for (int i = 1; i <= n; i++) {
        for (int w = 1; w <= capacity; w++) {
            if (weights[i - 1] <= w) {
                // Include or exclude the item
                dp[i][w] = Math.max(values[i - 1] + dp[i - 1][w - weights[i - 1]], 
                                    dp[i - 1][w]);
            } else {
                // Cannot include the item
                dp[i][w] = dp[i - 1][w];
            }
        }
    }
    
    return dp[n][capacity];
}
```

#### 2. Unbounded Knapsack Pattern

Used when you can use items multiple times.

**Examples**:
- Unbounded Knapsack
- Rod Cutting
- Coin Change
- Minimum Coin Change

**Template**:
```java
public int unboundedKnapsack(int[] weights, int[] values, int capacity) {
    int n = weights.length;
    int[] dp = new int[capacity + 1];
    
    for (int w = 1; w <= capacity; w++) {
        for (int i = 0; i < n; i++) {
            if (weights[i] <= w) {
                dp[w] = Math.max(dp[w], dp[w - weights[i]] + values[i]);
            }
        }
    }
    
    return dp[capacity];
}
```

#### 3. Fibonacci Pattern

Used for problems with overlapping subproblems that follow a recursive pattern.

**Examples**:
- Fibonacci numbers
- Staircase
- House Thief
- Jump Game

**Template**:
```java
public int fibonacciDP(int n) {
    if (n <= 1) {
        return n;
    }
    
    int[] dp = new int[n + 1];
    dp[0] = 0;
    dp[1] = 1;
    
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    
    return dp[n];
}
```

#### 4. Longest Common Subsequence (LCS) Pattern

Used for problems involving finding common parts between sequences.

**Examples**:
- Longest Common Subsequence
- Longest Common Substring
- Edit Distance
- Minimum Deletions to Make a Sequence Sorted

**Template**:
```java
public int longestCommonSubsequence(String text1, String text2) {
    int m = text1.length();
    int n = text2.length();
    int[][] dp = new int[m + 1][n + 1];
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                dp[i][j] = 1 + dp[i - 1][j - 1];
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    
    return dp[m][n];
}
```

## 14. Bit Manipulation Pattern

Bit manipulation patterns use bitwise operations to solve problems efficiently.

### When to use:
- Problems involving binary representation
- Problems where bitwise operations can optimize the solution
- Problems with constraints like finding non-duplicate number

### Identification:
- XOR operations
- Single number in duplicates
- Power of two
- Bit counting

### Examples:
1. **Single Number**
2. **Number of 1 Bits**
3. **Counting Bits**
4. **Missing Number**
5. **Power of Two**

### Implementation Template:

```java
public int bitManipulationTemplate(int[] nums, ...) {
    int result = 0; // Or another initial value
    
    for (int num : nums) {
        // Use bitwise operations (AND, OR, XOR, shift)
        result ^= num; // Example: XOR for finding single number
    }
    
    return result;
}
```

### Example Problem (Single Number):

```java
public int singleNumber(int[] nums) {
    int result = 0;
    
    for (int num : nums) {
        result ^= num;
    }
    
    return result;
}
```

## 15. Recursion and Backtracking Pattern

Recursion and backtracking patterns solve problems by exploring all possible solutions.

### When to use:
- Problems involving exploring all possible configurations
- Problems where you need to find all possible solutions

### Identification:
- Permutations
- Combinations
- N-Queens
- Sudoku Solver

### Implementation Template:

```java
public void backtrackingTemplate(int[] nums, ...) {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> current = new ArrayList<>();
    
    backtrack(nums, result, current, ...);
    
    // Return result or process it further
}

private void backtrack(int[] nums, List<List<Integer>> result, List<Integer> current, ...) {
    // Base case: if the current state is a solution
    if (/* condition */) {
        result.add(new ArrayList<>(current));
        return;
    }
    
    // Try all possible choices
    for (int i = 0; i < nums.length; i++) {
        // Skip invalid choices
        if (/* invalid choice */) {
            continue;
        }
        
        // Make a choice
        current.add(nums[i]);
        
        // Recurse with the choice
        backtrack(nums, result, current, ...);
        
        // Undo the choice (backtrack)
        current.remove(current.size() - 1);
    }
}
```

### Example Problem (Permutations):

```java
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(nums, result, new ArrayList<>(), new boolean[nums.length]);
    return result;
}

private void backtrack(int[] nums, List<List<Integer>> result, List<Integer> current, boolean[] used) {
    // If current permutation is complete
    if (current.size() == nums.length) {
        result.add(new ArrayList<>(current));
        return;
    }
    
    for (int i = 0; i < nums.length; i++) {
        // Skip used elements
        if (used[i]) {
            continue;
        }
        
        // Make choice
        used[i] = true;
        current.add(nums[i]);
        
        // Recurse
        backtrack(nums, result, current, used);
        
        // Backtrack
        used[i] = false;
        current.remove(current.size() - 1);
    }
}
```

## Conclusion

Understanding these common patterns is crucial for solving coding interview problems efficiently. When faced with a new problem, try to identify which pattern it most closely matches and apply the corresponding template as a starting point.

Remember that most interview problems are variations or combinations of these patterns. With practice, you'll be able to quickly recognize the underlying patterns and apply the appropriate techniques.

## Next Steps

- Study each pattern in depth
- Practice identifying patterns in new problems
- Solve example problems for each pattern
- Create your own template implementations

Continue to [Problem-Solving Strategies](/coding-challenges/strategies.md) to learn more about approaching coding problems effectively.

---

This guide will be regularly updated with new patterns and examples. Happy coding!