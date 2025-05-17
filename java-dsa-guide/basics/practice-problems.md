# Java Basics Practice Problems

This file contains practice problems to reinforce your understanding of Java basics. Each section corresponds to a fundamental concept covered in the basics module.

## Variables and Data Types

### Problem 1: Temperature Converter
Write a program that converts temperature from Celsius to Fahrenheit using the formula: F = C × 9/5 + 32

```java
public class TemperatureConverter {
    public static void main(String[] args) {
        double celsius = 25.0;
        double fahrenheit = (celsius * 9/5) + 32;
        
        System.out.println(celsius + " degrees Celsius is equal to " + fahrenheit + " degrees Fahrenheit.");
    }
}
```

### Problem 2: Circle Calculator
Write a program that calculates the area and circumference of a circle given its radius.

```java
public class CircleCalculator {
    public static void main(String[] args) {
        double radius = 5.0;
        double area = Math.PI * radius * radius;
        double circumference = 2 * Math.PI * radius;
        
        System.out.println("Circle with radius " + radius);
        System.out.println("Area: " + area);
        System.out.println("Circumference: " + circumference);
    }
}
```

### Problem 3: Variable Swap
Write a program that swaps the values of two variables without using a third variable.

```java
public class VariableSwap {
    public static void main(String[] args) {
        int a = 5;
        int b = 10;
        
        System.out.println("Before swap: a = " + a + ", b = " + b);
        
        // Swap without using a third variable
        a = a + b; // a now holds sum
        b = a - b; // b now holds original value of a
        a = a - b; // a now holds original value of b
        
        System.out.println("After swap: a = " + a + ", b = " + b);
    }
}
```

### Problem 4: Primitive Type Ranges
Write a program that prints the minimum and maximum values for each primitive numeric data type in Java.

```java
public class PrimitiveRanges {
    public static void main(String[] args) {
        System.out.println("byte range: " + Byte.MIN_VALUE + " to " + Byte.MAX_VALUE);
        System.out.println("short range: " + Short.MIN_VALUE + " to " + Short.MAX_VALUE);
        System.out.println("int range: " + Integer.MIN_VALUE + " to " + Integer.MAX_VALUE);
        System.out.println("long range: " + Long.MIN_VALUE + " to " + Long.MAX_VALUE);
        System.out.println("float range: " + Float.MIN_VALUE + " to " + Float.MAX_VALUE);
        System.out.println("double range: " + Double.MIN_VALUE + " to " + Double.MAX_VALUE);
    }
}
```

## Control Flow

### Problem 1: FizzBuzz
Write a program that prints numbers from 1 to 100. For multiples of 3, print "Fizz" instead of the number. For multiples of 5, print "Buzz". For multiples of both 3 and 5, print "FizzBuzz".

```java
public class FizzBuzz {
    public static void main(String[] args) {
        for (int i = 1; i <= 100; i++) {
            if (i % 3 == 0 && i % 5 == 0) {
                System.out.println("FizzBuzz");
            } else if (i % 3 == 0) {
                System.out.println("Fizz");
            } else if (i % 5 == 0) {
                System.out.println("Buzz");
            } else {
                System.out.println(i);
            }
        }
    }
}
```

### Problem 2: Leap Year Checker
Write a program that determines whether a given year is a leap year.

```java
public class LeapYearChecker {
    public static void main(String[] args) {
        int year = 2024;
        
        boolean isLeapYear = false;
        
        if (year % 4 == 0) {
            if (year % 100 == 0) {
                if (year % 400 == 0) {
                    isLeapYear = true;
                }
            } else {
                isLeapYear = true;
            }
        }
        
        if (isLeapYear) {
            System.out.println(year + " is a leap year.");
        } else {
            System.out.println(year + " is not a leap year.");
        }
    }
}
```

### Problem 3: Grade Calculator
Write a program that calculates the grade based on a student's score.

```java
public class GradeCalculator {
    public static void main(String[] args) {
        int score = 85;
        char grade;
        
        if (score >= 90) {
            grade = 'A';
        } else if (score >= 80) {
            grade = 'B';
        } else if (score >= 70) {
            grade = 'C';
        } else if (score >= 60) {
            grade = 'D';
        } else {
            grade = 'F';
        }
        
        System.out.println("Score: " + score);
        System.out.println("Grade: " + grade);
    }
}
```

### Problem 4: Prime Number Checker
Write a program that checks if a number is prime.

```java
public class PrimeChecker {
    public static void main(String[] args) {
        int number = 29;
        boolean isPrime = true;
        
        if (number <= 1) {
            isPrime = false;
        } else {
            for (int i = 2; i <= Math.sqrt(number); i++) {
                if (number % i == 0) {
                    isPrime = false;
                    break;
                }
            }
        }
        
        if (isPrime) {
            System.out.println(number + " is a prime number.");
        } else {
            System.out.println(number + " is not a prime number.");
        }
    }
}
```

### Problem 5: Multiplication Table
Write a program that prints the multiplication table for a given number.

```java
public class MultiplicationTable {
    public static void main(String[] args) {
        int number = 7;
        
        System.out.println("Multiplication table for " + number + ":");
        for (int i = 1; i <= 10; i++) {
            System.out.println(number + " × " + i + " = " + (number * i));
        }
    }
}
```

## Methods

### Problem 1: Calculator
Create a calculator class with methods for basic arithmetic operations.

```java
public class Calculator {
    public static int add(int a, int b) {
        return a + b;
    }
    
    public static int subtract(int a, int b) {
        return a - b;
    }
    
    public static int multiply(int a, int b) {
        return a * b;
    }
    
    public static double divide(int a, int b) {
        if (b == 0) {
            throw new ArithmeticException("Cannot divide by zero");
        }
        return (double) a / b;
    }
    
    public static void main(String[] args) {
        int a = 10;
        int b = 5;
        
        System.out.println(a + " + " + b + " = " + add(a, b));
        System.out.println(a + " - " + b + " = " + subtract(a, b));
        System.out.println(a + " × " + b + " = " + multiply(a, b));
        System.out.println(a + " ÷ " + b + " = " + divide(a, b));
    }
}
```

### Problem 2: Palindrome Checker
Write a method that checks if a string is a palindrome.

```java
public class PalindromeChecker {
    public static boolean isPalindrome(String str) {
        // Remove non-alphanumeric characters and convert to lowercase
        str = str.replaceAll("[^a-zA-Z0-9]", "").toLowerCase();
        
        int left = 0;
        int right = str.length() - 1;
        
        while (left < right) {
            if (str.charAt(left) != str.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        
        return true;
    }
    
    public static void main(String[] args) {
        String text1 = "A man, a plan, a canal: Panama";
        String text2 = "race a car";
        
        System.out.println("Is \"" + text1 + "\" a palindrome? " + isPalindrome(text1));
        System.out.println("Is \"" + text2 + "\" a palindrome? " + isPalindrome(text2));
    }
}
```

### Problem 3: Power Calculator
Write a recursive method that calculates the power of a number.

```java
public class PowerCalculator {
    public static double power(double base, int exponent) {
        // Handle negative exponent
        if (exponent < 0) {
            return 1 / power(base, -exponent);
        }
        
        // Base cases
        if (exponent == 0) {
            return 1;
        }
        if (exponent == 1) {
            return base;
        }
        
        // Recursive case
        // If exponent is even, calculate power(base, exponent/2)²
        // If exponent is odd, calculate base × power(base, exponent-1)
        if (exponent % 2 == 0) {
            double half = power(base, exponent / 2);
            return half * half;
        } else {
            return base * power(base, exponent - 1);
        }
    }
    
    public static void main(String[] args) {
        double base = 2.0;
        int exponent = 10;
        
        System.out.println(base + " raised to the power " + exponent + " = " + power(base, exponent));
        System.out.println("Using Math.pow: " + Math.pow(base, exponent));
    }
}
```

### Problem 4: Greatest Common Divisor
Write a method that finds the greatest common divisor (GCD) of two numbers using Euclid's algorithm.

```java
public class GCDCalculator {
    public static int gcd(int a, int b) {
        if (b == 0) {
            return a;
        }
        return gcd(b, a % b);
    }
    
    public static void main(String[] args) {
        int a = 48;
        int b = 18;
        
        System.out.println("GCD of " + a + " and " + b + " is " + gcd(a, b));
    }
}
```

### Problem 5: Number Digit Sum
Write a recursive method that calculates the sum of digits of a number.

```java
public class DigitSum {
    public static int sumOfDigits(int n) {
        // Handle negative numbers
        if (n < 0) {
            return sumOfDigits(-n);
        }
        
        // Base case
        if (n < 10) {
            return n;
        }
        
        // Recursive case: last digit + sum of remaining digits
        return n % 10 + sumOfDigits(n / 10);
    }
    
    public static void main(String[] args) {
        int number = 12345;
        
        System.out.println("Sum of digits in " + number + " = " + sumOfDigits(number));
    }
}
```

## Arrays

### Problem 1: Array Reverse
Write a method that reverses an array in place.

```java
import java.util.Arrays;

public class ArrayReverser {
    public static void reverse(int[] array) {
        int left = 0;
        int right = array.length - 1;
        
        while (left < right) {
            // Swap elements at left and right
            int temp = array[left];
            array[left] = array[right];
            array[right] = temp;
            
            // Move towards center
            left++;
            right--;
        }
    }
    
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3, 4, 5};
        
        System.out.println("Original array: " + Arrays.toString(numbers));
        reverse(numbers);
        System.out.println("Reversed array: " + Arrays.toString(numbers));
    }
}
```

### Problem 2: Find Second Largest
Write a method that finds the second largest element in an array.

```java
public class SecondLargest {
    public static int findSecondLargest(int[] array) {
        if (array.length < 2) {
            throw new IllegalArgumentException("Array should have at least two elements");
        }
        
        int largest = Integer.MIN_VALUE;
        int secondLargest = Integer.MIN_VALUE;
        
        for (int num : array) {
            if (num > largest) {
                secondLargest = largest;
                largest = num;
            } else if (num > secondLargest && num != largest) {
                secondLargest = num;
            }
        }
        
        if (secondLargest == Integer.MIN_VALUE) {
            throw new IllegalArgumentException("No second largest element found");
        }
        
        return secondLargest;
    }
    
    public static void main(String[] args) {
        int[] numbers = {5, 12, 9, 7, 3, 12, 4};
        
        System.out.println("Array: " + java.util.Arrays.toString(numbers));
        System.out.println("Second largest element: " + findSecondLargest(numbers));
    }
}
```

### Problem 3: Rotate Array
Write a method that rotates an array to the right by k steps.

```java
import java.util.Arrays;

public class ArrayRotator {
    public static void rotate(int[] nums, int k) {
        int n = nums.length;
        k = k % n; // Handle case when k > n
        
        if (k == 0) {
            return; // No rotation needed
        }
        
        // Reverse the entire array
        reverse(nums, 0, n - 1);
        // Reverse the first k elements
        reverse(nums, 0, k - 1);
        // Reverse the remaining n-k elements
        reverse(nums, k, n - 1);
    }
    
    private static void reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
    
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3, 4, 5, 6, 7};
        int k = 3;
        
        System.out.println("Original array: " + Arrays.toString(numbers));
        rotate(numbers, k);
        System.out.println("After rotating right by " + k + " steps: " + Arrays.toString(numbers));
    }
}
```

### Problem 4: Merge Sorted Arrays
Write a method that merges two sorted arrays into a single sorted array.

```java
import java.util.Arrays;

public class ArrayMerger {
    public static int[] mergeSortedArrays(int[] arr1, int[] arr2) {
        int[] result = new int[arr1.length + arr2.length];
        
        int i = 0, j = 0, k = 0;
        
        // Compare elements from both arrays and add the smaller one to result
        while (i < arr1.length && j < arr2.length) {
            if (arr1[i] <= arr2[j]) {
                result[k++] = arr1[i++];
            } else {
                result[k++] = arr2[j++];
            }
        }
        
        // Add remaining elements from arr1, if any
        while (i < arr1.length) {
            result[k++] = arr1[i++];
        }
        
        // Add remaining elements from arr2, if any
        while (j < arr2.length) {
            result[k++] = arr2[j++];
        }
        
        return result;
    }
    
    public static void main(String[] args) {
        int[] arr1 = {1, 3, 5, 7, 9};
        int[] arr2 = {2, 4, 6, 8, 10};
        
        System.out.println("Array 1: " + Arrays.toString(arr1));
        System.out.println("Array 2: " + Arrays.toString(arr2));
        
        int[] merged = mergeSortedArrays(arr1, arr2);
        System.out.println("Merged array: " + Arrays.toString(merged));
    }
}
```

### Problem 5: Find Pairs with Given Sum
Write a method that finds all pairs of elements in an array whose sum is equal to a given number.

```java
import java.util.HashSet;
import java.util.Set;

public class PairFinder {
    public static void findPairsWithSum(int[] array, int targetSum) {
        Set<Integer> seen = new HashSet<>();
        boolean foundPair = false;
        
        for (int num : array) {
            int complement = targetSum - num;
            
            if (seen.contains(complement)) {
                System.out.println("Pair found: (" + complement + ", " + num + ")");
                foundPair = true;
            }
            
            seen.add(num);
        }
        
        if (!foundPair) {
            System.out.println("No pairs found with sum " + targetSum);
        }
    }
    
    public static void main(String[] args) {
        int[] numbers = {2, 4, 3, 5, 6, -2, 8, 7, 9};
        int targetSum = 7;
        
        System.out.println("Array: " + java.util.Arrays.toString(numbers));
        System.out.println("Target sum: " + targetSum);
        findPairsWithSum(numbers, targetSum);
    }
}
```

## Combined Problems

### Problem 1: Bank Account
Create a simple bank account class with methods for deposit, withdrawal, and checking balance.

```java
public class BankAccount {
    private String accountNumber;
    private String ownerName;
    private double balance;
    
    public BankAccount(String accountNumber, String ownerName, double initialBalance) {
        this.accountNumber = accountNumber;
        this.ownerName = ownerName;
        this.balance = initialBalance;
    }
    
    public void deposit(double amount) {
        if (amount <= 0) {
            System.out.println("Deposit amount must be positive");
            return;
        }
        
        balance += amount;
        System.out.println("Deposited: $" + amount);
        System.out.println("New balance: $" + balance);
    }
    
    public void withdraw(double amount) {
        if (amount <= 0) {
            System.out.println("Withdrawal amount must be positive");
            return;
        }
        
        if (amount > balance) {
            System.out.println("Insufficient funds");
            return;
        }
        
        balance -= amount;
        System.out.println("Withdrawn: $" + amount);
        System.out.println("New balance: $" + balance);
    }
    
    public double getBalance() {
        return balance;
    }
    
    public void displayAccountInfo() {
        System.out.println("Account Information:");
        System.out.println("Account Number: " + accountNumber);
        System.out.println("Owner: " + ownerName);
        System.out.println("Balance: $" + balance);
    }
    
    public static void main(String[] args) {
        BankAccount account = new BankAccount("123456789", "John Doe", 1000.0);
        
        account.displayAccountInfo();
        
        account.deposit(500.0);
        account.withdraw(200.0);
        account.withdraw(2000.0); // Should show insufficient funds message
        
        account.displayAccountInfo();
    }
}
```

### Problem 2: Student Grade System
Create a student grade system that calculates average grades and determines the final grade.

```java
import java.util.Arrays;

public class StudentGradeSystem {
    private String studentName;
    private int[] grades;
    
    public StudentGradeSystem(String studentName, int[] grades) {
        this.studentName = studentName;
        this.grades = grades;
    }
    
    public double calculateAverage() {
        if (grades.length == 0) {
            return 0.0;
        }
        
        int sum = 0;
        for (int grade : grades) {
            sum += grade;
        }
        
        return (double) sum / grades.length;
    }
    
    public char determineGrade(double average) {
        if (average >= 90) {
            return 'A';
        } else if (average >= 80) {
            return 'B';
        } else if (average >= 70) {
            return 'C';
        } else if (average >= 60) {
            return 'D';
        } else {
            return 'F';
        }
    }
    
    public int getHighestGrade() {
        if (grades.length == 0) {
            throw new IllegalStateException("No grades available");
        }
        
        int highest = grades[0];
        for (int grade : grades) {
            if (grade > highest) {
                highest = grade;
            }
        }
        
        return highest;
    }
    
    public int getLowestGrade() {
        if (grades.length == 0) {
            throw new IllegalStateException("No grades available");
        }
        
        int lowest = grades[0];
        for (int grade : grades) {
            if (grade < lowest) {
                lowest = grade;
            }
        }
        
        return lowest;
    }
    
    public void displayStudentInfo() {
        System.out.println("Student: " + studentName);
        System.out.println("Grades: " + Arrays.toString(grades));
        
        double average = calculateAverage();
        char finalGrade = determineGrade(average);
        
        System.out.println("Average: " + average);
        System.out.println("Highest Grade: " + getHighestGrade());
        System.out.println("Lowest Grade: " + getLowestGrade());
        System.out.println("Final Grade: " + finalGrade);
    }
    
    public static void main(String[] args) {
        int[] grades = {85, 90, 78, 92, 88};
        StudentGradeSystem student = new StudentGradeSystem("Jane Smith", grades);
        
        student.displayStudentInfo();
    }
}
```

### Problem 3: Simple Library System
Create a simple library system with books and methods for borrowing and returning books.

```java
import java.util.ArrayList;
import java.util.List;

public class LibrarySystem {
    private List<Book> books;
    
    public LibrarySystem() {
        books = new ArrayList<>();
    }
    
    public void addBook(Book book) {
        books.add(book);
        System.out.println("Book added: " + book.getTitle());
    }
    
    public void borrowBook(String title) {
        for (Book book : books) {
            if (book.getTitle().equalsIgnoreCase(title)) {
                if (book.isAvailable()) {
                    book.setAvailable(false);
                    System.out.println("Book borrowed: " + book.getTitle());
                    return;
                } else {
                    System.out.println("Book is already borrowed: " + book.getTitle());
                    return;
                }
            }
        }
        
        System.out.println("Book not found: " + title);
    }
    
    public void returnBook(String title) {
        for (Book book : books) {
            if (book.getTitle().equalsIgnoreCase(title)) {
                if (!book.isAvailable()) {
                    book.setAvailable(true);
                    System.out.println("Book returned: " + book.getTitle());
                    return;
                } else {
                    System.out.println("Book was not borrowed: " + book.getTitle());
                    return;
                }
            }
        }
        
        System.out.println("Book not found: " + title);
    }
    
    public void displayAllBooks() {
        if (books.isEmpty()) {
            System.out.println("No books in the library.");
            return;
        }
        
        System.out.println("Library Books:");
        for (Book book : books) {
            System.out.println(book);
        }
    }
    
    public void displayAvailableBooks() {
        if (books.isEmpty()) {
            System.out.println("No books in the library.");
            return;
        }
        
        System.out.println("Available Books:");
        boolean found = false;
        
        for (Book book : books) {
            if (book.isAvailable()) {
                System.out.println(book);
                found = true;
            }
        }
        
        if (!found) {
            System.out.println("No available books.");
        }
    }
    
    public static void main(String[] args) {
        LibrarySystem library = new LibrarySystem();
        
        // Add books
        library.addBook(new Book("The Hobbit", "J.R.R. Tolkien", "Fantasy"));
        library.addBook(new Book("To Kill a Mockingbird", "Harper Lee", "Fiction"));
        library.addBook(new Book("1984", "George Orwell", "Dystopian"));
        
        // Display all books
        library.displayAllBooks();
        
        // Borrow books
        library.borrowBook("The Hobbit");
        library.borrowBook("1984");
        
        // Display available books
        library.displayAvailableBooks();
        
        // Return a book
        library.returnBook("The Hobbit");
        
        // Display available books again
        library.displayAvailableBooks();
    }
}

class Book {
    private String title;
    private String author;
    private String genre;
    private boolean available;
    
    public Book(String title, String author, String genre) {
        this.title = title;
        this.author = author;
        this.genre = genre;
        this.available = true;
    }
    
    public String getTitle() {
        return title;
    }
    
    public String getAuthor() {
        return author;
    }
    
    public String getGenre() {
        return genre;
    }
    
    public boolean isAvailable() {
        return available;
    }
    
    public void setAvailable(boolean available) {
        this.available = available;
    }
    
    @Override
    public String toString() {
        return title + " by " + author + " (" + genre + ") - " + 
               (available ? "Available" : "Borrowed");
    }
}
```

## Next Steps

Now that you've practiced these basic Java concepts, you're ready to move on to more complex topics:

1. Continue to [Object-Oriented Programming](/oop/01-classes-objects.md) to learn how to structure your code using classes and objects.
2. Explore [Java Collections](/collections/README.md) to work with more advanced data structures.
3. Learn about [Exception Handling](/exceptions/README.md) to make your programs more robust.