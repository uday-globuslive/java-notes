# Dynamic Programming in Java

Dynamic Programming (DP) is a powerful technique used to solve complex problems by breaking them down into simpler overlapping subproblems and storing the results to avoid redundant calculations. This guide covers the fundamental concepts of dynamic programming and explores various types of DP problems with their Java implementations.

## Table of Contents
- [Introduction to Dynamic Programming](#introduction-to-dynamic-programming)
- [Key Components of Dynamic Programming](#key-components-of-dynamic-programming)
- [Approaches to Dynamic Programming](#approaches-to-dynamic-programming)
- [Classic Dynamic Programming Problems](#classic-dynamic-programming-problems)
- [Advanced Dynamic Programming Techniques](#advanced-dynamic-programming-techniques)
- [Common Interview Problems](#common-interview-problems)
- [Practice Problems](#practice-problems)

## Introduction to Dynamic Programming

Dynamic Programming is both a mathematical optimization method and a computer programming method. It was developed by Richard Bellman in the 1950s. The essence of dynamic programming is:

1. Break down a complex problem into simpler subproblems
2. Solve each subproblem only once and store the solutions
3. Reuse the stored solutions to avoid redundant calculations
4. Combine the solutions of subproblems to solve the original problem

### When to Use Dynamic Programming

Dynamic Programming is applicable when a problem has:
1. **Optimal Substructure**: The optimal solution to the original problem can be constructed from optimal solutions of its subproblems.
2. **Overlapping Subproblems**: The same subproblems are solved multiple times.

## Key Components of Dynamic Programming

### 1. State

The state represents the subproblem we're solving. It defines the parameters that uniquely identify a specific subproblem.

### 2. Recurrence Relation

The recurrence relation defines how the solution to a problem relates to the solutions of its subproblems.

### 3. Base Cases

Base cases are the simplest instances of the problem that can be solved directly without further recursion.

### 4. State Storage (Memoization/Tabulation)

The storage mechanism (array, HashMap, etc.) that holds the solutions to subproblems to avoid redundant calculations.

## Approaches to Dynamic Programming

### 1. Top-Down Approach (Memoization)

Start with the original problem and recursively break it down into subproblems. Use memoization to store results of subproblems.

```java
// Fibonacci using memoization
public static int fibonacci(int n) {
    int[] memo = new int[n + 1];
    Arrays.fill(memo, -1);
    return fibMemo(n, memo);
}

private static int fibMemo(int n, int[] memo) {
    if (n <= 1) return n;
    
    if (memo[n] != -1) return memo[n];
    
    memo[n] = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
    return memo[n];
}
```

### 2. Bottom-Up Approach (Tabulation)

Start with the base cases and iteratively build up the solution to the original problem.

```java
// Fibonacci using tabulation
public static int fibonacci(int n) {
    if (n <= 1) return n;
    
    int[] dp = new int[n + 1];
    dp[0] = 0;
    dp[1] = 1;
    
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    
    return dp[n];
}
```

### Comparing the Two Approaches

| Aspect | Top-Down (Memoization) | Bottom-Up (Tabulation) |
|--------|------------------------|------------------------|
| Implementation | Usually recursive | Usually iterative |
| Complexity | Space for recursion stack | Typically more efficient |
| Ease of understanding | Often more intuitive | May be harder to formulate |
| Subproblem solving | Solves only necessary subproblems | Solves all subproblems |

## Classic Dynamic Programming Problems

### 1. Fibonacci Numbers

**Problem**: Find the nth Fibonacci number.

**Recurrence Relation**: F(n) = F(n-1) + F(n-2)

**Base Cases**: F(0) = 0, F(1) = 1

**Tabulation Implementation**:
```java
public static int fibonacci(int n) {
    if (n <= 1) return n;
    
    int[] dp = new int[n + 1];
    dp[0] = 0;
    dp[1] = 1;
    
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    
    return dp[n];
}
```

**Space-Optimized Implementation**:
```java
public static int fibonacci(int n) {
    if (n <= 1) return n;
    
    int prev = 0, curr = 1;
    for (int i = 2; i <= n; i++) {
        int next = prev + curr;
        prev = curr;
        curr = next;
    }
    
    return curr;
}
```

### 2. Longest Increasing Subsequence (LIS)

**Problem**: Find the length of the longest subsequence such that all elements of the subsequence are sorted in increasing order.

**State**: dp[i] = length of LIS ending at index i

**Recurrence Relation**: dp[i] = 1 + max(dp[j]) for all j < i where nums[j] < nums[i]

**Base Case**: dp[i] = 1 for all i (single element is always a valid subsequence)

```java
public static int lengthOfLIS(int[] nums) {
    if (nums.length == 0) return 0;
    
    int[] dp = new int[nums.length];
    Arrays.fill(dp, 1);
    int maxLength = 1;
    
    for (int i = 1; i < nums.length; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
        maxLength = Math.max(maxLength, dp[i]);
    }
    
    return maxLength;
}
```

### 3. Knapsack Problem

**Problem**: Given weights and values of n items, put these items in a knapsack of capacity W to get the maximum total value.

**State**: dp[i][w] = maximum value that can be obtained using first i items and with weight limit w

**Recurrence Relation**:
- If weight of item i > w: dp[i][w] = dp[i-1][w] (skip item i)
- Otherwise: dp[i][w] = max(dp[i-1][w], dp[i-1][w-weight[i]] + value[i]) (take or skip item i)

**Base Case**: dp[0][w] = 0 for all w, dp[i][0] = 0 for all i

```java
public static int knapsack(int[] weights, int[] values, int capacity) {
    int n = weights.length;
    int[][] dp = new int[n + 1][capacity + 1];
    
    for (int i = 1; i <= n; i++) {
        for (int w = 0; w <= capacity; w++) {
            if (weights[i - 1] <= w) {
                // Include or exclude current item
                dp[i][w] = Math.max(values[i - 1] + dp[i - 1][w - weights[i - 1]], dp[i - 1][w]);
            } else {
                // Can't include current item
                dp[i][w] = dp[i - 1][w];
            }
        }
    }
    
    return dp[n][capacity];
}
```

### 4. Coin Change Problem

**Problem**: Given a set of coin denominations and a target amount, find the minimum number of coins needed to make up that amount.

**State**: dp[i] = minimum number of coins needed to make amount i

**Recurrence Relation**: dp[i] = min(dp[i], dp[i - coin] + 1) for each coin denomination

**Base Case**: dp[0] = 0, dp[i] = ∞ for i > 0

```java
public static int coinChange(int[] coins, int amount) {
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, amount + 1); // Fill with a value larger than any possible answer
    dp[0] = 0;
    
    for (int coin : coins) {
        for (int i = coin; i <= amount; i++) {
            dp[i] = Math.min(dp[i], dp[i - coin] + 1);
        }
    }
    
    return dp[amount] > amount ? -1 : dp[amount];
}
```

### 5. Longest Common Subsequence (LCS)

**Problem**: Find the length of the longest subsequence common to two sequences.

**State**: dp[i][j] = length of LCS of substrings text1[0...i-1] and text2[0...j-1]

**Recurrence Relation**:
- If text1[i-1] == text2[j-1]: dp[i][j] = dp[i-1][j-1] + 1
- Otherwise: dp[i][j] = max(dp[i-1][j], dp[i][j-1])

**Base Case**: dp[0][j] = 0 for all j, dp[i][0] = 0 for all i

```java
public static int longestCommonSubsequence(String text1, String text2) {
    int m = text1.length();
    int n = text2.length();
    int[][] dp = new int[m + 1][n + 1];
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    
    return dp[m][n];
}
```

## Advanced Dynamic Programming Techniques

### 1. State Compression

In some DP problems, the state space can be large but can be compressed using bit manipulation or other techniques.

**Example: Traveling Salesman Problem using Bitmasks**

```java
public static int tsp(int[][] graph) {
    int n = graph.length;
    int allVisited = (1 << n) - 1;
    
    // dp[i][mask] = minimum distance to visit all cities in mask and end at city i
    int[][] dp = new int[n][1 << n];
    for (int[] row : dp) {
        Arrays.fill(row, Integer.MAX_VALUE / 2); // Avoid overflow
    }
    
    // Base case: start at city 0
    dp[0][1] = 0; // 1 is the bit mask for visiting only city 0
    
    for (int mask = 1; mask < (1 << n); mask++) {
        for (int end = 0; end < n; end++) {
            // Skip if current city is not in the mask
            if ((mask & (1 << end)) == 0) continue;
            
            // Previous mask without the end city
            int prevMask = mask ^ (1 << end);
            
            // Skip if prevMask is empty (except when end is the starting city)
            if (prevMask == 0 && end != 0) continue;
            
            for (int prev = 0; prev < n; prev++) {
                if ((prevMask & (1 << prev)) == 0) continue;
                
                dp[end][mask] = Math.min(dp[end][mask], dp[prev][prevMask] + graph[prev][end]);
            }
        }
    }
    
    // Return shortest path that visits all cities and returns to starting city
    int result = Integer.MAX_VALUE;
    for (int end = 1; end < n; end++) {
        result = Math.min(result, dp[end][allVisited] + graph[end][0]);
    }
    
    return result;
}
```

### 2. DP on Trees

Dynamic programming can be applied to tree data structures, typically using post-order traversal.

**Example: Maximum Path Sum in a Binary Tree**

```java
public static int maxPathSum(TreeNode root) {
    int[] maxSum = {Integer.MIN_VALUE};
    maxPathSumHelper(root, maxSum);
    return maxSum[0];
}

private static int maxPathSumHelper(TreeNode node, int[] maxSum) {
    if (node == null) return 0;
    
    // Get max path sum from left and right subtrees
    int leftSum = Math.max(0, maxPathSumHelper(node.left, maxSum));
    int rightSum = Math.max(0, maxPathSumHelper(node.right, maxSum));
    
    // Update maxSum with the path through current node
    maxSum[0] = Math.max(maxSum[0], node.val + leftSum + rightSum);
    
    // Return the max path sum extending to parent
    return node.val + Math.max(leftSum, rightSum);
}
```

### 3. DP with State Transition Optimization

Some DP problems can be optimized by changing the order of computation or using more advanced data structures.

**Example: Longest Increasing Subsequence (Optimized)**

```java
public static int lengthOfLIS(int[] nums) {
    int[] tails = new int[nums.length];
    int size = 0;
    
    for (int x : nums) {
        int i = 0, j = size;
        while (i != j) {
            int m = (i + j) / 2;
            if (tails[m] < x) {
                i = m + 1;
            } else {
                j = m;
            }
        }
        tails[i] = x;
        if (i == size) size++;
    }
    
    return size;
}
```

## Common Interview Problems

### 1. Edit Distance

**Problem**: Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2. Operations include insert, delete, and replace.

```java
public static int minDistance(String word1, String word2) {
    int m = word1.length();
    int n = word2.length();
    int[][] dp = new int[m + 1][n + 1];
    
    // Base cases
    for (int i = 0; i <= m; i++) {
        dp[i][0] = i;
    }
    for (int j = 0; j <= n; j++) {
        dp[0][j] = j;
    }
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = 1 + Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1]));
            }
        }
    }
    
    return dp[m][n];
}
```

### 2. Maximum Subarray Sum

**Problem**: Find the contiguous subarray within an array that has the largest sum.

```java
public static int maxSubArray(int[] nums) {
    int n = nums.length;
    int maxSoFar = nums[0], maxEndingHere = nums[0];
    
    for (int i = 1; i < n; i++) {
        maxEndingHere = Math.max(nums[i], maxEndingHere + nums[i]);
        maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
    
    return maxSoFar;
}
```

### 3. Climbing Stairs

**Problem**: You are climbing a staircase. It takes n steps to reach the top. Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

```java
public static int climbStairs(int n) {
    if (n <= 2) return n;
    
    int[] dp = new int[n + 1];
    dp[1] = 1;
    dp[2] = 2;
    
    for (int i = 3; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    
    return dp[n];
}
```

### 4. House Robber

**Problem**: You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. You cannot rob adjacent houses. Find the maximum amount of money you can rob.

```java
public static int rob(int[] nums) {
    if (nums.length == 0) return 0;
    if (nums.length == 1) return nums[0];
    
    int n = nums.length;
    int[] dp = new int[n];
    dp[0] = nums[0];
    dp[1] = Math.max(nums[0], nums[1]);
    
    for (int i = 2; i < n; i++) {
        dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);
    }
    
    return dp[n - 1];
}
```

### 5. Palindrome Partitioning

**Problem**: Given a string s, partition s such that every substring of the partition is a palindrome. Return the minimum cuts needed.

```java
public static int minCut(String s) {
    int n = s.length();
    boolean[][] isPalindrome = new boolean[n][n];
    int[] cuts = new int[n];
    
    // Precompute palindrome information
    for (int j = 0; j < n; j++) {
        cuts[j] = j; // Maximum cuts needed
        for (int i = 0; i <= j; i++) {
            if (s.charAt(i) == s.charAt(j) && (j - i <= 1 || isPalindrome[i + 1][j - 1])) {
                isPalindrome[i][j] = true;
                cuts[j] = (i == 0) ? 0 : Math.min(cuts[j], cuts[i - 1] + 1);
            }
        }
    }
    
    return cuts[n - 1];
}
```

## Practice Problems

1. **Word Break**

```java
public static boolean wordBreak(String s, List<String> wordDict) {
    Set<String> wordSet = new HashSet<>(wordDict);
    boolean[] dp = new boolean[s.length() + 1];
    dp[0] = true;
    
    for (int i = 1; i <= s.length(); i++) {
        for (int j = 0; j < i; j++) {
            if (dp[j] && wordSet.contains(s.substring(j, i))) {
                dp[i] = true;
                break;
            }
        }
    }
    
    return dp[s.length()];
}
```

2. **Target Sum**

```java
public static int findTargetSumWays(int[] nums, int target) {
    int sum = 0;
    for (int num : nums) {
        sum += num;
    }
    
    // Not possible if target exceeds sum or if (sum + target) is odd
    if (Math.abs(target) > sum || (sum + target) % 2 != 0) return 0;
    
    int newTarget = (sum + target) / 2;
    int[] dp = new int[newTarget + 1];
    dp[0] = 1;
    
    for (int num : nums) {
        for (int i = newTarget; i >= num; i--) {
            dp[i] += dp[i - num];
        }
    }
    
    return dp[newTarget];
}
```

3. **Unique Paths in a Grid**

```java
public static int uniquePaths(int m, int n) {
    int[][] dp = new int[m][n];
    
    // Initialize first row and column to 1
    for (int i = 0; i < m; i++) {
        dp[i][0] = 1;
    }
    for (int j = 0; j < n; j++) {
        dp[0][j] = 1;
    }
    
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        }
    }
    
    return dp[m - 1][n - 1];
}
```

4. **Longest Palindromic Subsequence**

```java
public static int longestPalindromeSubseq(String s) {
    int n = s.length();
    int[][] dp = new int[n][n];
    
    // All single characters are palindromes
    for (int i = 0; i < n; i++) {
        dp[i][i] = 1;
    }
    
    // Fill the dp table
    for (int len = 2; len <= n; len++) {
        for (int i = 0; i <= n - len; i++) {
            int j = i + len - 1;
            if (s.charAt(i) == s.charAt(j)) {
                dp[i][j] = dp[i + 1][j - 1] + 2;
            } else {
                dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
    }
    
    return dp[0][n - 1];
}
```

5. **Best Time to Buy and Sell Stock with Cooldown**

```java
public static int maxProfit(int[] prices) {
    if (prices.length <= 1) return 0;
    
    int n = prices.length;
    int[] buy = new int[n]; // Maximum profit if we have a stock on day i
    int[] sell = new int[n]; // Maximum profit if we don't have a stock on day i
    
    // Base cases
    buy[0] = -prices[0];
    sell[0] = 0;
    
    for (int i = 1; i < n; i++) {
        // We can either maintain previous state or buy stock today
        buy[i] = Math.max(buy[i - 1], (i >= 2 ? sell[i - 2] : 0) - prices[i]);
        
        // We can either maintain previous state or sell stock today
        sell[i] = Math.max(sell[i - 1], buy[i - 1] + prices[i]);
    }
    
    return sell[n - 1];
}
```

## Tips for Solving DP Problems

1. **Identify if the problem is a DP problem**:
   - Does it ask for an optimal value (maximum, minimum)?
   - Can the problem be broken down into simpler subproblems?
   - Are there overlapping subproblems?

2. **Define the state**:
   - What information do we need to solve a subproblem?
   - How do we represent this information as state variables?

3. **Establish the recurrence relation**:
   - How does the solution to a state relate to solutions of other states?

4. **Identify base cases**:
   - What are the simplest instances of the problem?
   - What are their solutions?

5. **Decide the approach**:
   - Use memoization (top-down) if recursive solution is more intuitive
   - Use tabulation (bottom-up) for better performance and space optimization

6. **Optimize space complexity** if needed:
   - Can we reduce from 2D to 1D array?
   - Do we need to store all states or just a few?

7. **Implement and test with simple examples** to verify the solution.

## Conclusion

Dynamic Programming is a powerful technique that can turn exponential time algorithms into polynomial time ones. By understanding the core principles and practicing various types of problems, you can become proficient in recognizing and solving DP problems efficiently.

Remember that the key to mastering DP is practice. Start with simpler problems like Fibonacci and Knapsack, then gradually tackle more complex ones like edit distance and state compression problems.