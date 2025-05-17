# Lambda Expressions and Functional Programming in Java

Java 8 introduced functional programming features to Java, allowing for more concise and expressive code. This guide covers lambda expressions, functional interfaces, streams, and other functional programming concepts in Java.

## Introduction to Functional Programming

Functional programming is a programming paradigm that treats computation as the evaluation of mathematical functions and avoids changing state and mutable data. In functional programming:

- Functions are first-class citizens (can be passed as arguments, returned from other functions, etc.)
- Emphasis on immutability and avoiding side effects
- Focus on declarative programming (what to do) rather than imperative programming (how to do it)

## Lambda Expressions

Lambda expressions provide a concise way to represent an anonymous function (a method without a name) that can be passed around as an argument or returned from a method call.

### Basic Syntax

```java
(parameters) -> expression
```

or

```java
(parameters) -> { statements; }
```

### Examples

```java
// Lambda with no parameters
Runnable runnable = () -> System.out.println("Hello, World!");

// Lambda with one parameter
Consumer<String> consumer = (msg) -> System.out.println(msg);

// Lambda with multiple parameters
BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;

// Lambda with a block of code
Comparator<String> comparator = (s1, s2) -> {
    if (s1.length() == s2.length()) {
        return s1.compareTo(s2);
    }
    return s1.length() - s2.length();
};
```

## Functional Interfaces

A functional interface is an interface with exactly one abstract method. Lambda expressions can be used to create instances of functional interfaces.

### Predefined Functional Interfaces

Java provides several functional interfaces in the `java.util.function` package:

#### 1. Function<T, R>

Represents a function that takes an argument of type T and returns a result of type R.

```java
Function<String, Integer> length = s -> s.length();
Integer result = length.apply("Hello"); // result = 5
```

#### 2. Predicate<T>

Represents a predicate (boolean-valued function) of one argument.

```java
Predicate<String> isEmpty = s -> s.isEmpty();
boolean result = isEmpty.test(""); // result = true
```

#### 3. Consumer<T>

Represents an operation that accepts a single input argument and returns no result.

```java
Consumer<String> print = s -> System.out.println(s);
print.accept("Hello, World!"); // Prints: Hello, World!
```

#### 4. Supplier<T>

Represents a supplier of results.

```java
Supplier<Double> random = () -> Math.random();
Double result = random.get(); // result = a random double value
```

#### 5. BinaryOperator<T>

Represents an operation upon two operands of the same type, producing a result of the same type.

```java
BinaryOperator<Integer> add = (a, b) -> a + b;
Integer result = add.apply(5, 3); // result = 8
```

### Creating Custom Functional Interfaces

You can create your own functional interfaces using the `@FunctionalInterface` annotation:

```java
@FunctionalInterface
interface MathOperation {
    int operate(int a, int b);
}

// Using the custom functional interface
MathOperation addition = (a, b) -> a + b;
MathOperation subtraction = (a, b) -> a - b;

int result1 = addition.operate(10, 5); // result1 = 15
int result2 = subtraction.operate(10, 5); // result2 = 5
```

## Method References

Method references provide a shorthand notation for lambda expressions that call a single method:

```java
// Instead of:
Function<String, Integer> length = s -> s.length();

// You can use:
Function<String, Integer> length = String::length;
```

### Types of Method References

1. **Reference to a static method**: `ContainingClass::staticMethodName`
   ```java
   Function<String, Integer> parseInt = Integer::parseInt;
   ```

2. **Reference to an instance method of a particular object**: `instance::instanceMethodName`
   ```java
   StringBuilder sb = new StringBuilder();
   Consumer<String> append = sb::append;
   ```

3. **Reference to an instance method of an arbitrary object of a particular type**: `ContainingType::methodName`
   ```java
   Comparator<String> comparing = String::compareTo;
   ```

4. **Reference to a constructor**: `ClassName::new`
   ```java
   Supplier<List<String>> listFactory = ArrayList::new;
   ```

## Stream API

The Stream API provides a powerful and expressive way to process collections of data. Streams represent a sequence of elements supporting sequential and parallel aggregate operations.

### Creating Streams

```java
// From a collection
List<String> list = Arrays.asList("apple", "banana", "cherry");
Stream<String> stream = list.stream();

// From an array
String[] array = {"apple", "banana", "cherry"};
Stream<String> stream = Arrays.stream(array);

// From individual values
Stream<String> stream = Stream.of("apple", "banana", "cherry");

// Infinite stream
Stream<Integer> infiniteStream = Stream.iterate(0, n -> n + 2); // Even numbers
Stream<Double> randomStream = Stream.generate(Math::random);
```

### Common Stream Operations

#### Intermediate Operations

These operations return a new stream and are lazy (they don't process elements until a terminal operation is invoked):

- `filter(Predicate<T> predicate)`: Filters elements based on a predicate
- `map(Function<T, R> mapper)`: Transforms elements
- `flatMap(Function<T, Stream<R>> mapper)`: Transforms each element into a stream and flattens the results
- `sorted()` / `sorted(Comparator<T> comparator)`: Sorts elements
- `distinct()`: Removes duplicates
- `limit(long maxSize)`: Limits the stream size
- `skip(long n)`: Skips the first n elements

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "Dave", "Eve");

// Filter names starting with 'A' or 'B', transform to uppercase, and sort
List<String> result = names.stream()
    .filter(name -> name.startsWith("A") || name.startsWith("B"))
    .map(String::toUpperCase)
    .sorted()
    .collect(Collectors.toList());
// result = [ALICE, BOB]
```

#### Terminal Operations

These operations produce a result or a side-effect and consume the stream:

- `forEach(Consumer<T> action)`: Performs an action for each element
- `collect(Collector<T, A, R> collector)`: Gathers elements into a collection
- `reduce(BinaryOperator<T> accumulator)`: Reduces elements to a single value
- `count()`: Counts elements
- `anyMatch(Predicate<T> predicate)`: Whether any elements match the predicate
- `allMatch(Predicate<T> predicate)`: Whether all elements match the predicate
- `noneMatch(Predicate<T> predicate)`: Whether no elements match the predicate
- `findFirst()` / `findAny()`: Finds the first/any element
- `min(Comparator<T> comparator)` / `max(Comparator<T> comparator)`: Finds min/max element
- `toArray()`: Converts to an array

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Sum of all numbers
int sum = numbers.stream()
    .reduce(0, Integer::sum);
// sum = 15

// Check if all numbers are positive
boolean allPositive = numbers.stream()
    .allMatch(n -> n > 0);
// allPositive = true

// Find maximum number
int max = numbers.stream()
    .max(Integer::compare)
    .orElse(0);
// max = 5
```

### Collectors

The `collect()` method is a terminal operation that transforms elements into a collection or other data structure. The `Collectors` class provides many useful factory methods:

```java
List<String> fruits = Arrays.asList("apple", "banana", "cherry", "apple", "banana");

// Collect as List
List<String> list = fruits.stream().collect(Collectors.toList());

// Collect as Set (removes duplicates)
Set<String> set = fruits.stream().collect(Collectors.toSet());

// Collect as Map
Map<String, Integer> map = fruits.stream()
    .distinct()
    .collect(Collectors.toMap(
        fruit -> fruit,
        fruit -> fruit.length()
    ));
// map = {banana=6, apple=5, cherry=6}

// Join elements into a single string
String joined = fruits.stream()
    .distinct()
    .collect(Collectors.joining(", "));
// joined = "apple, banana, cherry"

// Group by length
Map<Integer, List<String>> groupedByLength = fruits.stream()
    .collect(Collectors.groupingBy(String::length));
// groupedByLength = {5=[apple], 6=[banana, cherry]}

// Partition by condition
Map<Boolean, List<String>> partitioned = fruits.stream()
    .distinct()
    .collect(Collectors.partitioningBy(f -> f.length() > 5));
// partitioned = {false=[apple], true=[banana, cherry]}
```

### Parallel Streams

Streams can be processed in parallel to improve performance on multi-core processors:

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// Sequential stream
int sumSequential = numbers.stream()
    .reduce(0, Integer::sum);

// Parallel stream
int sumParallel = numbers.parallelStream()
    .reduce(0, Integer::sum);
```

**Note**: Parallel streams can improve performance for large collections, but they come with overhead and should be used judiciously. They're not always faster than sequential streams, especially for small collections or operations with little computation.

## Optional

`Optional<T>` is a container object that may or may not contain a non-null value. It helps avoid `NullPointerException` and explicitly indicates that a value might be absent.

### Creating Optional Objects

```java
// Empty Optional
Optional<String> empty = Optional.empty();

// Optional with a non-null value
Optional<String> present = Optional.of("Hello");

// Optional that might be null
String value = // may be null
Optional<String> nullable = Optional.ofNullable(value);
```

### Working with Optional Values

```java
Optional<String> optional = Optional.ofNullable(getValue());

// Check if value is present
if (optional.isPresent()) {
    String value = optional.get();
    System.out.println("Value: " + value);
} else {
    System.out.println("Value is absent");
}

// Modern approach using lambdas
optional.ifPresent(value -> System.out.println("Value: " + value));

// Get value or default
String result = optional.orElse("Default Value");

// Get value or compute default
String result = optional.orElseGet(() -> computeDefaultValue());

// Get value or throw exception
String result = optional.orElseThrow(() -> new NoSuchElementException("Value not found"));

// Map value if present
Optional<Integer> length = optional.map(String::length);

// Filter value
Optional<String> filtered = optional.filter(value -> value.length() > 3);
```

## Functional Programming Patterns

### 1. Function Composition

Combine multiple functions into a single function:

```java
Function<String, String> lowercase = String::toLowerCase;
Function<String, String> trim = String::trim;

// Compose functions
Function<String, String> trimAndLowercase = lowercase.compose(trim);
// equivalent to: s -> lowercase.apply(trim.apply(s))

String result = trimAndLowercase.apply("  HELLO  ");
// result = "hello"
```

### 2. Currying

Transform a function that takes multiple arguments into a sequence of functions that each take a single argument:

```java
// Traditional approach
BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;
int result1 = add.apply(2, 3); // result1 = 5

// Curried approach
Function<Integer, Function<Integer, Integer>> curriedAdd = a -> b -> a + b;
int result2 = curriedAdd.apply(2).apply(3); // result2 = 5
```

### 3. Pure Functions

Functions that always produce the same output for the same input and have no side effects:

```java
// Not pure: has side effect (modifies external state)
List<Integer> numbers = new ArrayList<>();
void addToList(int number) {
    numbers.add(number); // Modifies external state
}

// Pure: no side effects, same input always gives same output
int add(int a, int b) {
    return a + b;
}
```

### 4. Immutability

Avoid changing state. Instead, create new objects with the desired changes:

```java
// Mutable approach
List<Integer> mutableList = new ArrayList<>(Arrays.asList(1, 2, 3));
mutableList.add(4); // Modifies the list

// Immutable approach
List<Integer> originalList = List.of(1, 2, 3); // Immutable list
List<Integer> newList = Stream.concat(
    originalList.stream(),
    Stream.of(4)
).collect(Collectors.toUnmodifiableList()); // Creates a new list
```

## Real-World Examples

### Example 1: Filtering and Transforming Data

```java
class Person {
    private String name;
    private int age;
    private String gender;
    
    // Constructor, getters, and setters...
}

// Find all adult females and get their names
List<Person> people = getListOfPeople();

List<String> adultFemaleNames = people.stream()
    .filter(person -> person.getAge() >= 18)
    .filter(person -> person.getGender().equals("female"))
    .map(Person::getName)
    .collect(Collectors.toList());
```

### Example 2: Grouping and Aggregating Data

```java
class Product {
    private String category;
    private double price;
    
    // Constructor, getters, and setters...
}

// Calculate average price by category
List<Product> products = getListOfProducts();

Map<String, Double> avgPriceByCategory = products.stream()
    .collect(Collectors.groupingBy(
        Product::getCategory,
        Collectors.averagingDouble(Product::getPrice)
    ));
```

### Example 3: Building a Pipeline of Operations

```java
class Order {
    private LocalDate orderDate;
    private List<OrderItem> items;
    private String customerName;
    
    // Constructor, getters, and setters...
}

class OrderItem {
    private String productName;
    private int quantity;
    private double price;
    
    // Constructor, getters, and setters...
}

// Get total order value for each customer in the last month
List<Order> orders = getListOfOrders();
LocalDate oneMonthAgo = LocalDate.now().minusMonths(1);

Map<String, Double> totalValueByCustomer = orders.stream()
    .filter(order -> order.getOrderDate().isAfter(oneMonthAgo))
    .collect(Collectors.groupingBy(
        Order::getCustomerName,
        Collectors.summingDouble(order -> 
            order.getItems().stream()
                .mapToDouble(item -> item.getPrice() * item.getQuantity())
                .sum()
        )
    ));
```

### Example 4: Lazy Evaluation with Streams

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "Dave", "Eve");

// This pipeline doesn't do anything until a terminal operation is added
Stream<String> pipeline = names.stream()
    .filter(name -> {
        System.out.println("Filtering: " + name);
        return name.length() > 3;
    })
    .map(name -> {
        System.out.println("Mapping: " + name);
        return name.toUpperCase();
    });

// Nothing is printed until we add a terminal operation
System.out.println("Pipeline created, but not executed yet");

// Now the pipeline executes
List<String> result = pipeline.collect(Collectors.toList());
```

## Best Practices

1. **Favor immutability**: Use `final` variables and immutable data structures where possible.

2. **Keep functions pure**: Avoid side effects and make functions depend only on their inputs.

3. **Use method references**: Replace lambda expressions with method references when appropriate for better readability.

4. **Be careful with parallel streams**: Use parallel streams only when you have a large dataset and the operations are computationally intensive.

5. **Watch out for intermediate operations**: Remember that intermediate operations are lazy and won't execute until a terminal operation is called.

6. **Use appropriate collectors**: Select the right collector for your needs to streamline your code.

7. **Handle nulls properly**: Use `Optional` instead of null checks for cleaner code.

8. **Consider function composition**: Combine small, focused functions rather than writing large, complex ones.

## Common Pitfalls

1. **Overusing streams**: Not every operation needs to be a stream. Simple loops might be more readable for simple operations.

2. **Ignoring parallel stream considerations**: Parallel streams aren't always faster, and can cause issues with stateful operations.

3. **Using shared mutable state in lambdas**: This can lead to race conditions and unpredictable behavior.

4. **Inefficient use of terminal operations**: Some operations like `collect` can be expensive; consider more efficient alternatives when appropriate.

5. **Ignoring checked exceptions in lambdas**: Checked exceptions must be handled or wrapped in lambdas.

## Practice Exercises

1. Convert an array of strings to uppercase using streams.

```java
String[] names = {"alice", "bob", "charlie"};
String[] uppercaseNames = Arrays.stream(names)
    .map(String::toUpperCase)
    .toArray(String[]::new);
```

2. Filter a list of numbers to get only the even numbers.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
List<Integer> evenNumbers = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());
```

3. Calculate the sum of all numbers in a list.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum = numbers.stream()
    .mapToInt(Integer::intValue)
    .sum();
```

4. Create a function that takes a list of strings and returns the count of strings that start with a given letter.

```java
Function<Character, Function<List<String>, Long>> countStartingWith = 
    c -> list -> list.stream()
        .filter(s -> !s.isEmpty() && s.charAt(0) == c)
        .count();

List<String> words = Arrays.asList("apple", "banana", "avocado", "cherry", "apricot");
long countA = countStartingWith.apply('a').apply(words); // countA = 3
```

5. Find the longest string in a list of strings.

```java
List<String> words = Arrays.asList("apple", "banana", "avocado", "cherry", "apricot");
String longest = words.stream()
    .max(Comparator.comparing(String::length))
    .orElse("");
```

## Next Steps

To deepen your understanding of functional programming in Java, consider exploring:

1. [Java Stream API](/collections/stream-api.md): Learn more advanced stream operations
2. [Design Patterns](/design-patterns/README.md): See how functional programming can simplify design patterns
3. [Reactive Programming](/reactive-programming/README.md): Explore reactive programming with libraries like RxJava
4. [CompletableFuture](/concurrency/completable-future.md): Use functional programming with asynchronous computations