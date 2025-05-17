# Hash Tables

Hash tables are data structures that provide fast insertion, deletion, and lookup operations. They are implemented in Java through the `HashMap` and `HashSet` classes, and are essential for solving many coding interview problems efficiently.

## Key Concepts

### 1. Hash Function

A hash function maps data of arbitrary size to fixed-size values. In hash tables, it converts keys into array indices.

Characteristics of a good hash function:
- Fast computation
- Uniform distribution of hash values
- Minimal collisions
- Deterministic (same input always produces same output)

### 2. Collision Resolution

Collisions occur when multiple keys hash to the same index. There are several strategies to handle collisions:

#### Chaining (Open Hashing)

Each bucket in the hash table stores a linked list of all key-value pairs that hash to the same index.

```
[0] → (key1, value1) → (key2, value2)
[1] → (key3, value3)
[2] → null
[3] → (key4, value4) → (key5, value5) → (key6, value6)
...
```

#### Open Addressing (Closed Hashing)

When a collision occurs, the algorithm searches for another open spot in the array. Common probing techniques include:

- **Linear Probing**: Check the next slot sequentially.
- **Quadratic Probing**: Check slots that are quadratic distances away.
- **Double Hashing**: Use a second hash function to determine the step size.

### 3. Load Factor and Rehashing

The load factor is the ratio of entries to buckets: `load factor = n/k` where `n` is the number of entries and `k` is the number of buckets.

When the load factor exceeds a threshold (typically 0.75 in Java), the hash table is resized (usually doubled) and all existing entries are rehashed into the new table.

## Hash Table Operations

| Operation | Average Time | Worst Time |
|-----------|--------------|------------|
| Search    | O(1)         | O(n)       |
| Insert    | O(1)         | O(n)       |
| Delete    | O(1)         | O(n)       |

## HashMap in Java

`HashMap` is the primary implementation of the Map interface in Java using a hash table.

### Basic Operations

```java
import java.util.HashMap;
import java.util.Map;

public class HashMapExample {
    public static void main(String[] args) {
        // Create a HashMap
        Map<String, Integer> scores = new HashMap<>();
        
        // Add key-value pairs
        scores.put("Alice", 95);
        scores.put("Bob", 80);
        scores.put("Charlie", 90);
        
        // Get a value by key
        int aliceScore = scores.get("Alice"); // Returns 95
        
        // Check if a key exists
        boolean hasKey = scores.containsKey("David"); // Returns false
        
        // Check if a value exists
        boolean hasValue = scores.containsValue(90); // Returns true
        
        // Remove a key-value pair
        scores.remove("Bob");
        
        // Get all keys
        for (String name : scores.keySet()) {
            System.out.println(name);
        }
        
        // Get all values
        for (int score : scores.values()) {
            System.out.println(score);
        }
        
        // Iterate through all entries
        for (Map.Entry<String, Integer> entry : scores.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
        
        // Get size
        int size = scores.size(); // Returns 2
        
        // Check if empty
        boolean isEmpty = scores.isEmpty(); // Returns false
        
        // Clear all entries
        scores.clear();
    }
}
```

### Common Methods

| Method | Description |
|--------|-------------|
| `put(K key, V value)` | Adds or updates a key-value pair |
| `get(Object key)` | Returns the value for a key or null if not found |
| `remove(Object key)` | Removes the key-value pair for a key |
| `containsKey(Object key)` | Checks if a key exists |
| `containsValue(Object value)` | Checks if a value exists |
| `keySet()` | Returns a Set of all keys |
| `values()` | Returns a Collection of all values |
| `entrySet()` | Returns a Set of all key-value pairs |
| `size()` | Returns the number of key-value pairs |
| `isEmpty()` | Checks if the map is empty |
| `clear()` | Removes all key-value pairs |
| `putIfAbsent(K key, V value)` | Adds a key-value pair only if the key is not present |
| `getOrDefault(Object key, V defaultValue)` | Returns the value for a key or a default value if not found |

## HashSet in Java

`HashSet` implements the Set interface and uses a HashMap internally to store unique elements.

### Basic Operations

```java
import java.util.HashSet;
import java.util.Set;

public class HashSetExample {
    public static void main(String[] args) {
        // Create a HashSet
        Set<String> fruits = new HashSet<>();
        
        // Add elements
        fruits.add("Apple");
        fruits.add("Banana");
        fruits.add("Orange");
        fruits.add("Apple"); // Duplicate, will not be added
        
        // Check if an element exists
        boolean hasApple = fruits.contains("Apple"); // Returns true
        
        // Remove an element
        fruits.remove("Banana");
        
        // Iterate through elements
        for (String fruit : fruits) {
            System.out.println(fruit);
        }
        
        // Get size
        int size = fruits.size(); // Returns 2
        
        // Check if empty
        boolean isEmpty = fruits.isEmpty(); // Returns false
        
        // Clear all elements
        fruits.clear();
    }
}
```

### Common Methods

| Method | Description |
|--------|-------------|
| `add(E e)` | Adds an element |
| `remove(Object o)` | Removes an element |
| `contains(Object o)` | Checks if an element exists |
| `size()` | Returns the number of elements |
| `isEmpty()` | Checks if the set is empty |
| `clear()` | Removes all elements |
| `iterator()` | Returns an iterator over the elements |

## Using Custom Objects in Hash Tables

When using custom objects as keys in a hash table, you must properly implement the `hashCode()` and `equals()` methods.

```java
public class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // Getters...
    
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        
        Person person = (Person) o;
        
        if (age != person.age) return false;
        return name != null ? name.equals(person.name) : person.name == null;
    }
    
    @Override
    public int hashCode() {
        int result = name != null ? name.hashCode() : 0;
        result = 31 * result + age;
        return result;
    }
}
```

Usage:

```java
Map<Person, String> personAddresses = new HashMap<>();
personAddresses.put(new Person("Alice", 30), "New York");
personAddresses.put(new Person("Bob", 25), "San Francisco");

// This will return "New York" because the equals() method identifies it as the same person
String address = personAddresses.get(new Person("Alice", 30));
```

## Common Hash Table Problem Patterns

### 1. Frequency Counting

Count the occurrences of elements in a dataset.

#### Example: Find the Most Frequent Element

```java
public int mostFrequent(int[] nums) {
    Map<Integer, Integer> frequencyMap = new HashMap<>();
    
    // Count frequencies
    for (int num : nums) {
        frequencyMap.put(num, frequencyMap.getOrDefault(num, 0) + 1);
    }
    
    // Find the most frequent
    int maxFreq = 0;
    int mostFrequentElement = nums[0];
    
    for (Map.Entry<Integer, Integer> entry : frequencyMap.entrySet()) {
        if (entry.getValue() > maxFreq) {
            maxFreq = entry.getValue();
            mostFrequentElement = entry.getKey();
        }
    }
    
    return mostFrequentElement;
}
```

### 2. Two-Sum Pattern

Find pairs that satisfy certain conditions.

#### Example: Two Sum Problem

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        
        if (map.containsKey(complement)) {
            return new int[] { map.get(complement), i };
        }
        
        map.put(nums[i], i);
    }
    
    return new int[] {}; // No solution
}
```

### 3. Grouping

Group elements that share a common property.

#### Example: Group Anagrams

```java
public List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> map = new HashMap<>();
    
    for (String str : strs) {
        char[] chars = str.toCharArray();
        Arrays.sort(chars);
        String key = new String(chars);
        
        if (!map.containsKey(key)) {
            map.put(key, new ArrayList<>());
        }
        
        map.get(key).add(str);
    }
    
    return new ArrayList<>(map.values());
}
```

### 4. Caching

Store computation results to avoid redundant work.

#### Example: Fibonacci with Memoization

```java
public int fibonacci(int n) {
    Map<Integer, Integer> memo = new HashMap<>();
    return fibHelper(n, memo);
}

private int fibHelper(int n, Map<Integer, Integer> memo) {
    if (n <= 1) {
        return n;
    }
    
    if (memo.containsKey(n)) {
        return memo.get(n);
    }
    
    int result = fibHelper(n - 1, memo) + fibHelper(n - 2, memo);
    memo.put(n, result);
    
    return result;
}
```

## Practice Problems

1. **Valid Anagram**: Given two strings, determine if one is an anagram of the other.

2. **First Unique Character**: Find the first non-repeating character in a string.

3. **Intersection of Two Arrays**: Find the intersection of two arrays.

4. **Longest Substring Without Repeating Characters**: Find the length of the longest substring without repeating characters.

5. **Isomorphic Strings**: Determine if two strings are isomorphic.

6. **Subarray Sum Equals K**: Find the number of continuous subarrays that sum to a given value.

7. **LRU Cache**: Implement a Least Recently Used (LRU) cache.

8. **Top K Frequent Elements**: Find the k most frequent elements in an array.

## Next Steps

Continue to [Trees](/dsa/trees/README.md) to learn about hierarchical data structures.