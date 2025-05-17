# Exception Handling in Java

Exception handling in Java allows you to handle runtime errors and exceptional conditions in a structured and controlled way. This guide covers Java's exception mechanism, exception hierarchy, and best practices.

## Exception Hierarchy

In Java, all exceptions are derived from the `Throwable` class, which has two main subclasses: `Exception` and `Error`.

```
               Throwable
                  /\
                 /  \
                /    \
           Exception  Error
              /\
             /  \
            /    \
RuntimeException  Other Exceptions
```

### Types of Exceptions

1. **Checked Exceptions**: Exceptions that must be either caught or declared in the method's `throws` clause.
   - Example: `IOException`, `SQLException`, `ClassNotFoundException`

2. **Unchecked Exceptions**: Exceptions that don't need to be explicitly caught or declared.
   - **Runtime Exceptions**: Subclasses of `RuntimeException`, such as `NullPointerException`, `ArrayIndexOutOfBoundsException`
   - **Errors**: Represent serious problems that a reasonable application should not try to catch, like `OutOfMemoryError`, `StackOverflowError`

## Basic Exception Handling

### The try-catch Block

```java
try {
    // Code that might throw an exception
    int result = 10 / 0; // ArithmeticException
} catch (ArithmeticException e) {
    // Code to handle the exception
    System.out.println("Cannot divide by zero: " + e.getMessage());
}
```

### Multiple catch Blocks

```java
try {
    // Code that might throw different exceptions
    int[] numbers = new int[5];
    numbers[10] = 50; // ArrayIndexOutOfBoundsException
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Array index out of bounds: " + e.getMessage());
} catch (Exception e) {
    System.out.println("Some other exception: " + e.getMessage());
}
```

### The finally Block

The `finally` block is always executed, whether an exception is thrown or not.

```java
FileInputStream file = null;
try {
    file = new FileInputStream("file.txt");
    // Process file
} catch (IOException e) {
    System.out.println("Error processing file: " + e.getMessage());
} finally {
    // This block is always executed
    try {
        if (file != null) {
            file.close();
        }
    } catch (IOException e) {
        System.out.println("Error closing file: " + e.getMessage());
    }
}
```

### Try-with-Resources (Java 7+)

The try-with-resources statement automatically closes resources that implement `AutoCloseable`.

```java
try (FileInputStream file = new FileInputStream("file.txt");
     Scanner scanner = new Scanner(file)) {
    // Process file and scanner
    // Resources are automatically closed when the block exits
} catch (IOException e) {
    System.out.println("Error processing file: " + e.getMessage());
}
```

## Throwing Exceptions

You can throw exceptions explicitly using the `throw` keyword.

```java
public void validateAge(int age) {
    if (age < 0) {
        throw new IllegalArgumentException("Age cannot be negative");
    }
}
```

## Declaring Exceptions

Methods that might throw checked exceptions must declare them using the `throws` clause.

```java
public void readFile(String filename) throws IOException {
    FileInputStream file = new FileInputStream(filename);
    // Process file
    file.close();
}
```

## Creating Custom Exceptions

You can create your own exception classes by extending existing exception classes.

```java
// Checked custom exception
public class InsufficientFundsException extends Exception {
    private double amount;
    
    public InsufficientFundsException(double amount) {
        super("Insufficient funds: shortage of $" + amount);
        this.amount = amount;
    }
    
    public double getAmount() {
        return amount;
    }
}

// Unchecked custom exception
public class InvalidUserException extends RuntimeException {
    public InvalidUserException(String message) {
        super(message);
    }
}
```

Usage:

```java
public class BankAccount {
    private double balance;
    
    public void withdraw(double amount) throws InsufficientFundsException {
        if (amount > balance) {
            throw new InsufficientFundsException(amount - balance);
        }
        balance -= amount;
    }
}
```

## Exception Chaining

You can chain exceptions to preserve the original cause of an exception.

```java
try {
    // Some code that throws an exception
    throw new SQLException("Database error");
} catch (SQLException e) {
    // Wrap the exception and rethrow
    throw new RuntimeException("Error processing request", e);
}
```

## Multi-catch Block (Java 7+)

You can catch multiple exception types in a single catch block.

```java
try {
    // Code that might throw different exceptions
} catch (IOException | SQLException e) {
    // Handle both IOException and SQLException
    System.out.println("Error: " + e.getMessage());
}
```

## Common Java Exceptions

### Runtime Exceptions (Unchecked)

1. **NullPointerException**
   - Thrown when attempting to use a `null` reference
   ```java
   String str = null;
   int length = str.length(); // NullPointerException
   ```

2. **ArrayIndexOutOfBoundsException**
   - Thrown when attempting to access an array with an invalid index
   ```java
   int[] array = new int[5];
   int value = array[10]; // ArrayIndexOutOfBoundsException
   ```

3. **ArithmeticException**
   - Thrown for arithmetic errors, such as division by zero
   ```java
   int result = 10 / 0; // ArithmeticException
   ```

4. **ClassCastException**
   - Thrown when attempting to cast an object to an incompatible type
   ```java
   Object obj = new Integer(10);
   String str = (String) obj; // ClassCastException
   ```

5. **IllegalArgumentException**
   - Thrown when a method receives an argument that is not valid
   ```java
   public void setAge(int age) {
       if (age < 0) {
           throw new IllegalArgumentException("Age cannot be negative");
       }
       this.age = age;
   }
   ```

6. **IllegalStateException**
   - Thrown when a method is invoked at an inappropriate time
   ```java
   Iterator<String> iterator = list.iterator();
   iterator.remove(); // IllegalStateException (no element to remove)
   ```

### Checked Exceptions

1. **IOException**
   - Thrown when an I/O operation fails
   ```java
   FileInputStream file = new FileInputStream("nonexistent.txt"); // FileNotFoundException
   ```

2. **SQLException**
   - Thrown when a database access error occurs
   ```java
   Connection conn = DriverManager.getConnection("invalid_url"); // SQLException
   ```

3. **ClassNotFoundException**
   - Thrown when an application tries to load a class but the class cannot be found
   ```java
   Class.forName("nonexistent.Class"); // ClassNotFoundException
   ```

4. **InterruptedException**
   - Thrown when a thread is interrupted
   ```java
   Thread.sleep(1000); // InterruptedException
   ```

## Best Practices for Exception Handling

### 1. Handle Exceptions at the Appropriate Level

Not every method that can throw an exception should catch it. Handle exceptions at the level where you can do something meaningful about it.

```java
// Lower-level method declares the exception
public void readFile(String filename) throws IOException {
    // Read file logic
}

// Higher-level method handles the exception
public void processFile(String filename) {
    try {
        readFile(filename);
    } catch (IOException e) {
        System.out.println("Could not process file: " + e.getMessage());
        // Log the exception, notify user, or use a default value
    }
}
```

### 2. Catch Specific Exceptions First

When using multiple catch blocks, catch more specific exceptions before more general ones.

```java
try {
    // Code that might throw exceptions
} catch (NullPointerException e) {
    // Handle NullPointerException
} catch (RuntimeException e) {
    // Handle other RuntimeExceptions
} catch (Exception e) {
    // Handle all other exceptions
}
```

### 3. Don't Swallow Exceptions

Avoid empty catch blocks or catch blocks that hide exceptions without proper handling.

```java
// Bad: Swallowing exception
try {
    // Risky operation
} catch (Exception e) {
    // Nothing here - bad practice!
}

// Better: At least log the exception
try {
    // Risky operation
} catch (Exception e) {
    logger.error("Operation failed", e);
    // Take appropriate action
}
```

### 4. Use Custom Exceptions Appropriately

Create custom exceptions when you need to provide specific information about an error or when standard exceptions don't adequately describe your error.

### 5. Clean Up Resources Properly

Always clean up resources (files, connections, etc.) in `finally` blocks or use try-with-resources.

### 6. Include Useful Information in Exceptions

When throwing exceptions, include enough information to understand what went wrong.

```java
// Bad: Not enough information
throw new IllegalArgumentException("Invalid value");

// Better: More specific information
throw new IllegalArgumentException("Age must be between 0 and 120, but got: " + age);
```

### 7. Document Exceptions

Document exceptions in your method's Javadoc using the `@throws` tag.

```java
/**
 * Withdraws the specified amount from this account.
 *
 * @param amount the amount to withdraw
 * @throws InsufficientFundsException if the amount exceeds the balance
 */
public void withdraw(double amount) throws InsufficientFundsException {
    // Implementation
}
```

## Advanced Exception Handling Techniques

### Using Assertions

Assertions are used to verify assumptions during development. They are disabled by default in production.

```java
assert age > 0 : "Age must be positive";
```

Enable assertions when running the program:
```
java -ea MyProgram
```

### Exception Filtering (Java 7+)

You can filter exceptions in a catch block based on certain conditions.

```java
try {
    // Code that might throw exceptions
} catch (SQLException e) {
    if (e.getErrorCode() == 1062) {
        // Handle duplicate entry error
    } else {
        // Handle other SQL errors
        throw e;
    }
}
```

### Exception Handling with Lambdas and Streams (Java 8+)

```java
// Converting checked exceptions to unchecked in streams
List<String> fileContents = fileNames.stream()
    .map(name -> {
        try {
            return readFile(name);
        } catch (IOException e) {
            throw new UncheckedIOException(e);
        }
    })
    .collect(Collectors.toList());
```

## Practice Exercises

1. Create a custom exception for validating user input in a registration form.
2. Write a method that reads multiple files and handles exceptions appropriately.
3. Implement a method that converts checked exceptions to unchecked exceptions.
4. Create a robust file processing system with proper exception handling.
5. Implement a retry mechanism for operations that might fail temporarily.

## Next Steps

- Learn about [Java I/O](/io/README.md) for more file handling techniques
- Explore [Java Concurrency](/concurrency/README.md) to understand thread interruption exceptions
- Study [JDBC](/jdbc/README.md) for database exception handling