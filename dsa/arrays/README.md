# Arrays and Strings in Data Structures & Algorithms

Arrays are one of the most fundamental data structures and are frequently used in coding interviews. This section covers array operations, manipulation techniques, and common problem-solving patterns.

## Basic Array Operations

### Time Complexity

| Operation | Time Complexity |
|-----------|----------------|
| Access | O(1) |
| Search (unsorted) | O(n) |
| Search (sorted) | O(log n) - using binary search |
| Insertion (at the end) | O(1) - amortized for ArrayList |
| Insertion (at a position) | O(n) |
| Deletion (at the end) | O(1) |
| Deletion (at a position) | O(n) |

### Space Complexity

Arrays have a space complexity of O(n), where n is the number of elements in the array.

## Common Array Techniques

### 1. Two Pointers Technique

The two pointers technique uses two pointers to solve problems efficiently. It's commonly used for:
- Finding pairs in a sorted array
- Reversing arrays
- Detecting cycles
- Finding subarrays with specific properties

#### Example: Two Sum in a Sorted Array

```java
public int[] twoSum(int[] numbers, int target) {
    int left = 0;
    int right = numbers.length - 1;
    
    while (left < right) {
        int sum = numbers[left] + numbers[right];
        
        if (sum == target) {
            return new int[] {left + 1, right + 1}; // 1-indexed array
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    
    return new int[] {-1, -1}; // No solution found
}
```

### 2. Sliding Window Technique

The sliding window technique is used to process arrays or lists by maintaining a "window" of elements and sliding it through the data.

#### Example: Find Maximum Sum Subarray of Size K

```java
public int maxSubArraySum(int[] arr, int k) {
    if (arr.length < k) {
        return -1; // Invalid input
    }
    
    // Compute sum of first window of size k
    int maxSum = 0;
    for (int i = 0; i < k; i++) {
        maxSum += arr[i];
    }
    
    // Compute sums of remaining windows by removing first element
    // of previous window and adding last element of current window
    int windowSum = maxSum;
    for (int i = k; i < arr.length; i++) {
        windowSum = windowSum - arr[i - k] + arr[i];
        maxSum = Math.max(maxSum, windowSum);
    }
    
    return maxSum;
}
```

### 3. Prefix Sum

Prefix sum is a technique where you pre-compute the cumulative sum of elements up to each index. This allows for efficient range sum queries.

#### Example: Range Sum Query

```java
class NumArray {
    private int[] prefixSum;
    
    public NumArray(int[] nums) {
        prefixSum = new int[nums.length + 1];
        for (int i = 0; i < nums.length; i++) {
            prefixSum[i + 1] = prefixSum[i] + nums[i];
        }
    }
    
    public int sumRange(int left, int right) {
        return prefixSum[right + 1] - prefixSum[left];
    }
}
```

### 4. Binary Search

Binary search is an efficient algorithm for finding a target value in a sorted array.

#### Example: Basic Binary Search

```java
public int binarySearch(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2; // Avoids overflow
        
        if (nums[mid] == target) {
            return mid; // Target found
        } else if (nums[mid] < target) {
            left = mid + 1; // Search the right half
        } else {
            right = mid - 1; // Search the left half
        }
    }
    
    return -1; // Target not found
}
```

## Common Array Problems

### 1. Array Traversal and Manipulation

#### Example: Rotate Array to the Right by K Steps

```java
public void rotate(int[] nums, int k) {
    int n = nums.length;
    k = k % n; // Handle cases where k > n
    
    // Reverse the entire array
    reverse(nums, 0, n - 1);
    // Reverse first k elements
    reverse(nums, 0, k - 1);
    // Reverse last n-k elements
    reverse(nums, k, n - 1);
}

private void reverse(int[] nums, int start, int end) {
    while (start < end) {
        int temp = nums[start];
        nums[start] = nums[end];
        nums[end] = temp;
        start++;
        end--;
    }
}
```

### 2. Finding Elements

#### Example: Find Majority Element (Boyer-Moore Voting Algorithm)

```java
public int majorityElement(int[] nums) {
    int count = 0;
    Integer candidate = null;
    
    for (int num : nums) {
        if (count == 0) {
            candidate = num;
        }
        
        count += (num == candidate) ? 1 : -1;
    }
    
    return candidate;
}
```

### 3. Subarray Problems

#### Example: Maximum Subarray (Kadane's Algorithm)

```java
public int maxSubArray(int[] nums) {
    int maxSoFar = nums[0];
    int maxEndingHere = nums[0];
    
    for (int i = 1; i < nums.length; i++) {
        maxEndingHere = Math.max(nums[i], maxEndingHere + nums[i]);
        maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
    
    return maxSoFar;
}
```

### 4. Matrix Operations

#### Example: Spiral Matrix Traversal

```java
public List<Integer> spiralOrder(int[][] matrix) {
    List<Integer> result = new ArrayList<>();
    
    if (matrix == null || matrix.length == 0) {
        return result;
    }
    
    int rows = matrix.length;
    int cols = matrix[0].length;
    
    int top = 0;
    int bottom = rows - 1;
    int left = 0;
    int right = cols - 1;
    
    while (top <= bottom && left <= right) {
        // Traverse right
        for (int j = left; j <= right; j++) {
            result.add(matrix[top][j]);
        }
        top++;
        
        // Traverse down
        for (int i = top; i <= bottom; i++) {
            result.add(matrix[i][right]);
        }
        right--;
        
        // Traverse left
        if (top <= bottom) {
            for (int j = right; j >= left; j--) {
                result.add(matrix[bottom][j]);
            }
            bottom--;
        }
        
        // Traverse up
        if (left <= right) {
            for (int i = bottom; i >= top; i--) {
                result.add(matrix[i][left]);
            }
            left++;
        }
    }
    
    return result;
}
```

## Strings in Java

Strings are immutable sequences of characters. Many array techniques can be applied to strings as well.

### Common String Operations

#### Example: Check if a String is a Palindrome

```java
public boolean isPalindrome(String s) {
    // Convert to lowercase and remove non-alphanumeric characters
    s = s.toLowerCase().replaceAll("[^a-z0-9]", "");
    
    int left = 0;
    int right = s.length() - 1;
    
    while (left < right) {
        if (s.charAt(left) != s.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }
    
    return true;
}
```

#### Example: Reverse Words in a String

```java
public String reverseWords(String s) {
    // Split the input string by one or more spaces
    String[] words = s.trim().split("\\s+");
    
    // Reverse the array of words
    StringBuilder reversed = new StringBuilder();
    for (int i = words.length - 1; i >= 0; i--) {
        reversed.append(words[i]);
        if (i > 0) {
            reversed.append(" ");
        }
    }
    
    return reversed.toString();
}
```

## Practice Problems

### Array Problems

1. **Two Sum**: Given an array of integers, return indices of the two numbers such that they add up to a specific target.

2. **Three Sum**: Find all unique triplets in the array which gives the sum of zero.

3. **Container With Most Water**: Given n non-negative integers representing the heights of bars, find two bars that together with the x-axis forms a container that holds the most water.

4. **Move Zeroes**: Given an array of integers, move all 0's to the end while maintaining the relative order of non-zero elements.

5. **Merge Sorted Arrays**: Merge two sorted arrays into a single sorted array.

6. **Search in Rotated Sorted Array**: Search for a target value in a rotated sorted array.

7. **Find First and Last Position**: Find the starting and ending position of a given target value in a sorted array.

8. **Product of Array Except Self**: Compute the product of all array elements except the current one without using division.

### String Problems

1. **Valid Anagram**: Determine if two strings are anagrams of each other.

2. **Group Anagrams**: Group strings that are anagrams of each other.

3. **Longest Substring Without Repeating Characters**: Find the length of the longest substring without repeating characters.

4. **Longest Palindromic Substring**: Find the longest palindromic substring in a string.

5. **String to Integer (atoi)**: Implement the `atoi` function to convert a string to an integer.

6. **Implement strStr()**: Return the index of the first occurrence of a substring.

7. **Valid Parentheses**: Determine if a string with parentheses, brackets, and braces is valid.

8. **Regular Expression Matching**: Implement regular expression matching with support for '.' and '*'.

## Next Steps

Move on to [Linked Lists](/dsa/linked-lists/README.md) to learn about another fundamental data structure.