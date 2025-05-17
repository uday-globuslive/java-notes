# Greedy Algorithms in Java

Greedy algorithms are a class of algorithms that make locally optimal choices at each step with the hope of finding a global optimum. They are often used for optimization problems where we need to maximize or minimize certain values. This guide covers the concept of greedy algorithms, common problems, and their implementations in Java.

## Table of Contents
- [Introduction to Greedy Algorithms](#introduction-to-greedy-algorithms)
- [Characteristics of Greedy Algorithms](#characteristics-of-greedy-algorithms)
- [When to Use Greedy Algorithms](#when-to-use-greedy-algorithms)
- [Classic Greedy Algorithm Problems](#classic-greedy-algorithm-problems)
- [Advanced Greedy Problems](#advanced-greedy-problems)
- [Common Interview Questions](#common-interview-questions)
- [Practice Problems](#practice-problems)

## Introduction to Greedy Algorithms

A greedy algorithm builds up a solution piece by piece, always choosing the next piece that offers the most immediate benefit. This means it makes a locally optimal choice at each step with the hope of finding the global optimum.

The key aspects of greedy algorithms are:
1. They never reconsider their choices
2. They are typically easier to implement and more efficient than other techniques
3. They don't always yield optimal solutions

## Characteristics of Greedy Algorithms

### 1. Greedy Choice Property
The globally optimal solution can be obtained by making locally optimal choices. In other words, a greedy choice plus the optimal solution to the remaining subproblem yields an optimal solution to the original problem.

### 2. Optimal Substructure
The optimal solution to the problem contains optimal solutions to its subproblems.

## When to Use Greedy Algorithms

Greedy algorithms are suitable when:
1. The problem exhibits the greedy choice property
2. The problem has optimal substructure
3. A locally optimal choice leads to a globally optimal solution

They are generally more efficient than other techniques like dynamic programming but are applicable to fewer problems.

## Classic Greedy Algorithm Problems

### 1. Activity Selection Problem

**Problem**: Given a set of activities with start and finish times, select the maximum number of activities that can be performed by a single person, assuming that a person can only work on a single activity at a time.

**Greedy Approach**: Sort activities by finish time and select activities that don't overlap.

```java
public static int maxActivities(int[] start, int[] finish) {
    int n = start.length;
    
    // Create a pair of start and finish times
    Activity[] activities = new Activity[n];
    for (int i = 0; i < n; i++) {
        activities[i] = new Activity(start[i], finish[i]);
    }
    
    // Sort activities by finish time
    Arrays.sort(activities, Comparator.comparingInt(a -> a.finish));
    
    int count = 1; // First activity is always selected
    int lastFinish = activities[0].finish;
    
    for (int i = 1; i < n; i++) {
        // If this activity starts after the finish of the last selected activity
        if (activities[i].start >= lastFinish) {
            count++;
            lastFinish = activities[i].finish;
        }
    }
    
    return count;
}

static class Activity {
    int start, finish;
    
    Activity(int start, int finish) {
        this.start = start;
        this.finish = finish;
    }
}
```

### 2. Fractional Knapsack

**Problem**: Given weights and values of n items, put these items in a knapsack of capacity W to get the maximum total value. In the fractional knapsack problem, we can break items for maximizing the total value.

**Greedy Approach**: Sort items by value-to-weight ratio and take items with higher ratio first. If we can't take the whole item, take a fraction of it.

```java
public static double getMaxValue(int[] weights, int[] values, int capacity) {
    int n = weights.length;
    Item[] items = new Item[n];
    
    for (int i = 0; i < n; i++) {
        items[i] = new Item(weights[i], values[i]);
    }
    
    // Sort by value-to-weight ratio in descending order
    Arrays.sort(items, (a, b) -> Double.compare(
        (double) b.value / b.weight, 
        (double) a.value / a.weight
    ));
    
    double totalValue = 0;
    int currentWeight = 0;
    
    for (Item item : items) {
        if (currentWeight + item.weight <= capacity) {
            // Take the whole item
            currentWeight += item.weight;
            totalValue += item.value;
        } else {
            // Take a fraction of the item
            double remainingCapacity = capacity - currentWeight;
            totalValue += item.value * (remainingCapacity / item.weight);
            break;
        }
    }
    
    return totalValue;
}

static class Item {
    int weight, value;
    
    Item(int weight, int value) {
        this.weight = weight;
        this.value = value;
    }
}
```

### 3. Huffman Coding

**Problem**: Huffman coding is a lossless data compression algorithm that assigns variable-length codes to input characters based on their frequencies.

**Greedy Approach**: Build a binary tree using a priority queue, where characters with lower frequencies have longer codes.

```java
public static Map<Character, String> huffmanCoding(String text) {
    if (text == null || text.isEmpty()) {
        return new HashMap<>();
    }
    
    // Count frequency of each character
    Map<Character, Integer> freqMap = new HashMap<>();
    for (char c : text.toCharArray()) {
        freqMap.put(c, freqMap.getOrDefault(c, 0) + 1);
    }
    
    // Create a priority queue with HuffmanNode comparison
    PriorityQueue<HuffmanNode> pq = new PriorityQueue<>(
        Comparator.comparingInt(a -> a.frequency)
    );
    
    // Add all characters to the priority queue
    for (Map.Entry<Character, Integer> entry : freqMap.entrySet()) {
        pq.add(new HuffmanNode(entry.getKey(), entry.getValue(), null, null));
    }
    
    // Build the Huffman tree
    while (pq.size() > 1) {
        HuffmanNode left = pq.poll();
        HuffmanNode right = pq.poll();
        
        // Create new internal node with frequency = sum of child frequencies
        HuffmanNode parent = new HuffmanNode('\0', left.frequency + right.frequency, left, right);
        pq.add(parent);
    }
    
    // Root of the Huffman tree
    HuffmanNode root = pq.poll();
    
    // Generate codes
    Map<Character, String> huffmanCodes = new HashMap<>();
    generateCodes(root, "", huffmanCodes);
    
    return huffmanCodes;
}

private static void generateCodes(HuffmanNode node, String code, Map<Character, String> huffmanCodes) {
    if (node == null) return;
    
    // If this is a leaf node, store the code
    if (node.left == null && node.right == null) {
        huffmanCodes.put(node.data, code);
    }
    
    // Traverse left with code + "0"
    generateCodes(node.left, code + "0", huffmanCodes);
    
    // Traverse right with code + "1"
    generateCodes(node.right, code + "1", huffmanCodes);
}

static class HuffmanNode {
    char data;
    int frequency;
    HuffmanNode left, right;
    
    HuffmanNode(char data, int frequency, HuffmanNode left, HuffmanNode right) {
        this.data = data;
        this.frequency = frequency;
        this.left = left;
        this.right = right;
    }
}
```

### 4. Coin Change (Greedy Approach)

**Problem**: Given a set of coin denominations and a target amount, find the minimum number of coins needed to make up that amount.

**Note**: The greedy approach works only for certain coin systems (like the US coin system), but not for all. For general cases, dynamic programming is required.

```java
public static int coinChangeGreedy(int[] coins, int amount) {
    // Sort coins in descending order
    Arrays.sort(coins);
    reverse(coins);
    
    int count = 0;
    int remaining = amount;
    
    for (int coin : coins) {
        // Use as many of the current coin as possible
        count += remaining / coin;
        remaining %= coin;
        
        if (remaining == 0) break;
    }
    
    return remaining == 0 ? count : -1; // Return -1 if exact change is not possible
}

private static void reverse(int[] arr) {
    int left = 0, right = arr.length - 1;
    while (left < right) {
        int temp = arr[left];
        arr[left] = arr[right];
        arr[right] = temp;
        left++;
        right--;
    }
}
```

### 5. Minimum Spanning Tree (Kruskal's Algorithm)

**Problem**: Find the minimum spanning tree of a connected, undirected graph.

**Greedy Approach**: Sort edges by weight and add them to the MST if they don't form a cycle.

```java
public static List<Edge> kruskalMST(List<Edge> edges, int vertices) {
    // Sort all edges in non-decreasing order of their weight
    Collections.sort(edges, Comparator.comparingInt(e -> e.weight));
    
    // Allocate memory for creating vertices subsets
    DisjointSet ds = new DisjointSet(vertices);
    
    List<Edge> result = new ArrayList<>();
    
    // Number of edges to be taken is equal to vertices-1
    int e = 0, i = 0;
    while (e < vertices - 1 && i < edges.size()) {
        Edge nextEdge = edges.get(i++);
        
        int x = ds.find(nextEdge.src);
        int y = ds.find(nextEdge.dest);
        
        // If including this edge doesn't cause cycle, include it in result
        if (x != y) {
            result.add(nextEdge);
            ds.union(x, y);
            e++;
        }
    }
    
    return result;
}

static class Edge {
    int src, dest, weight;
    
    Edge(int src, int dest, int weight) {
        this.src = src;
        this.dest = dest;
        this.weight = weight;
    }
}

static class DisjointSet {
    int[] parent, rank;
    
    DisjointSet(int n) {
        parent = new int[n];
        rank = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }
    
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    }
    
    void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        if (rootX == rootY) return;
        
        // Union by rank
        if (rank[rootX] < rank[rootY]) {
            parent[rootX] = rootY;
        } else if (rank[rootX] > rank[rootY]) {
            parent[rootY] = rootX;
        } else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }
    }
}
```

## Advanced Greedy Problems

### 1. Job Sequencing with Deadlines

**Problem**: Given a set of jobs where each job has a deadline and profit associated with it. Earn maximum profit by completing jobs before their deadlines. Each job takes unit time to complete and only one job can be scheduled at a time.

```java
public static int[] jobScheduling(Job[] jobs, int n) {
    // Sort jobs based on profit in descending order
    Arrays.sort(jobs, (a, b) -> b.profit - a.profit);
    
    // Find the maximum deadline
    int maxDeadline = 0;
    for (Job job : jobs) {
        maxDeadline = Math.max(maxDeadline, job.deadline);
    }
    
    // Initialize result array and slot array
    int[] result = new int[maxDeadline];
    Arrays.fill(result, -1); // -1 means the slot is free
    
    int countJobs = 0, jobProfit = 0;
    
    // Iterate through all jobs
    for (int i = 0; i < n; i++) {
        // Find a free slot for this job (latest available free slot)
        for (int j = Math.min(maxDeadline - 1, jobs[i].deadline - 1); j >= 0; j--) {
            if (result[j] == -1) {
                result[j] = i; // Add this job to the sequence
                countJobs++;
                jobProfit += jobs[i].profit;
                break;
            }
        }
    }
    
    return new int[]{countJobs, jobProfit};
}

static class Job {
    char id;
    int deadline, profit;
    
    Job(char id, int deadline, int profit) {
        this.id = id;
        this.deadline = deadline;
        this.profit = profit;
    }
}
```

### 2. Minimum Number of Platforms

**Problem**: Given arrival and departure times of all trains that reach a railway station, find the minimum number of platforms required for the railway station so that no train waits.

```java
public static int findPlatform(int[] arr, int[] dep, int n) {
    // Sort arrival and departure arrays
    Arrays.sort(arr);
    Arrays.sort(dep);
    
    int platformsNeeded = 1, result = 1;
    int i = 1, j = 0;
    
    // Similar to merge in merge sort to process all events in sorted order
    while (i < n && j < n) {
        // If next event in sorted order is arrival, increment platforms needed
        if (arr[i] <= dep[j]) {
            platformsNeeded++;
            i++;
        }
        // If next event is departure, decrement platforms needed
        else {
            platformsNeeded--;
            j++;
        }
        
        result = Math.max(result, platformsNeeded);
    }
    
    return result;
}
```

### 3. Gas Station

**Problem**: Given two integer arrays gas and cost, where gas[i] represents the amount of gas at the ith station and cost[i] represents the cost to travel from the ith station to the (i+1)th station, find the starting gas station's index such that you can travel around the circuit once in the clockwise direction. Return -1 if there is no solution.

```java
public static int canCompleteCircuit(int[] gas, int[] cost) {
    int n = gas.length;
    
    int totalGas = 0, totalCost = 0;
    for (int i = 0; i < n; i++) {
        totalGas += gas[i];
        totalCost += cost[i];
    }
    
    // If total gas is less than total cost, there is no solution
    if (totalGas < totalCost) return -1;
    
    int startStation = 0;
    int currentGas = 0;
    
    for (int i = 0; i < n; i++) {
        // Calculate current gas after visiting station i
        currentGas += gas[i] - cost[i];
        
        // If we can't reach the next station, start from the next station
        if (currentGas < 0) {
            startStation = i + 1;
            currentGas = 0;
        }
    }
    
    return startStation;
}
```

## Common Interview Questions

### 1. Meeting Rooms II

**Problem**: Given an array of meeting time intervals consisting of start and end times, find the minimum number of conference rooms required.

```java
public static int minMeetingRooms(int[][] intervals) {
    if (intervals == null || intervals.length == 0) return 0;
    
    // Extract start and end times
    int n = intervals.length;
    int[] startTimes = new int[n];
    int[] endTimes = new int[n];
    
    for (int i = 0; i < n; i++) {
        startTimes[i] = intervals[i][0];
        endTimes[i] = intervals[i][1];
    }
    
    // Sort start and end times separately
    Arrays.sort(startTimes);
    Arrays.sort(endTimes);
    
    int rooms = 0, endIdx = 0;
    
    for (int startIdx = 0; startIdx < n; startIdx++) {
        // If the current meeting starts before the earliest end
        // we need a new room
        if (startTimes[startIdx] < endTimes[endIdx]) {
            rooms++;
        } else {
            // Otherwise we can reuse a room that just ended
            endIdx++;
        }
    }
    
    return rooms;
}
```

### 2. Task Scheduler

**Problem**: Given a character array representing tasks CPU need to do, and a non-negative integer n representing the cooldown period between two same tasks, return the least number of intervals the CPU will take to finish all the given tasks.

```java
public static int leastInterval(char[] tasks, int n) {
    // Count frequency of each task
    int[] frequencies = new int[26];
    for (char task : tasks) {
        frequencies[task - 'A']++;
    }
    
    // Sort frequencies in ascending order
    Arrays.sort(frequencies);
    
    // Max frequency
    int maxFreq = frequencies[25];
    
    // Number of slots in which we can place tasks
    int idleSlots = (maxFreq - 1) * n;
    
    // Fill idle slots with other tasks
    for (int i = 24; i >= 0 && frequencies[i] > 0; i--) {
        idleSlots -= Math.min(maxFreq - 1, frequencies[i]);
    }
    
    // If there are still idle slots, add them to the result
    return idleSlots > 0 ? idleSlots + tasks.length : tasks.length;
}
```

### 3. Minimum Cost to Connect Sticks

**Problem**: You have some sticks with positive integer lengths. You can connect any two sticks of lengths x and y into one stick by paying a cost of x + y. You perform this operation until there is one stick remaining. Return the minimum cost of connecting all the given sticks into one stick.

```java
public static int connectSticks(int[] sticks) {
    if (sticks.length <= 1) return 0;
    
    // Use a min heap to always get the two smallest sticks
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    
    // Add all sticks to the min heap
    for (int stick : sticks) {
        minHeap.add(stick);
    }
    
    int totalCost = 0;
    
    // Keep connecting the two smallest sticks until only one stick is left
    while (minHeap.size() > 1) {
        int first = minHeap.poll();
        int second = minHeap.poll();
        
        int cost = first + second;
        totalCost += cost;
        
        minHeap.add(cost);
    }
    
    return totalCost;
}
```

### 4. Maximum Subarray (Kadane's Algorithm)

**Problem**: Find the contiguous subarray within an array which has the largest sum.

```java
public static int maxSubArray(int[] nums) {
    int maxSoFar = nums[0];
    int maxEndingHere = nums[0];
    
    for (int i = 1; i < nums.length; i++) {
        maxEndingHere = Math.max(nums[i], maxEndingHere + nums[i]);
        maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
    
    return maxSoFar;
}
```

### 5. Jump Game

**Problem**: Given an array of non-negative integers, you are initially positioned at the first index of the array. Each element in the array represents your maximum jump length at that position. Determine if you are able to reach the last index.

```java
public static boolean canJump(int[] nums) {
    int maxReach = 0;
    
    for (int i = 0; i < nums.length; i++) {
        // If we can't reach current position, return false
        if (i > maxReach) return false;
        
        // Update the farthest we can reach from current position
        maxReach = Math.max(maxReach, i + nums[i]);
        
        // If we can already reach the end, return true
        if (maxReach >= nums.length - 1) return true;
    }
    
    return true;
}
```

## Practice Problems

1. **Minimum Spanning Tree (Prim's Algorithm)**

```java
public static int primMST(int[][] graph) {
    int V = graph.length;
    
    // Array to store constructed MST
    int[] parent = new int[V];
    
    // Key values used to pick minimum weight edge
    int[] key = new int[V];
    
    // To represent set of vertices not yet included in MST
    boolean[] mstSet = new boolean[V];
    
    // Initialize all keys as INFINITE
    Arrays.fill(key, Integer.MAX_VALUE);
    
    // Always include first vertex in MST
    key[0] = 0; // Make key 0 so that this vertex is picked as first vertex
    parent[0] = -1; // First node is always root of MST
    
    // The MST will have V vertices
    for (int count = 0; count < V - 1; count++) {
        // Pick the minimum key vertex from the set of vertices not yet included in MST
        int u = minKey(key, mstSet, V);
        
        // Add the picked vertex to the MST Set
        mstSet[u] = true;
        
        // Update key value and parent index of the adjacent vertices of the picked vertex
        for (int v = 0; v < V; v++) {
            // Update the key only if graph[u][v] is smaller than key[v]
            if (graph[u][v] != 0 && !mstSet[v] && graph[u][v] < key[v]) {
                parent[v] = u;
                key[v] = graph[u][v];
            }
        }
    }
    
    // Calculate the total weight of MST
    int totalWeight = 0;
    for (int i = 1; i < V; i++) {
        totalWeight += graph[i][parent[i]];
    }
    
    return totalWeight;
}

private static int minKey(int[] key, boolean[] mstSet, int V) {
    int min = Integer.MAX_VALUE, minIndex = -1;
    
    for (int v = 0; v < V; v++) {
        if (!mstSet[v] && key[v] < min) {
            min = key[v];
            minIndex = v;
        }
    }
    
    return minIndex;
}
```

2. **Assign Cookies**

```java
public static int findContentChildren(int[] g, int[] s) {
    Arrays.sort(g); // Sort children's greed factors
    Arrays.sort(s); // Sort cookie sizes
    
    int i = 0; // Child index
    
    for (int j = 0; i < g.length && j < s.length; j++) {
        // If this cookie can satisfy this child
        if (s[j] >= g[i]) {
            i++;
        }
    }
    
    return i; // Return the number of content children
}
```

3. **Queue Reconstruction by Height**

```java
public static int[][] reconstructQueue(int[][] people) {
    // Sort by height in descending order and by k value in ascending order
    Arrays.sort(people, (a, b) -> a[0] == b[0] ? a[1] - b[1] : b[0] - a[0]);
    
    List<int[]> result = new ArrayList<>();
    
    // Insert each person in the correct position
    for (int[] person : people) {
        result.add(person[1], person);
    }
    
    return result.toArray(new int[result.size()][2]);
}
```

4. **Minimum Number of Arrows to Burst Balloons**

```java
public static int findMinArrowShots(int[][] points) {
    if (points.length == 0) return 0;
    
    // Sort by end points
    Arrays.sort(points, Comparator.comparingInt(a -> a[1]));
    
    int arrowPos = points[0][1];
    int arrowCount = 1;
    
    for (int i = 1; i < points.length; i++) {
        // If the balloon starts after the end of the current arrow
        if (points[i][0] > arrowPos) {
            arrowCount++;
            arrowPos = points[i][1];
        }
    }
    
    return arrowCount;
}
```

5. **Non-overlapping Intervals**

```java
public static int eraseOverlapIntervals(int[][] intervals) {
    if (intervals.length == 0) return 0;
    
    // Sort by end time
    Arrays.sort(intervals, Comparator.comparingInt(a -> a[1]));
    
    int count = 1; // Count of non-overlapping intervals
    int end = intervals[0][1];
    
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] >= end) {
            // Current interval does not overlap with the previous one
            end = intervals[i][1];
            count++;
        }
    }
    
    return intervals.length - count;
}
```

## Tips for Solving Greedy Problems

1. **Understand the problem statement clearly**:
   - Identify what needs to be optimized (maximized or minimized)
   - Check if there are any constraints

2. **Identify the greedy choice**:
   - What local decision should you make at each step?
   - Why is this choice optimal?

3. **Prove the greedy choice**:
   - Convince yourself that making the greedy choice at each step will lead to the globally optimal solution
   - Counterexamples can help identify if a greedy approach won't work

4. **Implement efficiently**:
   - Sorting is often a key pre-processing step
   - Priority queues (heaps) are useful for dynamically finding the next best option

5. **Test with examples**:
   - Verify your solution with simple test cases
   - Check edge cases (empty input, minimum/maximum values)

## Limitations of Greedy Algorithms

While greedy algorithms are efficient and easy to implement, they have some limitations:

1. **Not Always Optimal**: Greedy algorithms don't always produce the optimal solution. For problems like the general Knapsack Problem, dynamic programming is required.

2. **Problem-Specific**: The greedy choice is problem-specific and may not be immediately obvious.

3. **Requires Proof**: The correctness of a greedy approach needs to be proven, unlike dynamic programming which is more generally applicable.

## Conclusion

Greedy algorithms are powerful tools for solving optimization problems efficiently. By making locally optimal choices, they can often find globally optimal solutions with less computational complexity than other approaches like dynamic programming.

When approaching a new problem, always consider if a greedy approach might work, but be prepared to validate your greedy choice and switch to another approach if necessary.

Remember that the key to mastering greedy algorithms is practice and understanding the underlying principles that make them work.