# Classes and Objects in Java

Object-Oriented Programming (OOP) is a programming paradigm that uses "objects" to design applications. Java is an object-oriented language, and understanding classes and objects is fundamental to mastering Java.

## What is a Class?

A class is a blueprint or template for creating objects. It defines:
- **Attributes** (fields/variables): Data that describes the object
- **Methods**: Actions/behaviors that the object can perform
- **Constructors**: Special methods used to initialize objects
- **Access control**: Visibility and accessibility of class members

## Defining a Class

The basic syntax for defining a class in Java:

```java
accessModifier class ClassName {
    // Fields (attributes)
    accessModifier dataType fieldName;
    
    // Constructor
    accessModifier ClassName(parameters) {
        // Initialization code
    }
    
    // Methods
    accessModifier returnType methodName(parameters) {
        // Method body
    }
}
```

Example of a simple class:

```java
public class Car {
    // Fields
    private String make;
    private String model;
    private int year;
    private double price;
    
    // Constructor
    public Car(String make, String model, int year, double price) {
        this.make = make;
        this.model = model;
        this.year = year;
        this.price = price;
    }
    
    // Methods
    public void startEngine() {
        System.out.println("The " + make + " " + model + " engine is starting...");
    }
    
    public void stopEngine() {
        System.out.println("The " + make + " " + model + " engine is stopping...");
    }
    
    // Getter methods
    public String getMake() {
        return make;
    }
    
    public String getModel() {
        return model;
    }
    
    public int getYear() {
        return year;
    }
    
    public double getPrice() {
        return price;
    }
    
    // Setter methods
    public void setPrice(double price) {
        this.price = price;
    }
}
```

## What is an Object?

An object is an instance of a class. It's a real-world entity that has:
- **State**: The values of its attributes/fields at a given time
- **Behavior**: The actions it can perform (methods)
- **Identity**: A unique identification that distinguishes it from other objects

## Creating Objects

Objects are created using the `new` keyword followed by a call to a constructor:

```java
ClassName objectName = new ClassName(arguments);
```

Example:

```java
Car myCar = new Car("Toyota", "Camry", 2022, 25000.0);
```

## Accessing Object Members

You can access an object's fields and methods using the dot (.) operator:

```java
objectName.fieldName;      // Access a field
objectName.methodName();   // Call a method
```

Example:

```java
System.out.println("My car is a " + myCar.getMake() + " " + myCar.getModel());
myCar.startEngine();
```

## Constructors

Constructors are special methods used to initialize objects. They have the same name as the class and no return type.

### Default Constructor

If you don't define any constructor, Java provides a default constructor that initializes fields to their default values.

```java
public class Person {
    private String name;
    private int age;
    
    // Default constructor (implicitly provided if no constructors are defined)
    public Person() {
        // Fields are initialized to default values
        // name = null, age = 0
    }
}
```

### Parameterized Constructor

Constructors can take parameters to initialize fields with specific values.

```java
public class Person {
    private String name;
    private int age;
    
    // Parameterized constructor
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

### Constructor Overloading

You can define multiple constructors with different parameter lists.

```java
public class Person {
    private String name;
    private int age;
    
    // Default constructor
    public Person() {
        this.name = "Unknown";
        this.age = 0;
    }
    
    // Constructor with name parameter
    public Person(String name) {
        this.name = name;
        this.age = 0;
    }
    
    // Constructor with both parameters
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

## The `this` Keyword

The `this` keyword refers to the current object and is used to:
- Differentiate between instance variables and parameters with the same name
- Call another constructor from within a constructor
- Return the current object from a method

```java
public class Rectangle {
    private double length;
    private double width;
    
    // Constructor
    public Rectangle(double length, double width) {
        // Using 'this' to refer to instance variables
        this.length = length;
        this.width = width;
    }
    
    // No-argument constructor calling the parameterized constructor
    public Rectangle() {
        this(1.0, 1.0); // Calls Rectangle(double, double)
    }
    
    // Method returning the current object
    public Rectangle setLength(double length) {
        this.length = length;
        return this; // Returns the current object for method chaining
    }
    
    public Rectangle setWidth(double width) {
        this.width = width;
        return this;
    }
    
    public double calculateArea() {
        return length * width;
    }
}
```

Usage of method chaining:

```java
Rectangle rect = new Rectangle();
double area = rect.setLength(5.0).setWidth(3.0).calculateArea(); // Returns 15.0
```

## Access Modifiers

Access modifiers control the visibility and accessibility of classes, fields, methods, and constructors.

| Modifier | Class | Package | Subclass | World |
|----------|-------|---------|----------|-------|
| `public` | Yes | Yes | Yes | Yes |
| `protected` | Yes | Yes | Yes | No |
| `default` (no modifier) | Yes | Yes | No | No |
| `private` | Yes | No | No | No |

### Usage Guidelines:

- **Fields**: Usually `private` to enforce encapsulation
- **Methods**: Often `public` for external access, `private` for internal helpers
- **Constructors**: Usually `public`, but can be `private` for singleton pattern
- **Classes**: Usually `public`, but can be `default` for internal use

## Instance Members vs Static Members

### Instance Members

Instance members belong to a specific instance (object) of a class:
- Each object has its own copy of instance variables
- Instance methods can access instance variables and other instance methods

```java
public class Counter {
    private int count; // Instance variable
    
    public void increment() { // Instance method
        count++;
    }
    
    public int getCount() { // Instance method
        return count;
    }
}
```

Usage:

```java
Counter c1 = new Counter();
Counter c2 = new Counter();

c1.increment(); // c1.count = 1, c2.count = 0
c1.increment(); // c1.count = 2, c2.count = 0
c2.increment(); // c1.count = 2, c2.count = 1
```

### Static Members

Static members belong to the class rather than any instance:
- All objects share the same static variables
- Static methods can only access static variables and call static methods
- Static methods can be called without creating an object

```java
public class MathUtils {
    public static final double PI = 3.14159; // Static variable
    
    public static int abs(int value) { // Static method
        return (value < 0) ? -value : value;
    }
    
    public static double square(double value) { // Static method
        return value * value;
    }
}
```

Usage:

```java
double pi = MathUtils.PI;
int absolute = MathUtils.abs(-5); // Returns 5
double squared = MathUtils.square(4.0); // Returns 16.0
```

## Class initialization blocks

Java provides two types of initialization blocks:

### Instance Initialization Block

Executed when an instance of a class is created, before constructor execution.

```java
public class Example {
    private int x;
    
    // Instance initialization block
    {
        x = 10;
        System.out.println("Instance initialization block executed");
    }
    
    public Example() {
        System.out.println("Constructor executed");
    }
}
```

### Static Initialization Block

Executed when the class is loaded, before any static methods are called or static variables are accessed.

```java
public class Database {
    private static Connection connection;
    
    // Static initialization block
    static {
        try {
            connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "user", "password");
            System.out.println("Database connection established");
        } catch (SQLException e) {
            System.err.println("Failed to connect to database");
        }
    }
}
```

## Object Lifecycle

1. **Creation**:
   - Memory is allocated
   - Instance variables are initialized to default values
   - Instance initialization blocks are executed
   - Constructor is called

2. **Usage**:
   - Object's methods are called
   - Object's state changes through method calls

3. **Destruction**:
   - Object becomes unreachable (no references point to it)
   - Garbage collector eventually frees the memory
   - `finalize()` method is called (deprecated in newer Java versions)

## Complete Example: Student Management System

```java
public class Student {
    // Private fields
    private String id;
    private String name;
    private int age;
    private double[] grades;
    private static int studentCount = 0; // Static counter
    
    // Static initialization block
    static {
        System.out.println("Student class is loaded");
    }
    
    // Instance initialization block
    {
        studentCount++;
        System.out.println("Creating student #" + studentCount);
    }
    
    // Constructors
    public Student() {
        this("Unknown", 0);
    }
    
    public Student(String name, int age) {
        this(generateId(), name, age, new double[0]);
    }
    
    public Student(String id, String name, int age, double[] grades) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.grades = grades;
    }
    
    // Static method to generate ID
    private static String generateId() {
        return "S" + String.format("%04d", studentCount);
    }
    
    // Instance methods
    public void addGrade(double grade) {
        double[] newGrades = new double[grades.length + 1];
        System.arraycopy(grades, 0, newGrades, 0, grades.length);
        newGrades[grades.length] = grade;
        grades = newGrades;
    }
    
    public double calculateAverage() {
        if (grades.length == 0) {
            return 0.0;
        }
        
        double sum = 0.0;
        for (double grade : grades) {
            sum += grade;
        }
        return sum / grades.length;
    }
    
    // Getters and setters
    public String getId() {
        return id;
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        this.age = age;
    }
    
    public double[] getGrades() {
        return grades.clone(); // Return a copy to protect the internal state
    }
    
    // Static method to get total student count
    public static int getStudentCount() {
        return studentCount;
    }
    
    // Override toString method for better representation
    @Override
    public String toString() {
        return "Student{id='" + id + "', name='" + name + "', age=" + age + 
               ", average=" + calculateAverage() + "}";
    }
}
```

Usage:

```java
public class Main {
    public static void main(String[] args) {
        // Create students
        Student student1 = new Student("John Doe", 20);
        student1.addGrade(85.5);
        student1.addGrade(90.0);
        student1.addGrade(78.5);
        
        Student student2 = new Student("Jane Smith", 22);
        student2.addGrade(92.0);
        student2.addGrade(88.5);
        
        // Display student information
        System.out.println(student1);
        System.out.println(student2);
        
        // Get student count
        System.out.println("Total students: " + Student.getStudentCount());
    }
}
```

## Practice Exercises

1. Create a `BankAccount` class with fields for account number, owner name, and balance. Include methods for deposit, withdraw, and checking balance.

2. Create a `Book` class with fields for title, author, and price. Include constructor, getters, setters, and a method to apply discount.

3. Create a `Circle` class with a radius field. Include methods to calculate area and circumference, and a static method to calculate distance between two circles.

4. Create a `Person` class and a `Student` class that inherits from `Person`. Add appropriate fields and methods to both classes.

5. Implement a `Calculator` class with static methods for basic arithmetic operations.

6. Create a `Product` class for an e-commerce system with fields for ID, name, price, and quantity. Include methods for calculating total value and applying discounts.

7. Design a `ShoppingCart` class that can hold multiple `Product` objects and calculate the total price.

## Next Steps

Continue to [Inheritance](/oop/02-inheritance.md) to learn how to create class hierarchies in Java.