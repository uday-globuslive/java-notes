# Top Coding Interview Questions

This list contains frequently asked coding interview questions organized by data structure and algorithm type. These problems cover the most common patterns you'll encounter in technical interviews.

## Array Problems

### Easy

1. **Two Sum**
   - Given an array of integers and a target, find two numbers that add up to the target.
   - Example: `nums = [2, 7, 11, 15], target = 9` → Output: `[0, 1]` (indices of 2 and 7)
   - Approach: Hash map to store visited numbers

2. **Best Time to Buy and Sell Stock**
   - Find the maximum profit by buying a stock on one day and selling on a future day.
   - Example: `prices = [7, 1, 5, 3, 6, 4]` → Output: `5` (buy at 1, sell at 6)
   - Approach: Keep track of minimum price and maximum profit

3. **Contains Duplicate**
   - Check if an array contains any duplicate elements.
   - Example: `nums = [1, 2, 3, 1]` → Output: `true`
   - Approach: Use a hash set to track seen elements

4. **Majority Element**
   - Find the element that appears more than n/2 times in an array of length n.
   - Example: `nums = [3, 2, 3]` → Output: `3`
   - Approach: Boyer-Moore Voting Algorithm

5. **Missing Number**
   - Find the missing number in an array of n distinct numbers from 0 to n.
   - Example: `nums = [3, 0, 1]` → Output: `2`
   - Approach: Calculate expected sum and subtract actual sum

### Medium

1. **Product of Array Except Self**
   - Return an array where each element is the product of all other elements.
   - Example: `nums = [1, 2, 3, 4]` → Output: `[24, 12, 8, 6]`
   - Approach: Use prefix and suffix products

2. **3Sum**
   - Find all unique triplets that sum to zero.
   - Example: `nums = [-1, 0, 1, 2, -1, -4]` → Output: `[[-1, -1, 2], [-1, 0, 1]]`
   - Approach: Sort array and use two pointers

3. **Container With Most Water**
   - Find the maximum area of water a container can store.
   - Example: `height = [1, 8, 6, 2, 5, 4, 8, 3, 7]` → Output: `49`
   - Approach: Two pointers from both ends

4. **Find First and Last Position of Element in Sorted Array**
   - Find the starting and ending position of a target value in a sorted array.
   - Example: `nums = [5, 7, 7, 8, 8, 10], target = 8` → Output: `[3, 4]`
   - Approach: Binary search for left and right bounds

5. **Rotate Array**
   - Rotate the array to the right by k steps.
   - Example: `nums = [1, 2, 3, 4, 5, 6, 7], k = 3` → Output: `[5, 6, 7, 1, 2, 3, 4]`
   - Approach: Reverse entire array, then reverse parts

### Hard

1. **Trapping Rain Water**
   - Calculate how much water can be trapped after raining.
   - Example: `height = [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]` → Output: `6`
   - Approach: Two pointers or dynamic programming

2. **Median of Two Sorted Arrays**
   - Find the median of two sorted arrays.
   - Example: `nums1 = [1, 3], nums2 = [2]` → Output: `2.0`
   - Approach: Binary search on the smaller array

3. **First Missing Positive**
   - Find the smallest missing positive integer.
   - Example: `nums = [1, 2, 0]` → Output: `3`
   - Approach: Place each number in its correct position

## String Problems

### Easy

1. **Valid Anagram**
   - Check if two strings are anagrams of each other.
   - Example: `s = "anagram", t = "nagaram"` → Output: `true`
   - Approach: Count character frequencies

2. **Valid Palindrome**
   - Check if a string is a palindrome after removing non-alphanumeric characters.
   - Example: `s = "A man, a plan, a canal: Panama"` → Output: `true`
   - Approach: Two pointers from both ends

3. **Reverse String**
   - Reverse a string in-place.
   - Example: `s = ["h", "e", "l", "l", "o"]` → Output: `["o", "l", "l", "e", "h"]`
   - Approach: Two pointers from both ends

4. **First Unique Character in a String**
   - Find the first non-repeating character.
   - Example: `s = "leetcode"` → Output: `0` (index of 'l')
   - Approach: Two-pass approach with a hash map

### Medium

1. **Longest Substring Without Repeating Characters**
   - Find the length of the longest substring without repeating characters.
   - Example: `s = "abcabcbb"` → Output: `3` ("abc")
   - Approach: Sliding window with a hash set

2. **String to Integer (atoi)**
   - Convert a string to a 32-bit signed integer.
   - Example: `s = "42"` → Output: `42`
   - Approach: Parse carefully with edge cases

3. **Group Anagrams**
   - Group strings that are anagrams of each other.
   - Example: `strs = ["eat", "tea", "tan", "ate", "nat", "bat"]` → Output: `[["bat"], ["nat", "tan"], ["ate", "eat", "tea"]]`
   - Approach: Hash map with sorted string as key

4. **Longest Palindromic Substring**
   - Find the longest palindromic substring.
   - Example: `s = "babad"` → Output: `"bab"` or `"aba"`
   - Approach: Expand around center

### Hard

1. **Regular Expression Matching**
   - Implement regular expression matching with '.' and '*'.
   - Example: `s = "aa", p = "a*"` → Output: `true`
   - Approach: Dynamic programming

2. **Minimum Window Substring**
   - Find the smallest substring that contains all characters of another string.
   - Example: `s = "ADOBECODEBANC", t = "ABC"` → Output: `"BANC"`
   - Approach: Sliding window with character counting

## Linked List Problems

### Easy

1. **Reverse Linked List**
   - Reverse a singly linked list.
   - Approach: Iterative with three pointers or recursive

2. **Merge Two Sorted Lists**
   - Merge two sorted linked lists into one sorted list.
   - Approach: Compare heads and build result list

3. **Linked List Cycle**
   - Detect if a linked list has a cycle.
   - Approach: Fast and slow pointers (Floyd's cycle-finding algorithm)

4. **Palindrome Linked List**
   - Check if a linked list is a palindrome.
   - Approach: Reverse second half and compare

### Medium

1. **Remove Nth Node From End of List**
   - Remove the nth node from the end of the list.
   - Approach: Two pointers with a gap of n

2. **Add Two Numbers**
   - Add two numbers represented by linked lists.
   - Approach: Node-by-node addition with carry

3. **Intersection of Two Linked Lists**
   - Find the node where two linked lists intersect.
   - Approach: Calculate length difference or two-pointer technique

4. **Sort List**
   - Sort a linked list in O(n log n) time.
   - Approach: Merge sort

### Hard

1. **Merge k Sorted Lists**
   - Merge k sorted linked lists into one sorted list.
   - Approach: Priority queue or divide and conquer

2. **Reverse Nodes in k-Group**
   - Reverse nodes in k-group.
   - Approach: Count nodes and reverse in groups

## Tree Problems

### Easy

1. **Maximum Depth of Binary Tree**
   - Find the maximum depth of a binary tree.
   - Approach: Recursive DFS or iterative BFS

2. **Validate Binary Search Tree**
   - Check if a binary tree is a valid BST.
   - Approach: Recursive with min/max bounds

3. **Symmetric Tree**
   - Check if a binary tree is symmetric around its center.
   - Approach: Recursive check of mirror nodes

4. **Binary Tree Level Order Traversal**
   - Traverse a binary tree level by level.
   - Approach: BFS with a queue

### Medium

1. **Binary Tree Zigzag Level Order Traversal**
   - Traverse a binary tree in zigzag order.
   - Approach: BFS with direction toggle

2. **Construct Binary Tree from Preorder and Inorder Traversal**
   - Build a binary tree from preorder and inorder traversals.
   - Approach: Recursive construction

3. **Lowest Common Ancestor of a Binary Tree**
   - Find the lowest common ancestor of two nodes.
   - Approach: Recursive search

4. **Binary Tree Right Side View**
   - View a binary tree from the right side.
   - Approach: Level order traversal and take rightmost

### Hard

1. **Serialize and Deserialize Binary Tree**
   - Design an algorithm to serialize and deserialize a binary tree.
   - Approach: Preorder traversal with null markers

2. **Binary Tree Maximum Path Sum**
   - Find the maximum path sum in a binary tree.
   - Approach: Recursive with global max tracking

## Graph Problems

### Medium

1. **Number of Islands**
   - Count the number of islands in a 2D grid.
   - Approach: DFS or BFS to mark connected land cells

2. **Course Schedule**
   - Determine if it's possible to finish all courses.
   - Approach: Topological sort or cycle detection

3. **Clone Graph**
   - Deep copy a graph.
   - Approach: BFS/DFS with a hash map

4. **Pacific Atlantic Water Flow**
   - Find cells that can flow to both oceans.
   - Approach: BFS/DFS from oceans inward

### Hard

1. **Word Ladder**
   - Find the shortest transformation sequence.
   - Approach: BFS with word transformation

2. **Alien Dictionary**
   - Determine the order of characters in an alien language.
   - Approach: Topological sort

3. **Graph Valid Tree**
   - Check if a graph is a valid tree.
   - Approach: Check connected and acyclic properties

## Dynamic Programming Problems

### Easy

1. **Climbing Stairs**
   - Count ways to climb to the top, taking 1 or 2 steps.
   - Approach: Bottom-up DP (similar to Fibonacci)

2. **House Robber**
   - Maximum amount of money you can rob without alerting the police.
   - Approach: DP with two states

3. **Maximum Subarray**
   - Find the contiguous subarray with the largest sum.
   - Approach: Kadane's algorithm

### Medium

1. **Unique Paths**
   - Count unique paths from top-left to bottom-right.
   - Approach: 2D DP or combination formula

2. **Coin Change**
   - Fewest number of coins to make up a target amount.
   - Approach: Bottom-up DP

3. **Longest Increasing Subsequence**
   - Find the length of the longest increasing subsequence.
   - Approach: DP or binary search

4. **Word Break**
   - Determine if a string can be segmented into dictionary words.
   - Approach: DP with subproblems by position

### Hard

1. **Edit Distance**
   - Minimum operations to convert one word to another.
   - Approach: 2D DP

2. **Longest Valid Parentheses**
   - Find the length of the longest valid parentheses substring.
   - Approach: DP or stack

3. **Regular Expression Matching**
   - Implement regular expression matching with '.' and '*'.
   - Approach: 2D DP

## Backtracking Problems

### Medium

1. **Permutations**
   - Generate all permutations of an array.
   - Approach: Backtracking with swapping

2. **Subsets**
   - Generate all possible subsets of an array.
   - Approach: Backtracking or bit manipulation

3. **Letter Combinations of a Phone Number**
   - Generate all possible letter combinations from a phone number.
   - Approach: Backtracking with mapping

4. **Word Search**
   - Search for a word in a 2D board.
   - Approach: Backtracking with DFS

### Hard

1. **N-Queens**
   - Place N queens on an N×N chessboard so no queens can attack each other.
   - Approach: Backtracking with valid placement checks

2. **Sudoku Solver**
   - Solve a Sudoku puzzle.
   - Approach: Backtracking with constraints

## Design Problems

### Medium

1. **LRU Cache**
   - Design a data structure that follows the Least Recently Used (LRU) cache policy.
   - Approach: Hash map + doubly linked list

2. **Implement Trie (Prefix Tree)**
   - Implement a trie with insert, search, and startsWith methods.
   - Approach: Trie data structure with nodes

3. **Design Add and Search Words Data Structure**
   - Design a data structure supporting word addition and search.
   - Approach: Trie with wildcard search

### Hard

1. **Design Twitter**
   - Design a simplified version of Twitter.
   - Approach: User, Tweet objects with lists and heaps

2. **LFU Cache**
   - Design a data structure that follows the Least Frequently Used (LFU) cache policy.
   - Approach: Hash maps and frequency lists

## Heap Problems

### Medium

1. **Top K Frequent Elements**
   - Find the k most frequent elements.
   - Approach: Hash map + min-heap

2. **Find K Pairs with Smallest Sums**
   - Find k pairs with the smallest sums.
   - Approach: Min-heap with pairs

3. **Merge k Sorted Lists**
   - Merge k sorted linked lists.
   - Approach: Min-heap with ListNode comparator

### Hard

1. **Find Median from Data Stream**
   - Find the median from a data stream.
   - Approach: Two heaps (max-heap for smaller half, min-heap for larger half)

## Bit Manipulation Problems

### Easy

1. **Single Number**
   - Find the number that appears only once.
   - Approach: XOR all numbers

2. **Number of 1 Bits**
   - Count the number of 1 bits in an integer.
   - Approach: Bit shifting or Brian Kernighan's algorithm

3. **Reverse Bits**
   - Reverse bits of a given integer.
   - Approach: Bit manipulation with shifts

### Medium

1. **Counting Bits**
   - Count the number of 1 bits for each number from 0 to n.
   - Approach: DP based on most significant bit

2. **Sum of Two Integers**
   - Add two integers without using + or - operators.
   - Approach: Bit manipulation with XOR and carry

## Tips for Approaching These Problems

1. **Understand the problem thoroughly**
   - Clarify any ambiguities
   - Identify input/output formats
   - Consider edge cases

2. **Start with a brute force solution**
   - Establish a baseline approach
   - Analyze its time and space complexity

3. **Optimize the solution**
   - Look for patterns or special properties
   - Consider using appropriate data structures
   - Apply relevant algorithms

4. **Test your solution**
   - Trace through with a simple example
   - Check edge cases
   - Verify time and space complexity

5. **Implement cleanly**
   - Write readable code
   - Use meaningful variable names
   - Structure your code logically

## How to Practice Effectively

1. **Start with easy problems**
   - Build confidence and familiarity
   - Master the fundamentals

2. **Move systematically to medium and hard**
   - Gradually increase difficulty
   - Don't rush the learning process

3. **Categorize problems by patterns**
   - Recognize common patterns across problems
   - Apply similar strategies to related problems

4. **Time yourself**
   - Aim to solve easy problems in 15-20 minutes
   - Medium problems in 30-45 minutes
   - Hard problems in 45-60 minutes

5. **Review solutions**
   - Compare your solution with model solutions
   - Learn alternative approaches
   - Understand the trade-offs

6. **Revisit problems**
   - Try solving previously solved problems from scratch
   - Focus on efficiency and clarity
   - Aim to solve more elegantly than before

## Weekly Practice Plan

### Week 1-2: Array and String Fundamentals
- Focus on easy array and string problems
- Master two pointers and hash table techniques
- Implement basic sorting algorithms

### Week 3-4: Linked Lists and Stacks/Queues
- Solve linked list manipulation problems
- Practice stack and queue implementations
- Learn in-place operations

### Week 5-6: Trees and Graphs
- DFS and BFS traversals
- Binary search trees operations
- Basic graph algorithms

### Week 7-8: Dynamic Programming
- Start with 1D DP problems
- Progress to 2D DP problems
- Learn to identify DP problem patterns

### Week 9-10: Advanced Topics
- Backtracking problems
- Advanced data structures (Trie, Segment Tree)
- Bit manipulation techniques

### Week 11-12: Mock Interviews and Review
- Conduct timed mock interviews
- Focus on weak areas
- Review all problem categories