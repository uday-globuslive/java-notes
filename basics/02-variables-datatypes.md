# Variables, Data Types, and Operators in Java

## Variables

A variable is a container that holds values used in a Java program. Before using a variable, you must declare it with a specific data type.

### Variable Declaration Syntax:
```java
dataType variableName = value;
```

Examples:
```java
int age = 25;
double salary = 75000.50;
char grade = 'A';
boolean isEmployed = true;
String name = "John Doe";
```

### Variable Naming Rules:
1. Can contain letters, digits, underscore (_), and dollar sign ($)
2. Must begin with a letter, underscore, or dollar sign
3. Cannot use Java reserved keywords
4. Case-sensitive (age and Age are different variables)
5. Follow camelCase convention for variables (firstName, totalAmount)

### Constants:
Constants are variables whose values cannot be changed after initialization.

```java
final double PI = 3.14159;
final int MAX_USERS = 100;
```

## Data Types

Java has two categories of data types:

### 1. Primitive Data Types

| Data Type | Size | Description | Range |
|-----------|------|-------------|-------|
| byte | 1 byte | Integer type | -128 to 127 |
| short | 2 bytes | Integer type | -32,768 to 32,767 |
| int | 4 bytes | Integer type | -2^31 to 2^31-1 |
| long | 8 bytes | Integer type | -2^63 to 2^63-1 |
| float | 4 bytes | Floating-point | ~±3.40282347E+38F |
| double | 8 bytes | Floating-point | ~±1.79769313486231570E+308 |
| char | 2 bytes | Single character | 0 to 65,535 |
| boolean | 1 bit | True or false | true or false |

Examples:
```java
byte smallNumber = 100;
short mediumNumber = 32000;
int normalNumber = 2000000;
long largeNumber = 9223372036854775807L; // Note the 'L' suffix
float decimalNumber = 45.75f; // Note the 'f' suffix
double preciseNumber = 45.75;
char singleCharacter = 'A';
boolean isTrue = true;
```

### 2. Reference Data Types

Reference types refer to objects and include:
- Classes (String, Arrays, etc.)
- Interfaces
- Enum
- Arrays

Example:
```java
String text = "Hello World";
int[] numbers = {1, 2, 3, 4, 5};
```

### Type Casting

Converting one data type to another:

1. **Widening Casting** (automatic): converting a smaller type to a larger type
   ```java
   int myInt = 9;
   double myDouble = myInt; // Automatic casting: int to double
   ```

2. **Narrowing Casting** (manual): converting a larger type to a smaller type
   ```java
   double myDouble = 9.78;
   int myInt = (int) myDouble; // Manual casting: double to int
   ```

## Operators

### 1. Arithmetic Operators

| Operator | Description | Example |
|----------|-------------|---------|
| + | Addition | a + b |
| - | Subtraction | a - b |
| * | Multiplication | a * b |
| / | Division | a / b |
| % | Modulus (remainder) | a % b |
| ++ | Increment | a++ or ++a |
| -- | Decrement | a-- or --a |

### 2. Assignment Operators

| Operator | Example | Equivalent to |
|----------|---------|---------------|
| = | a = b | a = b |
| += | a += b | a = a + b |
| -= | a -= b | a = a - b |
| *= | a *= b | a = a * b |
| /= | a /= b | a = a / b |
| %= | a %= b | a = a % b |

### 3. Comparison Operators

| Operator | Description | Example |
|----------|-------------|---------|
| == | Equal to | a == b |
| != | Not equal to | a != b |
| > | Greater than | a > b |
| < | Less than | a < b |
| >= | Greater than or equal to | a >= b |
| <= | Less than or equal to | a <= b |

### 4. Logical Operators

| Operator | Description | Example |
|----------|-------------|---------|
| && | Logical AND | a && b |
| \|\| | Logical OR | a \|\| b |
| ! | Logical NOT | !a |

### 5. Bitwise Operators

| Operator | Description |
|----------|-------------|
| & | Bitwise AND |
| \| | Bitwise OR |
| ^ | Bitwise XOR |
| ~ | Bitwise complement |
| << | Left shift |
| >> | Right shift |
| >>> | Unsigned right shift |

## Examples

Let's see some examples that combine variables, data types, and operators:

```java
public class VariablesExample {
    public static void main(String[] args) {
        // Variable declarations
        int a = 10;
        int b = 5;
        double c = 7.5;
        String firstName = "John";
        String lastName = "Doe";
        
        // Arithmetic operations
        int sum = a + b;          // 15
        int difference = a - b;    // 5
        int product = a * b;       // 50
        int quotient = a / b;      // 2
        int remainder = a % b;     // 0
        
        // Increment and decrement
        int x = a++;               // x = 10, then a becomes 11
        int y = ++b;               // b becomes 6, then y = 6
        
        // Combined assignment
        int m = 10;
        m += 5;                    // m = 15
        
        // String concatenation
        String fullName = firstName + " " + lastName;  // "John Doe"
        
        // Comparison operations
        boolean isEqual = (a == b);            // false
        boolean isGreater = (a > b);           // true
        
        // Logical operations
        boolean logicalResult = (a > b) && (c > b);   // true
        
        // Print results
        System.out.println("Sum: " + sum);
        System.out.println("Full Name: " + fullName);
        System.out.println("Is a equal to b? " + isEqual);
        System.out.println("Logical result: " + logicalResult);
    }
}
```

## Practice Exercises

1. Create variables of each primitive data type and print their values
2. Perform all arithmetic operations on two numbers
3. Convert a temperature from Celsius to Fahrenheit (formula: F = C × 9/5 + 32)
4. Calculate the area and perimeter of a rectangle
5. Swap the values of two variables without using a third variable

## Next Steps

Continue to [Control Flow (if/else, switch, loops)](/basics/03-control-flow.md) to learn how to control the flow of execution in Java programs.