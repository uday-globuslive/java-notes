# Control Flow in Java

Control flow statements determine the order in which statements are executed in a program. Java provides several structures to control the flow of your program.

## Conditional Statements

### 1. if Statement

The `if` statement executes a block of code if a specified condition is true.

```java
if (condition) {
    // code to execute if condition is true
}
```

Example:
```java
int age = 18;
if (age >= 18) {
    System.out.println("You are an adult.");
}
```

### 2. if-else Statement

The `if-else` statement executes one block of code if a condition is true and another block if the condition is false.

```java
if (condition) {
    // code to execute if condition is true
} else {
    // code to execute if condition is false
}
```

Example:
```java
int age = 16;
if (age >= 18) {
    System.out.println("You are an adult.");
} else {
    System.out.println("You are a minor.");
}
```

### 3. if-else-if Ladder

The `if-else-if` ladder tests multiple conditions and executes the block corresponding to the first true condition.

```java
if (condition1) {
    // code to execute if condition1 is true
} else if (condition2) {
    // code to execute if condition2 is true
} else if (condition3) {
    // code to execute if condition3 is true
} else {
    // code to execute if none of the conditions are true
}
```

Example:
```java
int score = 85;
if (score >= 90) {
    System.out.println("Grade: A");
} else if (score >= 80) {
    System.out.println("Grade: B");
} else if (score >= 70) {
    System.out.println("Grade: C");
} else if (score >= 60) {
    System.out.println("Grade: D");
} else {
    System.out.println("Grade: F");
}
```

### 4. Nested if Statements

You can place `if` statements inside other `if` statements.

```java
if (condition1) {
    // code to execute if condition1 is true
    if (condition2) {
        // code to execute if both condition1 and condition2 are true
    }
}
```

Example:
```java
int age = 25;
boolean hasLicense = true;

if (age >= 18) {
    if (hasLicense) {
        System.out.println("You can drive.");
    } else {
        System.out.println("You need a license to drive.");
    }
} else {
    System.out.println("You are too young to drive.");
}
```

### 5. Ternary Operator

The ternary operator is a shorthand for the `if-else` statement.

```java
variable = (condition) ? valueIfTrue : valueIfFalse;
```

Example:
```java
int age = 20;
String status = (age >= 18) ? "adult" : "minor";
System.out.println("You are a " + status);
```

## Switch Statement

The `switch` statement tests a variable against multiple values and executes the code block corresponding to the matching value.

```java
switch (expression) {
    case value1:
        // code to execute if expression equals value1
        break;
    case value2:
        // code to execute if expression equals value2
        break;
    // more cases
    default:
        // code to execute if expression doesn't match any case
}
```

Example:
```java
int day = 3;
switch (day) {
    case 1:
        System.out.println("Monday");
        break;
    case 2:
        System.out.println("Tuesday");
        break;
    case 3:
        System.out.println("Wednesday");
        break;
    case 4:
        System.out.println("Thursday");
        break;
    case 5:
        System.out.println("Friday");
        break;
    case 6:
        System.out.println("Saturday");
        break;
    case 7:
        System.out.println("Sunday");
        break;
    default:
        System.out.println("Invalid day");
}
```

Enhanced Switch (Java 14+):
```java
int day = 3;
String dayName = switch (day) {
    case 1 -> "Monday";
    case 2 -> "Tuesday";
    case 3 -> "Wednesday";
    case 4 -> "Thursday";
    case 5 -> "Friday";
    case 6 -> "Saturday";
    case 7 -> "Sunday";
    default -> "Invalid day";
};
System.out.println(dayName);
```

## Loops

Loops execute a block of code repeatedly as long as a specified condition is true.

### 1. while Loop

The `while` loop executes a block of code as long as a condition is true.

```java
while (condition) {
    // code to execute while condition is true
}
```

Example:
```java
int i = 1;
while (i <= 5) {
    System.out.println(i);
    i++;
}
```

### 2. do-while Loop

The `do-while` loop executes a block of code once, then checks a condition to determine whether to execute it again.

```java
do {
    // code to execute
} while (condition);
```

Example:
```java
int i = 1;
do {
    System.out.println(i);
    i++;
} while (i <= 5);
```

### 3. for Loop

The `for` loop provides a concise way to iterate over a range of values.

```java
for (initialization; condition; update) {
    // code to execute while condition is true
}
```

Example:
```java
for (int i = 1; i <= 5; i++) {
    System.out.println(i);
}
```

### 4. Enhanced for Loop (for-each)

The enhanced `for` loop simplifies iterating over arrays and collections.

```java
for (type variable : array/collection) {
    // code to execute for each element
}
```

Example:
```java
int[] numbers = {1, 2, 3, 4, 5};
for (int num : numbers) {
    System.out.println(num);
}
```

## Control Flow Statements

These statements alter the normal flow of control in loops and switch statements.

### 1. break Statement

The `break` statement terminates the innermost loop or switch statement.

Example in a loop:
```java
for (int i = 1; i <= 10; i++) {
    if (i == 6) {
        break; // exits the loop when i equals 6
    }
    System.out.println(i);
}
// Output: 1 2 3 4 5
```

### 2. continue Statement

The `continue` statement skips the current iteration and continues with the next iteration of the loop.

Example:
```java
for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) {
        continue; // skips even numbers
    }
    System.out.println(i);
}
// Output: 1 3 5 7 9
```

### 3. return Statement

The `return` statement exits the current method and optionally returns a value.

Example:
```java
public int findMax(int a, int b) {
    if (a > b) {
        return a; // returns a and exits the method
    } else {
        return b; // returns b and exits the method
    }
}
```

## Practice Examples

### Example 1: Calculate factorial using a for loop
```java
public class Factorial {
    public static void main(String[] args) {
        int num = 5;
        long factorial = 1;
        
        for (int i = 1; i <= num; i++) {
            factorial *= i;
        }
        
        System.out.println("Factorial of " + num + " is " + factorial);
    }
}
```

### Example 2: Print the Fibonacci series using a while loop
```java
public class Fibonacci {
    public static void main(String[] args) {
        int n = 10;
        int first = 0, second = 1;
        
        System.out.print("Fibonacci Series of " + n + " numbers: ");
        
        int i = 1;
        while (i <= n) {
            System.out.print(first + " ");
            
            int next = first + second;
            first = second;
            second = next;
            
            i++;
        }
    }
}
```

### Example 3: Check if a number is prime using if-else
```java
public class PrimeCheck {
    public static void main(String[] args) {
        int num = 29;
        boolean isPrime = true;
        
        if (num <= 1) {
            isPrime = false;
        } else {
            for (int i = 2; i <= Math.sqrt(num); i++) {
                if (num % i == 0) {
                    isPrime = false;
                    break;
                }
            }
        }
        
        if (isPrime) {
            System.out.println(num + " is a prime number");
        } else {
            System.out.println(num + " is not a prime number");
        }
    }
}
```

## Practice Exercises

1. Write a program to find the largest of three numbers using if-else statements
2. Write a program to check if a year is a leap year
3. Write a program to display the multiplication table of a given number
4. Write a program to count the number of digits in a given number
5. Write a program to check if a number is a palindrome

## Next Steps

Continue to [Methods and Functions](/basics/04-methods.md) to learn how to organize your code into reusable blocks.