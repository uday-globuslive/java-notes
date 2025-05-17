# Java Collections Framework

The Java Collections Framework provides a unified architecture for representing and manipulating collections of objects. It contains interfaces, implementations, and algorithms to work with groups of objects.

## Collection Hierarchy

The Collections Framework is organized into a hierarchy of interfaces and their implementations:

```
                            Collection
                                |
                +---------------+---------------+
                |               |               |
              List            Queue           Set
                |               |               |
    +-----------+           +---+---+       +---+---+
    |           |           |       |       |       |
ArrayList    LinkedList   Deque  PriorityQ  HashSet  SortedSet
                            |                           |
                        ArrayDeque                   TreeSet
```

Additionally, there's a separate `Map` interface for key-value pairs:

```
                              Map
                               |
                    +----------+-----------+
                    |          |           |
                 HashMap    LinkedHashMap  SortedMap
                                              |
                                           TreeMap
```

## Core Interfaces

### 1. Collection

The root interface in the collection hierarchy. A collection represents a group of objects known as its elements.

```java
public interface Collection<E> extends Iterable<E> {
    // Basic operations
    boolean add(E e);
    boolean remove(Object o);
    boolean contains(Object o);
    
    // Bulk operations
    boolean addAll(Collection<? extends E> c);
    boolean removeAll(Collection<?> c);
    boolean retainAll(Collection<?> c);
    boolean containsAll(Collection<?> c);
    
    // Array operations
    Object[] toArray();
    <T> T[] toArray(T[] a);
    
    // Size operations
    int size();
    boolean isEmpty();
    void clear();
    
    // Iterator
    Iterator<E> iterator();
}
```

### 2. List

An ordered collection (sequence) that allows duplicate elements.

```java
public interface List<E> extends Collection<E> {
    // Positional access
    E get(int index);
    E set(int index, E element);
    void add(int index, E element);
    E remove(int index);
    
    // Search
    int indexOf(Object o);
    int lastIndexOf(Object o);
    
    // List iteration
    ListIterator<E> listIterator();
    ListIterator<E> listIterator(int index);
    
    // Range view
    List<E> subList(int fromIndex, int toIndex);
}
```

### 3. Set

A collection that cannot contain duplicate elements.

```java
public interface Set<E> extends Collection<E> {
    // Inherits methods from Collection
    // but adds the constraint that duplicates are not allowed
}
```

### 4. Queue

A collection designed for holding elements prior to processing.

```java
public interface Queue<E> extends Collection<E> {
    // Inserts
    boolean offer(E e);  // Adds element (non-blocking)
    
    // Removes
    E poll();  // Retrieves and removes head (null if empty)
    
    // Examines
    E peek();  // Retrieves but does not remove head (null if empty)
}
```

### 5. Deque

A double-ended queue that supports adding and removing elements from both ends.

```java
public interface Deque<E> extends Queue<E> {
    // Head operations
    void addFirst(E e);
    void offerFirst(E e);
    E removeFirst();
    E pollFirst();
    E getFirst();
    E peekFirst();
    
    // Tail operations
    void addLast(E e);
    void offerLast(E e);
    E removeLast();
    E pollLast();
    E getLast();
    E peekLast();
}
```

### 6. Map

A collection that maps keys to values, with no duplicate keys allowed.

```java
public interface Map<K, V> {
    // Basic operations
    V put(K key, V value);
    V get(Object key);
    V remove(Object key);
    boolean containsKey(Object key);
    boolean containsValue(Object value);
    
    // Bulk operations
    void putAll(Map<? extends K, ? extends V> m);
    
    // Collection views
    Set<K> keySet();
    Collection<V> values();
    Set<Map.Entry<K, V>> entrySet();
    
    // Size operations
    int size();
    boolean isEmpty();
    void clear();
    
    // Interface for map entries
    interface Entry<K, V> {
        K getKey();
        V getValue();
        V setValue(V value);
    }
}
```

## Common Implementations

### List Implementations

#### ArrayList

A resizable array implementation of the List interface.

- **Pros**: Fast random access, efficient when the size doesn't change often
- **Cons**: Slow insertion/deletion in the middle
- **Time Complexity**:
  - Access: O(1)
  - Search: O(n)
  - Insertion (at end): O(1) amortized
  - Insertion (at position): O(n)
  - Deletion (at end): O(1)
  - Deletion (at position): O(n)

```java
List<String> list = new ArrayList<>();
list.add("Apple");
list.add("Banana");
list.add("Cherry");
String fruit = list.get(1); // "Banana"
```

#### LinkedList

A doubly-linked list implementation of the List and Deque interfaces.

- **Pros**: Fast insertion/deletion
- **Cons**: Slow random access
- **Time Complexity**:
  - Access: O(n)
  - Search: O(n)
  - Insertion (at position with iterator): O(1)
  - Deletion (at position with iterator): O(1)

```java
List<String> linkedList = new LinkedList<>();
linkedList.add("Apple");
linkedList.add("Banana");
linkedList.addFirst("Cherry"); // LinkedList specific method
```

### Set Implementations

#### HashSet

An implementation of Set backed by a hash table (actually a HashMap).

- **Pros**: Very fast add, remove, contains operations
- **Cons**: No order guarantee
- **Time Complexity**: O(1) for basic operations (average case)

```java
Set<String> hashSet = new HashSet<>();
hashSet.add("Apple");
hashSet.add("Banana");
hashSet.add("Apple"); // Duplicate, not added
boolean containsApple = hashSet.contains("Apple"); // true
```

#### LinkedHashSet

Hash table and linked list implementation of the Set interface, with predictable iteration order.

- **Pros**: Maintains insertion order, fast operations
- **Cons**: Slightly slower than HashSet, more memory
- **Time Complexity**: O(1) for basic operations

```java
Set<String> linkedHashSet = new LinkedHashSet<>();
linkedHashSet.add("Banana");
linkedHashSet.add("Apple");
linkedHashSet.add("Cherry");
// Iterates in insertion order: Banana, Apple, Cherry
```

#### TreeSet

A NavigableSet implementation based on a TreeMap, which is a Red-Black tree.

- **Pros**: Elements are sorted, supports range queries
- **Cons**: Slower than HashSet
- **Time Complexity**: O(log n) for basic operations

```java
Set<String> treeSet = new TreeSet<>();
treeSet.add("Banana");
treeSet.add("Apple");
treeSet.add("Cherry");
// Iterates in natural order: Apple, Banana, Cherry
```

### Queue Implementations

#### ArrayDeque

Resizable-array implementation of the Deque interface.

- **Pros**: Faster than LinkedList for queue operations
- **Cons**: Not suitable when you need to remove elements from the middle
- **Time Complexity**: O(1) for most operations

```java
Deque<String> deque = new ArrayDeque<>();
deque.addFirst("First");
deque.addLast("Last");
String first = deque.removeFirst(); // "First"
```

#### PriorityQueue

An unbounded priority queue based on a priority heap.

- **Pros**: Efficient way to retrieve the smallest/largest element
- **Cons**: Not suitable for random access
- **Time Complexity**:
  - Insert: O(log n)
  - Remove: O(log n)
  - Peek: O(1)

```java
Queue<Integer> priorityQueue = new PriorityQueue<>();
priorityQueue.add(3);
priorityQueue.add(1);
priorityQueue.add(2);
int smallest = priorityQueue.poll(); // Returns 1
```

### Map Implementations

#### HashMap

Hash table based implementation of the Map interface.

- **Pros**: Very fast basic operations
- **Cons**: No order guarantee
- **Time Complexity**: O(1) for basic operations (average case)

```java
Map<String, Integer> hashMap = new HashMap<>();
hashMap.put("Apple", 10);
hashMap.put("Banana", 20);
int quantity = hashMap.get("Apple"); // 10
```

#### LinkedHashMap

Hash table and linked list implementation of the Map interface, with predictable iteration order.

- **Pros**: Maintains insertion order, fast operations
- **Cons**: Slightly slower than HashMap, more memory
- **Time Complexity**: O(1) for basic operations

```java
Map<String, Integer> linkedHashMap = new LinkedHashMap<>();
linkedHashMap.put("Banana", 20);
linkedHashMap.put("Apple", 10);
// Iterates in insertion order: Banana->20, Apple->10
```

#### TreeMap

A Red-Black tree based NavigableMap implementation.

- **Pros**: Sorted keys, supports range queries
- **Cons**: Slower than HashMap
- **Time Complexity**: O(log n) for basic operations

```java
Map<String, Integer> treeMap = new TreeMap<>();
treeMap.put("Banana", 20);
treeMap.put("Apple", 10);
treeMap.put("Cherry", 30);
// Iterates in key order: Apple->10, Banana->20, Cherry->30
```

## Utility Classes

### Collections

The `Collections` class provides static methods for operating on collections.

```java
// Sorting
Collections.sort(list);
Collections.sort(list, comparator);

// Searching
int index = Collections.binarySearch(list, key);

// Min/Max
T min = Collections.min(collection);
T max = Collections.max(collection);

// Shuffling
Collections.shuffle(list);

// Unmodifiable views
List<T> unmodList = Collections.unmodifiableList(list);
Map<K, V> unmodMap = Collections.unmodifiableMap(map);

// Synchronized views
List<T> syncList = Collections.synchronizedList(list);
Map<K, V> syncMap = Collections.synchronizedMap(map);
```

### Arrays

The `Arrays` class provides static methods for operating on arrays.

```java
// Sorting
Arrays.sort(array);
Arrays.sort(array, comparator);

// Searching
int index = Arrays.binarySearch(array, key);

// Copying
T[] copy = Arrays.copyOf(array, length);

// Converting to List
List<T> list = Arrays.asList(array);

// Filling
Arrays.fill(array, value);

// Equality
boolean equal = Arrays.equals(array1, array2);

// String representation
String s = Arrays.toString(array);
```

## Common Operations and Patterns

### Iterating Over Collections

#### Using for-each Loop

```java
for (String element : collection) {
    System.out.println(element);
}
```

#### Using Iterator

```java
Iterator<String> iterator = collection.iterator();
while (iterator.hasNext()) {
    String element = iterator.next();
    // Safe to remove during iteration
    if (element.startsWith("A")) {
        iterator.remove();
    }
}
```

#### Using Stream API (Java 8+)

```java
collection.stream()
    .filter(e -> e.startsWith("A"))
    .map(String::toUpperCase)
    .forEach(System.out::println);
```

### Iterating Over Maps

#### Using entrySet

```java
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    String key = entry.getKey();
    Integer value = entry.getValue();
    System.out.println(key + " -> " + value);
}
```

#### Using keySet and values

```java
// Keys only
for (String key : map.keySet()) {
    System.out.println(key);
}

// Values only
for (Integer value : map.values()) {
    System.out.println(value);
}
```

#### Using forEach (Java 8+)

```java
map.forEach((key, value) -> System.out.println(key + " -> " + value));
```

## Advanced Topics

### Generics in Collections

Collections use generics to ensure type safety at compile time.

```java
List<String> strings = new ArrayList<>();
strings.add("Hello"); // OK
strings.add(42); // Compile error
```

### Wildcards in Generics

Wildcards provide flexibility when working with generic types.

```java
// Upper bounded wildcard
void processElements(List<? extends Number> list) {
    // Can read elements as Number
    Number n = list.get(0);
    // Cannot add elements
}

// Lower bounded wildcard
void addElements(List<? super Integer> list) {
    // Can add Integers
    list.add(42);
    // Cannot reliably read elements as anything more specific than Object
    Object obj = list.get(0);
}

// Unbounded wildcard
void printElements(List<?> list) {
    // Can only read as Object
    for (Object o : list) {
        System.out.println(o);
    }
    // Cannot add elements (except null)
}
```

### Thread Safety

Most collection implementations are not thread-safe. For thread-safe collections, you can:

1. Use synchronized wrappers:
   ```java
   List<String> syncList = Collections.synchronizedList(new ArrayList<>());
   Map<String, Integer> syncMap = Collections.synchronizedMap(new HashMap<>());
   ```

2. Use concurrent collections from `java.util.concurrent`:
   ```java
   Map<String, Integer> concurrentMap = new ConcurrentHashMap<>();
   Queue<String> concurrentQueue = new ConcurrentLinkedQueue<>();
   ```

### Custom Collections

You can implement your own collection by extending existing ones or implementing interfaces directly.

```java
public class UniqueList<E> extends ArrayList<E> {
    @Override
    public boolean add(E element) {
        if (contains(element)) {
            return false;
        }
        return super.add(element);
    }
}
```

## Practice Exercises

1. Create a program that counts the frequency of words in a text file using a `Map`.
2. Implement a custom priority queue using a `List`.
3. Write a method that finds the intersection of two sets.
4. Create a program that maintains a list of tasks sorted by priority.
5. Implement a simple cache using `LinkedHashMap` with LRU (Least Recently Used) eviction policy.

## Next Steps

Proceed to the detailed guides for each collection type for more in-depth information:

- [List Implementations](list-implementations.md)
- [Set Implementations](set-implementations.md)
- [Map Implementations](map-implementations.md)
- [Queue and Deque Implementations](queue-implementations.md)
- [Collections Utility Class](collections-utility.md)