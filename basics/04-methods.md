# Methods in Java

Methods are blocks of code that perform a specific task and can be reused throughout a program. They help in organizing code, improving reusability, and make code maintenance easier.

## Method Declaration

The basic syntax of a method declaration:

```java
accessModifier returnType methodName(parameterList) {
    // method body
    // code to be executed
    return value; // if return type is not void
}
```

Components:
- **Access Modifier**: Determines the visibility of the method (e.g., `public`, `private`, `protected`)
- **Return Type**: Data type of the value returned by the method (or `void` if no value is returned)
- **Method Name**: Identifier that is used to call the method
- **Parameter List**: Input parameters that the method accepts (can be empty)
- **Method Body**: The code to be executed when the method is called
- **Return Statement**: Returns a value to the caller (if return type is not `void`)

## Types of Methods

### 1. Based on Return Types

#### Void Methods (No Return Value)
```java
public void printMessage(String message) {
    System.out.println(message);
    // No return statement needed
}
```

#### Methods that Return a Value
```java
public int add(int a, int b) {
    int sum = a + b;
    return sum; // Returns the sum to the caller
}
```

### 2. Based on Parameters

#### Methods with No Parameters
```java
public void sayHello() {
    System.out.println("Hello!");
}
```

#### Methods with Parameters
```java
public void greet(String name) {
    System.out.println("Hello, " + name + "!");
}
```

#### Methods with Multiple Parameters
```java
public double calculateArea(double length, double width) {
    return length * width;
}
```

### 3. Based on Static Keyword

#### Static Methods (Class Methods)
Static methods belong to the class rather than any specific instance. They can be called without creating an object of the class.

```java
public static int multiply(int a, int b) {
    return a * b;
}
```

Usage:
```java
int result = ClassName.multiply(5, 3); // Calling a static method
```

#### Instance Methods
Instance methods belong to instances of a class and require an object of the class to be called.

```java
public double calculateArea() {
    return this.length * this.width;
}
```

Usage:
```java
Rectangle rect = new Rectangle(5, 3);
double area = rect.calculateArea(); // Calling an instance method
```

## Method Overloading

Method overloading allows multiple methods in the same class to have the same name but different parameter lists.

```java
public class Calculator {
    // Method with two int parameters
    public int add(int a, int b) {
        return a + b;
    }
    
    // Overloaded method with three int parameters
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    // Overloaded method with two double parameters
    public double add(double a, double b) {
        return a + b;
    }
}
```

Usage:
```java
Calculator calc = new Calculator();
int sum1 = calc.add(5, 10);            // Calls the first method
int sum2 = calc.add(5, 10, 15);        // Calls the second method
double sum3 = calc.add(5.5, 10.5);     // Calls the third method
```

## Recursion

Recursion is a technique where a method calls itself to solve a problem.

Example: Factorial calculation using recursion
```java
public int factorial(int n) {
    // Base case
    if (n == 0 || n == 1) {
        return 1;
    }
    // Recursive case
    else {
        return n * factorial(n - 1);
    }
}
```

Usage:
```java
int result = factorial(5); // Returns 120 (5 * 4 * 3 * 2 * 1)
```

## Variable Arguments (Varargs)

Variable arguments allow methods to accept an arbitrary number of values as a single parameter.

```java
public int sum(int... numbers) {
    int total = 0;
    for (int num : numbers) {
        total += num;
    }
    return total;
}
```

Usage:
```java
int sum1 = sum(1, 2);             // Returns 3
int sum2 = sum(1, 2, 3, 4, 5);    // Returns 15
int sum3 = sum();                 // Returns 0
```

## Method Parameters

### Pass by Value

In Java, all parameters are passed by value:
- For primitive types, a copy of the value is passed
- For objects, a copy of the reference is passed

Example with primitive types:
```java
public void modifyValue(int x) {
    x = x * 2; // This modification doesn't affect the original value
}

public static void main(String[] args) {
    int a = 10;
    modifyValue(a);
    System.out.println(a); // Still prints 10
}
```

Example with objects:
```java
public void modifyList(List<Integer> list) {
    list.add(4); // This affects the original list because we're modifying the object the reference points to
}

public static void main(String[] args) {
    List<Integer> myList = new ArrayList<>();
    myList.add(1);
    myList.add(2);
    myList.add(3);
    
    modifyList(myList);
    System.out.println(myList); // Prints [1, 2, 3, 4]
}
```

## Scope of Variables in Methods

Variables declared within a method are called local variables and are only accessible within that method.

```java
public void exampleMethod() {
    int localVar = 10; // Local variable
    System.out.println(localVar); // Accessible here
}

public void anotherMethod() {
    // System.out.println(localVar); // Error: localVar is not accessible here
}
```

## Complete Example

```java
public class MethodsExample {
    // Instance variable
    private int value;
    
    // Constructor
    public MethodsExample(int value) {
        this.value = value;
    }
    
    // Instance method that uses the instance variable
    public int getValue() {
        return this.value;
    }
    
    // Instance method that takes parameters
    public void setValue(int value) {
        this.value = value;
    }
    
    // Static method (belongs to the class)
    public static int square(int number) {
        return number * number;
    }
    
    // Method with multiple parameters
    public static double calculateCircleArea(double radius) {
        return Math.PI * radius * radius;
    }
    
    // Method with return value
    public int add(int a, int b) {
        return a + b;
    }
    
    // Void method (no return value)
    public void printValue() {
        System.out.println("Current value: " + value);
    }
    
    // Method using varargs
    public static int findMax(int... numbers) {
        if (numbers.length == 0) {
            throw new IllegalArgumentException("At least one number must be provided");
        }
        
        int max = numbers[0];
        for (int i = 1; i < numbers.length; i++) {
            if (numbers[i] > max) {
                max = numbers[i];
            }
        }
        
        return max;
    }
    
    // Recursive method
    public static int fibonacci(int n) {
        if (n <= 1) {
            return n;
        }
        return fibonacci(n - 1) + fibonacci(n - 2);
    }
    
    // Main method - program entry point
    public static void main(String[] args) {
        // Create an object of the class
        MethodsExample example = new MethodsExample(5);
        
        // Call instance methods
        example.printValue(); // Prints "Current value: 5"
        example.setValue(10);
        example.printValue(); // Prints "Current value: 10"
        
        // Call static methods
        int squared = MethodsExample.square(4); // Returns 16
        System.out.println("4 squared is: " + squared);
        
        // Calculate and print circle area
        double area = MethodsExample.calculateCircleArea(3.0);
        System.out.println("Area of circle with radius 3.0: " + area);
        
        // Use method with return value
        int sum = example.add(5, 7);
        System.out.println("5 + 7 = " + sum);
        
        // Use varargs method
        int maximum = MethodsExample.findMax(10, 5, 25, 8, 15);
        System.out.println("The maximum value is: " + maximum);
        
        // Use recursive method
        System.out.println("The 6th Fibonacci number is: " + fibonacci(6));
    }
}
```

## Practice Exercises

1. Write a method to check if a number is even or odd
2. Write a method to find the greatest common divisor (GCD) of two numbers
3. Write a method to calculate the power of a number (without using Math.pow)
4. Write a recursive method to calculate the sum of digits of a number
5. Write a method that takes an array and returns the reverse of that array
6. Write a method to check if a string is a palindrome
7. Implement a method that converts decimal to binary
8. Create a simple calculator class with methods for basic operations

## Next Steps

Continue to [Arrays and Basic Data Structures](/basics/05-arrays.md) to learn how to work with collections of data in Java.