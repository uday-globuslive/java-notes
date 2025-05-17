# Object-Oriented Programming Practice Problems

This file contains practice problems to reinforce your understanding of object-oriented programming concepts in Java. Each problem focuses on different aspects of OOP including classes, objects, inheritance, polymorphism, abstraction, and encapsulation.

## Classes and Objects

### Problem 1: Rectangle Class

Create a `Rectangle` class that represents a rectangle with methods to calculate its area and perimeter.

```java
public class Rectangle {
    private double length;
    private double width;
    
    // Constructor
    public Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }
    
    // Calculate area
    public double calculateArea() {
        return length * width;
    }
    
    // Calculate perimeter
    public double calculatePerimeter() {
        return 2 * (length + width);
    }
    
    // Getters and setters
    public double getLength() {
        return length;
    }
    
    public void setLength(double length) {
        this.length = length;
    }
    
    public double getWidth() {
        return width;
    }
    
    public void setWidth(double width) {
        this.width = width;
    }
    
    // Override toString method
    @Override
    public String toString() {
        return "Rectangle [length=" + length + ", width=" + width + "]";
    }
    
    // Main method for testing
    public static void main(String[] args) {
        Rectangle rectangle = new Rectangle(5.0, 3.0);
        
        System.out.println(rectangle);
        System.out.println("Area: " + rectangle.calculateArea());
        System.out.println("Perimeter: " + rectangle.calculatePerimeter());
        
        // Update dimensions
        rectangle.setLength(7.0);
        rectangle.setWidth(4.0);
        
        System.out.println("\nAfter updating dimensions:");
        System.out.println(rectangle);
        System.out.println("Area: " + rectangle.calculateArea());
        System.out.println("Perimeter: " + rectangle.calculatePerimeter());
    }
}
```

### Problem 2: Bank Account

Create a `BankAccount` class that simulates a bank account with methods for deposit, withdrawal, and checking balance.

```java
public class BankAccount {
    private String accountNumber;
    private String accountHolder;
    private double balance;
    private static int nextAccountNumber = 1000;
    
    // Constructor with account holder name
    public BankAccount(String accountHolder) {
        this.accountNumber = "ACC" + nextAccountNumber++;
        this.accountHolder = accountHolder;
        this.balance = 0.0;
    }
    
    // Constructor with account holder name and initial balance
    public BankAccount(String accountHolder, double initialBalance) {
        this(accountHolder);
        if (initialBalance > 0) {
            this.balance = initialBalance;
        }
    }
    
    // Deposit money
    public void deposit(double amount) {
        if (amount <= 0) {
            System.out.println("Error: Deposit amount must be positive");
            return;
        }
        
        balance += amount;
        System.out.println("Deposited: $" + amount);
        System.out.println("Current Balance: $" + balance);
    }
    
    // Withdraw money
    public boolean withdraw(double amount) {
        if (amount <= 0) {
            System.out.println("Error: Withdrawal amount must be positive");
            return false;
        }
        
        if (amount > balance) {
            System.out.println("Error: Insufficient funds");
            return false;
        }
        
        balance -= amount;
        System.out.println("Withdrawn: $" + amount);
        System.out.println("Current Balance: $" + balance);
        return true;
    }
    
    // Get account information
    public void displayAccountInfo() {
        System.out.println("Account Number: " + accountNumber);
        System.out.println("Account Holder: " + accountHolder);
        System.out.println("Current Balance: $" + balance);
    }
    
    // Getters and setters
    public String getAccountNumber() {
        return accountNumber;
    }
    
    public String getAccountHolder() {
        return accountHolder;
    }
    
    public void setAccountHolder(String accountHolder) {
        this.accountHolder = accountHolder;
    }
    
    public double getBalance() {
        return balance;
    }
    
    // Main method for testing
    public static void main(String[] args) {
        // Create accounts
        BankAccount account1 = new BankAccount("John Doe");
        BankAccount account2 = new BankAccount("Jane Smith", 1000.0);
        
        // Display initial account information
        System.out.println("Account 1:");
        account1.displayAccountInfo();
        
        System.out.println("\nAccount 2:");
        account2.displayAccountInfo();
        
        // Perform transactions
        System.out.println("\nPerforming transactions on Account 1:");
        account1.deposit(500.0);
        account1.withdraw(200.0);
        account1.withdraw(400.0); // Should fail - insufficient funds
        
        System.out.println("\nPerforming transactions on Account 2:");
        account2.deposit(250.0);
        account2.withdraw(750.0);
        
        // Display final account information
        System.out.println("\nFinal Account 1 Information:");
        account1.displayAccountInfo();
        
        System.out.println("\nFinal Account 2 Information:");
        account2.displayAccountInfo();
    }
}
```

### Problem 3: Student Management System

Create a `Student` class and a `Course` class that represents a simple student management system.

```java
import java.util.ArrayList;
import java.util.List;

// Course class
class Course {
    private String courseCode;
    private String courseName;
    private int creditHours;
    
    public Course(String courseCode, String courseName, int creditHours) {
        this.courseCode = courseCode;
        this.courseName = courseName;
        this.creditHours = creditHours;
    }
    
    public String getCourseCode() {
        return courseCode;
    }
    
    public String getCourseName() {
        return courseName;
    }
    
    public int getCreditHours() {
        return creditHours;
    }
    
    @Override
    public String toString() {
        return courseCode + ": " + courseName + " (" + creditHours + " credits)";
    }
}

// Student class
public class Student {
    private String studentId;
    private String name;
    private List<Course> enrolledCourses;
    private List<Double> grades;
    private static int nextStudentId = 1001;
    
    public Student(String name) {
        this.studentId = "S" + nextStudentId++;
        this.name = name;
        this.enrolledCourses = new ArrayList<>();
        this.grades = new ArrayList<>();
    }
    
    public void enrollCourse(Course course) {
        enrolledCourses.add(course);
        grades.add(0.0); // Initialize grade to 0.0
        System.out.println(name + " enrolled in " + course.getCourseName());
    }
    
    public void setGrade(String courseCode, double grade) {
        for (int i = 0; i < enrolledCourses.size(); i++) {
            if (enrolledCourses.get(i).getCourseCode().equals(courseCode)) {
                grades.set(i, grade);
                System.out.println("Grade set for " + courseCode + ": " + grade);
                return;
            }
        }
        System.out.println("Course not found: " + courseCode);
    }
    
    public double calculateGPA() {
        if (enrolledCourses.isEmpty()) {
            return 0.0;
        }
        
        double totalPoints = 0.0;
        int totalCredits = 0;
        
        for (int i = 0; i < enrolledCourses.size(); i++) {
            Course course = enrolledCourses.get(i);
            double grade = grades.get(i);
            
            totalPoints += grade * course.getCreditHours();
            totalCredits += course.getCreditHours();
        }
        
        return totalPoints / totalCredits;
    }
    
    public void displayStudentInfo() {
        System.out.println("Student ID: " + studentId);
        System.out.println("Name: " + name);
        System.out.println("Enrolled Courses:");
        
        for (int i = 0; i < enrolledCourses.size(); i++) {
            System.out.println("  - " + enrolledCourses.get(i) + ", Grade: " + grades.get(i));
        }
        
        System.out.println("GPA: " + calculateGPA());
    }
    
    public String getStudentId() {
        return studentId;
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public List<Course> getEnrolledCourses() {
        return new ArrayList<>(enrolledCourses); // Return a copy to prevent external modification
    }
    
    public static void main(String[] args) {
        // Create courses
        Course course1 = new Course("CS101", "Introduction to Programming", 3);
        Course course2 = new Course("CS102", "Data Structures", 4);
        Course course3 = new Course("MATH101", "Calculus I", 4);
        
        // Create students
        Student student1 = new Student("John Doe");
        Student student2 = new Student("Jane Smith");
        
        // Enroll students in courses
        student1.enrollCourse(course1);
        student1.enrollCourse(course2);
        student1.enrollCourse(course3);
        
        student2.enrollCourse(course1);
        student2.enrollCourse(course3);
        
        // Set grades
        student1.setGrade("CS101", 3.7);
        student1.setGrade("CS102", 4.0);
        student1.setGrade("MATH101", 3.3);
        
        student2.setGrade("CS101", 3.5);
        student2.setGrade("MATH101", 3.8);
        
        // Display student information
        System.out.println("\nStudent 1 Information:");
        student1.displayStudentInfo();
        
        System.out.println("\nStudent 2 Information:");
        student2.displayStudentInfo();
    }
}
```

## Inheritance and Polymorphism

### Problem 1: Shape Hierarchy

Create a hierarchy of shape classes demonstrating inheritance and polymorphism.

```java
// Base class
abstract class Shape {
    protected String color;
    
    public Shape(String color) {
        this.color = color;
    }
    
    public String getColor() {
        return color;
    }
    
    public void setColor(String color) {
        this.color = color;
    }
    
    // Abstract methods
    public abstract double calculateArea();
    public abstract double calculatePerimeter();
    
    @Override
    public String toString() {
        return "Shape [color=" + color + "]";
    }
}

// Circle class
class Circle extends Shape {
    private double radius;
    
    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }
    
    public double getRadius() {
        return radius;
    }
    
    public void setRadius(double radius) {
        this.radius = radius;
    }
    
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public double calculatePerimeter() {
        return 2 * Math.PI * radius;
    }
    
    @Override
    public String toString() {
        return "Circle [color=" + color + ", radius=" + radius + "]";
    }
}

// Rectangle class
class Rectangle extends Shape {
    private double length;
    private double width;
    
    public Rectangle(String color, double length, double width) {
        super(color);
        this.length = length;
        this.width = width;
    }
    
    public double getLength() {
        return length;
    }
    
    public void setLength(double length) {
        this.length = length;
    }
    
    public double getWidth() {
        return width;
    }
    
    public void setWidth(double width) {
        this.width = width;
    }
    
    @Override
    public double calculateArea() {
        return length * width;
    }
    
    @Override
    public double calculatePerimeter() {
        return 2 * (length + width);
    }
    
    @Override
    public String toString() {
        return "Rectangle [color=" + color + ", length=" + length + ", width=" + width + "]";
    }
}

// Triangle class
class Triangle extends Shape {
    private double side1;
    private double side2;
    private double side3;
    
    public Triangle(String color, double side1, double side2, double side3) {
        super(color);
        this.side1 = side1;
        this.side2 = side2;
        this.side3 = side3;
    }
    
    @Override
    public double calculateArea() {
        // Using Heron's formula
        double s = (side1 + side2 + side3) / 2;
        return Math.sqrt(s * (s - side1) * (s - side2) * (s - side3));
    }
    
    @Override
    public double calculatePerimeter() {
        return side1 + side2 + side3;
    }
    
    @Override
    public String toString() {
        return "Triangle [color=" + color + ", sides=[" + side1 + ", " + side2 + ", " + side3 + "]]";
    }
}

// Main class for testing
public class ShapeTest {
    public static void main(String[] args) {
        // Create array of shapes
        Shape[] shapes = new Shape[3];
        shapes[0] = new Circle("Red", 5.0);
        shapes[1] = new Rectangle("Blue", 4.0, 6.0);
        shapes[2] = new Triangle("Green", 3.0, 4.0, 5.0);
        
        // Display information about each shape
        for (Shape shape : shapes) {
            System.out.println(shape);
            System.out.println("Area: " + shape.calculateArea());
            System.out.println("Perimeter: " + shape.calculatePerimeter());
            System.out.println();
        }
        
        // Demonstrate polymorphism
        System.out.println("Total area of all shapes: " + calculateTotalArea(shapes));
    }
    
    // Method demonstrating polymorphism
    public static double calculateTotalArea(Shape[] shapes) {
        double totalArea = 0.0;
        
        for (Shape shape : shapes) {
            totalArea += shape.calculateArea();
        }
        
        return totalArea;
    }
}
```

### Problem 2: Employee Hierarchy

Create a hierarchy of employee classes with different payment calculation methods.

```java
// Base Employee class
abstract class Employee {
    protected String name;
    protected String id;
    protected static int nextEmployeeId = 1001;
    
    public Employee(String name) {
        this.name = name;
        this.id = "EMP" + nextEmployeeId++;
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public String getId() {
        return id;
    }
    
    // Abstract method for calculating payment
    public abstract double calculatePayment();
    
    @Override
    public String toString() {
        return "Employee [id=" + id + ", name=" + name + "]";
    }
}

// Hourly Employee class
class HourlyEmployee extends Employee {
    private double hourlyRate;
    private double hoursWorked;
    
    public HourlyEmployee(String name, double hourlyRate) {
        super(name);
        this.hourlyRate = hourlyRate;
        this.hoursWorked = 0;
    }
    
    public void addHours(double hours) {
        if (hours > 0) {
            this.hoursWorked += hours;
        }
    }
    
    public void resetHours() {
        this.hoursWorked = 0;
    }
    
    public double getHourlyRate() {
        return hourlyRate;
    }
    
    public void setHourlyRate(double hourlyRate) {
        this.hourlyRate = hourlyRate;
    }
    
    public double getHoursWorked() {
        return hoursWorked;
    }
    
    @Override
    public double calculatePayment() {
        double regularPay = 0;
        double overtimePay = 0;
        
        if (hoursWorked <= 40) {
            regularPay = hoursWorked * hourlyRate;
        } else {
            regularPay = 40 * hourlyRate;
            overtimePay = (hoursWorked - 40) * (hourlyRate * 1.5);
        }
        
        return regularPay + overtimePay;
    }
    
    @Override
    public String toString() {
        return "HourlyEmployee [id=" + id + ", name=" + name + 
               ", hourlyRate=$" + hourlyRate + ", hoursWorked=" + hoursWorked + "]";
    }
}

// Salaried Employee class
class SalariedEmployee extends Employee {
    private double annualSalary;
    
    public SalariedEmployee(String name, double annualSalary) {
        super(name);
        this.annualSalary = annualSalary;
    }
    
    public double getAnnualSalary() {
        return annualSalary;
    }
    
    public void setAnnualSalary(double annualSalary) {
        this.annualSalary = annualSalary;
    }
    
    @Override
    public double calculatePayment() {
        return annualSalary / 26; // Bi-weekly payment (26 pay periods per year)
    }
    
    @Override
    public String toString() {
        return "SalariedEmployee [id=" + id + ", name=" + name + 
               ", annualSalary=$" + annualSalary + "]";
    }
}

// Commission Employee class
class CommissionEmployee extends Employee {
    private double baseSalary;
    private double commissionRate;
    private double salesAmount;
    
    public CommissionEmployee(String name, double baseSalary, double commissionRate) {
        super(name);
        this.baseSalary = baseSalary;
        this.commissionRate = commissionRate;
        this.salesAmount = 0;
    }
    
    public void addSales(double amount) {
        if (amount > 0) {
            this.salesAmount += amount;
        }
    }
    
    public void resetSales() {
        this.salesAmount = 0;
    }
    
    public double getBaseSalary() {
        return baseSalary;
    }
    
    public void setBaseSalary(double baseSalary) {
        this.baseSalary = baseSalary;
    }
    
    public double getCommissionRate() {
        return commissionRate;
    }
    
    public void setCommissionRate(double commissionRate) {
        this.commissionRate = commissionRate;
    }
    
    public double getSalesAmount() {
        return salesAmount;
    }
    
    @Override
    public double calculatePayment() {
        return baseSalary + (salesAmount * commissionRate);
    }
    
    @Override
    public String toString() {
        return "CommissionEmployee [id=" + id + ", name=" + name + 
               ", baseSalary=$" + baseSalary + ", commissionRate=" + (commissionRate * 100) + 
               "%, salesAmount=$" + salesAmount + "]";
    }
}

// Main class for testing
public class EmployeeTest {
    public static void main(String[] args) {
        // Create employees
        HourlyEmployee hourlyEmp = new HourlyEmployee("John Doe", 20.0);
        SalariedEmployee salariedEmp = new SalariedEmployee("Jane Smith", 52000.0);
        CommissionEmployee commissionEmp = new CommissionEmployee("Bob Johnson", 1000.0, 0.1);
        
        // Set hours for hourly employee
        hourlyEmp.addHours(45);
        
        // Set sales for commission employee
        commissionEmp.addSales(15000.0);
        
        // Store employees in an array
        Employee[] employees = {hourlyEmp, salariedEmp, commissionEmp};
        
        // Display employee information and calculate payments
        System.out.println("Employee Payment Details:");
        for (Employee emp : employees) {
            System.out.println("\n" + emp);
            System.out.printf("Payment: $%.2f%n", emp.calculatePayment());
        }
        
        // Calculate total payroll
        System.out.printf("\nTotal Payroll: $%.2f%n", calculateTotalPayroll(employees));
    }
    
    // Method to calculate total payroll
    public static double calculateTotalPayroll(Employee[] employees) {
        double totalPayroll = 0.0;
        
        for (Employee emp : employees) {
            totalPayroll += emp.calculatePayment();
        }
        
        return totalPayroll;
    }
}
```

### Problem 3: Vehicle Class Hierarchy

Create a hierarchy of vehicle classes with methods specific to each type.

```java
// Base Vehicle class
abstract class Vehicle {
    protected String make;
    protected String model;
    protected int year;
    protected double price;
    
    public Vehicle(String make, String model, int year, double price) {
        this.make = make;
        this.model = model;
        this.year = year;
        this.price = price;
    }
    
    // Abstract methods
    public abstract double calculateMileage();
    public abstract String getVehicleType();
    
    // Common methods
    public void displayInfo() {
        System.out.println("Vehicle Type: " + getVehicleType());
        System.out.println("Make: " + make);
        System.out.println("Model: " + model);
        System.out.println("Year: " + year);
        System.out.println("Price: $" + price);
        System.out.println("Mileage: " + calculateMileage() + " miles per gallon");
    }
    
    // Getters and setters
    public String getMake() {
        return make;
    }
    
    public void setMake(String make) {
        this.make = make;
    }
    
    public String getModel() {
        return model;
    }
    
    public void setModel(String model) {
        this.model = model;
    }
    
    public int getYear() {
        return year;
    }
    
    public void setYear(int year) {
        this.year = year;
    }
    
    public double getPrice() {
        return price;
    }
    
    public void setPrice(double price) {
        this.price = price;
    }
}

// Car class
class Car extends Vehicle {
    private int numDoors;
    private String bodyStyle;
    private double engineSize;
    
    public Car(String make, String model, int year, double price, int numDoors, String bodyStyle, double engineSize) {
        super(make, model, year, price);
        this.numDoors = numDoors;
        this.bodyStyle = bodyStyle;
        this.engineSize = engineSize;
    }
    
    @Override
    public double calculateMileage() {
        // Simple mileage calculation based on engine size
        return 30.0 - (engineSize * 5);
    }
    
    @Override
    public String getVehicleType() {
        return "Car";
    }
    
    @Override
    public void displayInfo() {
        super.displayInfo();
        System.out.println("Number of Doors: " + numDoors);
        System.out.println("Body Style: " + bodyStyle);
        System.out.println("Engine Size: " + engineSize + "L");
    }
    
    // Car-specific method
    public void honkHorn() {
        System.out.println("Beep beep!");
    }
}

// Motorcycle class
class Motorcycle extends Vehicle {
    private String type; // Sports, Cruiser, Touring, etc.
    private boolean hasSideCar;
    
    public Motorcycle(String make, String model, int year, double price, String type, boolean hasSideCar) {
        super(make, model, year, price);
        this.type = type;
        this.hasSideCar = hasSideCar;
    }
    
    @Override
    public double calculateMileage() {
        // Simple mileage calculation based on type and sidecar
        double baseMileage = 50.0;
        
        if (type.equalsIgnoreCase("Sports")) {
            baseMileage -= 10;
        } else if (type.equalsIgnoreCase("Cruiser")) {
            baseMileage -= 5;
        }
        
        if (hasSideCar) {
            baseMileage -= 8;
        }
        
        return baseMileage;
    }
    
    @Override
    public String getVehicleType() {
        return "Motorcycle";
    }
    
    @Override
    public void displayInfo() {
        super.displayInfo();
        System.out.println("Type: " + type);
        System.out.println("Has Sidecar: " + (hasSideCar ? "Yes" : "No"));
    }
    
    // Motorcycle-specific method
    public void revEngine() {
        System.out.println("Vroom vroom!");
    }
}

// Truck class
class Truck extends Vehicle {
    private double cargoCapacity;
    private boolean hasTrailer;
    private int numAxles;
    
    public Truck(String make, String model, int year, double price, double cargoCapacity, boolean hasTrailer, int numAxles) {
        super(make, model, year, price);
        this.cargoCapacity = cargoCapacity;
        this.hasTrailer = hasTrailer;
        this.numAxles = numAxles;
    }
    
    @Override
    public double calculateMileage() {
        // Simple mileage calculation based on cargo capacity, trailer, and axles
        double baseMileage = 20.0;
        
        // Reduce mileage for larger cargo capacity
        baseMileage -= (cargoCapacity / 1000);
        
        // Reduce mileage if has trailer
        if (hasTrailer) {
            baseMileage -= 5;
        }
        
        // Reduce mileage for more axles
        baseMileage -= (numAxles - 2) * 2;
        
        return Math.max(baseMileage, 5.0); // Minimum 5 MPG
    }
    
    @Override
    public String getVehicleType() {
        return "Truck";
    }
    
    @Override
    public void displayInfo() {
        super.displayInfo();
        System.out.println("Cargo Capacity: " + cargoCapacity + " lbs");
        System.out.println("Has Trailer: " + (hasTrailer ? "Yes" : "No"));
        System.out.println("Number of Axles: " + numAxles);
    }
    
    // Truck-specific method
    public void loadCargo(double weight) {
        if (weight <= cargoCapacity) {
            System.out.println("Loading " + weight + " lbs of cargo...");
        } else {
            System.out.println("Error: Cannot load " + weight + " lbs. Exceeds capacity of " + cargoCapacity + " lbs.");
        }
    }
}

// Main class for testing
public class VehicleTest {
    public static void main(String[] args) {
        // Create different types of vehicles
        Car car = new Car("Honda", "Accord", 2020, 25000.0, 4, "Sedan", 2.0);
        Motorcycle motorcycle = new Motorcycle("Harley-Davidson", "Sportster", 2021, 15000.0, "Cruiser", false);
        Truck truck = new Truck("Ford", "F-150", 2019, 45000.0, 2000.0, true, 3);
        
        // Store vehicles in an array
        Vehicle[] vehicles = {car, motorcycle, truck};
        
        // Display information for each vehicle
        for (Vehicle vehicle : vehicles) {
            vehicle.displayInfo();
            System.out.println();
        }
        
        // Call type-specific methods using actual types
        System.out.println("Type-specific behaviors:");
        car.honkHorn();
        motorcycle.revEngine();
        truck.loadCargo(1500.0);
        truck.loadCargo(2500.0); // Should show error
        
        // Calculate average mileage of all vehicles
        System.out.println("\nAverage mileage of all vehicles: " + calculateAverageMileage(vehicles) + " MPG");
    }
    
    // Calculate average mileage of vehicles
    public static double calculateAverageMileage(Vehicle[] vehicles) {
        if (vehicles.length == 0) {
            return 0.0;
        }
        
        double totalMileage = 0.0;
        for (Vehicle vehicle : vehicles) {
            totalMileage += vehicle.calculateMileage();
        }
        
        return totalMileage / vehicles.length;
    }
}
```

## Abstraction and Interfaces

### Problem 1: Media Player

Create a media player system using interfaces for different media types.

```java
// Playable interface
interface Playable {
    void play();
    void pause();
    void stop();
    void next();
    void previous();
    String getTitle();
}

// Volume control interface
interface VolumeControl {
    void increaseVolume();
    void decreaseVolume();
    void mute();
    int getVolume();
}

// Display interface
interface Displayable {
    void showInfo();
    String getType();
}

// Base MediaPlayer class implementing common functionality
abstract class MediaPlayer implements Playable, VolumeControl, Displayable {
    protected String title;
    protected boolean isPlaying;
    protected int volume;
    protected boolean isMuted;
    
    public MediaPlayer(String title) {
        this.title = title;
        this.isPlaying = false;
        this.volume = 50; // Default volume 50%
        this.isMuted = false;
    }
    
    // Implementing Playable methods
    @Override
    public void play() {
        isPlaying = true;
        System.out.println("Playing: " + title);
    }
    
    @Override
    public void pause() {
        if (isPlaying) {
            isPlaying = false;
            System.out.println("Paused: " + title);
        }
    }
    
    @Override
    public void stop() {
        isPlaying = false;
        System.out.println("Stopped: " + title);
    }
    
    @Override
    public String getTitle() {
        return title;
    }
    
    // Implementing VolumeControl methods
    @Override
    public void increaseVolume() {
        if (volume < 100) {
            volume += 10;
            System.out.println("Volume increased to " + volume + "%");
        }
    }
    
    @Override
    public void decreaseVolume() {
        if (volume > 0) {
            volume -= 10;
            System.out.println("Volume decreased to " + volume + "%");
        }
    }
    
    @Override
    public void mute() {
        isMuted = !isMuted;
        System.out.println(isMuted ? "Muted" : "Unmuted");
    }
    
    @Override
    public int getVolume() {
        return isMuted ? 0 : volume;
    }
    
    // Implementing Displayable method
    @Override
    public void showInfo() {
        System.out.println("Media Type: " + getType());
        System.out.println("Title: " + title);
        System.out.println("Status: " + (isPlaying ? "Playing" : "Stopped"));
        System.out.println("Volume: " + getVolume() + "%" + (isMuted ? " (Muted)" : ""));
    }
}

// Audio player class
class AudioPlayer extends MediaPlayer {
    private String artist;
    private String album;
    private int duration; // in seconds
    
    public AudioPlayer(String title, String artist, String album, int duration) {
        super(title);
        this.artist = artist;
        this.album = album;
        this.duration = duration;
    }
    
    @Override
    public void next() {
        System.out.println("Playing next audio track");
    }
    
    @Override
    public void previous() {
        System.out.println("Playing previous audio track");
    }
    
    @Override
    public String getType() {
        return "Audio";
    }
    
    @Override
    public void showInfo() {
        super.showInfo();
        System.out.println("Artist: " + artist);
        System.out.println("Album: " + album);
        System.out.println("Duration: " + formatDuration(duration));
    }
    
    private String formatDuration(int seconds) {
        int minutes = seconds / 60;
        int remainingSeconds = seconds % 60;
        return minutes + ":" + (remainingSeconds < 10 ? "0" : "") + remainingSeconds;
    }
}

// Video player class
class VideoPlayer extends MediaPlayer {
    private String director;
    private int duration; // in seconds
    private String resolution;
    
    public VideoPlayer(String title, String director, int duration, String resolution) {
        super(title);
        this.director = director;
        this.duration = duration;
        this.resolution = resolution;
    }
    
    @Override
    public void next() {
        System.out.println("Playing next video");
    }
    
    @Override
    public void previous() {
        System.out.println("Playing previous video");
    }
    
    @Override
    public String getType() {
        return "Video";
    }
    
    @Override
    public void showInfo() {
        super.showInfo();
        System.out.println("Director: " + director);
        System.out.println("Duration: " + formatDuration(duration));
        System.out.println("Resolution: " + resolution);
    }
    
    private String formatDuration(int seconds) {
        int hours = seconds / 3600;
        int minutes = (seconds % 3600) / 60;
        int remainingSeconds = seconds % 60;
        return hours + ":" + (minutes < 10 ? "0" : "") + minutes + ":" + 
               (remainingSeconds < 10 ? "0" : "") + remainingSeconds;
    }
    
    // Video-specific method
    public void changeResolution(String newResolution) {
        this.resolution = newResolution;
        System.out.println("Resolution changed to " + newResolution);
    }
}

// Main class for testing
public class MediaPlayerTest {
    public static void main(String[] args) {
        // Create media players
        AudioPlayer audioPlayer = new AudioPlayer("Bohemian Rhapsody", "Queen", "A Night at the Opera", 355);
        VideoPlayer videoPlayer = new VideoPlayer("The Shawshank Redemption", "Frank Darabont", 8520, "1080p");
        
        // Test AudioPlayer
        System.out.println("===== Audio Player =====");
        audioPlayer.play();
        audioPlayer.increaseVolume();
        audioPlayer.increaseVolume();
        audioPlayer.showInfo();
        audioPlayer.next();
        audioPlayer.pause();
        audioPlayer.mute();
        audioPlayer.showInfo();
        audioPlayer.stop();
        
        // Test VideoPlayer
        System.out.println("\n===== Video Player =====");
        videoPlayer.play();
        videoPlayer.decreaseVolume();
        videoPlayer.changeResolution("4K");
        videoPlayer.showInfo();
        videoPlayer.previous();
        videoPlayer.stop();
        
        // Demonstrate polymorphism
        System.out.println("\n===== Using Polymorphism =====");
        Playable[] mediaItems = {audioPlayer, videoPlayer};
        
        for (Playable item : mediaItems) {
            System.out.println("\nPlaying: " + item.getTitle());
            item.play();
            
            // We can cast to check if it's a specific type
            if (item instanceof VideoPlayer) {
                VideoPlayer video = (VideoPlayer) item;
                video.changeResolution("720p");
            }
            
            // Using another interface method
            if (item instanceof Displayable) {
                ((Displayable) item).showInfo();
            }
        }
    }
}
```

### Problem 2: Payment System

Create a payment system using interfaces for different payment methods.

```java
// Payment interface
interface Payable {
    boolean processPayment(double amount);
    boolean refundPayment(double amount);
    String getPaymentMethod();
    double getBalance();
}

// Transaction interface
interface Transactional {
    String generateTransactionId();
    void recordTransaction(String type, double amount);
    void printTransactionHistory();
}

// Credit Card Payment
class CreditCardPayment implements Payable, Transactional {
    private String cardNumber;
    private String cardHolderName;
    private String expiryDate;
    private int cvv;
    private double creditLimit;
    private double balance;
    private ArrayList<String> transactions;
    
    public CreditCardPayment(String cardNumber, String cardHolderName, String expiryDate, int cvv, double creditLimit) {
        this.cardNumber = maskCardNumber(cardNumber);
        this.cardHolderName = cardHolderName;
        this.expiryDate = expiryDate;
        this.cvv = cvv;
        this.creditLimit = creditLimit;
        this.balance = 0;
        this.transactions = new ArrayList<>();
    }
    
    private String maskCardNumber(String cardNumber) {
        // Remove spaces and dashes
        cardNumber = cardNumber.replaceAll("[\\s-]", "");
        
        // Keep only the last 4 digits
        if (cardNumber.length() > 4) {
            String lastFourDigits = cardNumber.substring(cardNumber.length() - 4);
            return "XXXX-XXXX-XXXX-" + lastFourDigits;
        }
        
        return cardNumber;
    }
    
    @Override
    public boolean processPayment(double amount) {
        if (amount <= 0) {
            System.out.println("Error: Invalid amount");
            return false;
        }
        
        if (balance + amount > creditLimit) {
            System.out.println("Error: Payment exceeds credit limit");
            return false;
        }
        
        balance += amount;
        recordTransaction("PAYMENT", amount);
        System.out.println("Credit card payment processed: $" + amount);
        return true;
    }
    
    @Override
    public boolean refundPayment(double amount) {
        if (amount <= 0) {
            System.out.println("Error: Invalid amount");
            return false;
        }
        
        balance -= amount;
        recordTransaction("REFUND", amount);
        System.out.println("Credit card refund processed: $" + amount);
        return true;
    }
    
    @Override
    public String getPaymentMethod() {
        return "Credit Card";
    }
    
    @Override
    public double getBalance() {
        return balance;
    }
    
    @Override
    public String generateTransactionId() {
        return "CC-" + System.currentTimeMillis() + "-" + Math.abs(new Random().nextInt(1000));
    }
    
    @Override
    public void recordTransaction(String type, double amount) {
        String transactionId = generateTransactionId();
        String timestamp = java.time.LocalDateTime.now().toString();
        String record = String.format("%s | %s | %s | $%.2f | Balance: $%.2f", 
                                     transactionId, timestamp, type, amount, balance);
        transactions.add(record);
    }
    
    @Override
    public void printTransactionHistory() {
        System.out.println("\nCredit Card Transaction History (" + cardNumber + "):");
        System.out.println("------------------------------------------------------------");
        
        if (transactions.isEmpty()) {
            System.out.println("No transactions found.");
        } else {
            for (String transaction : transactions) {
                System.out.println(transaction);
            }
        }
        
        System.out.println("------------------------------------------------------------");
        System.out.println("Current Balance: $" + balance);
        System.out.println("Available Credit: $" + (creditLimit - balance));
    }
    
    // Credit card specific method
    public double getAvailableCredit() {
        return creditLimit - balance;
    }
}

// Digital Wallet Payment
class DigitalWalletPayment implements Payable, Transactional {
    private String walletId;
    private String ownerName;
    private double balance;
    private ArrayList<String> transactions;
    
    public DigitalWalletPayment(String walletId, String ownerName, double initialBalance) {
        this.walletId = walletId;
        this.ownerName = ownerName;
        this.balance = initialBalance;
        this.transactions = new ArrayList<>();
        
        if (initialBalance > 0) {
            recordTransaction("DEPOSIT", initialBalance);
        }
    }
    
    public void addFunds(double amount) {
        if (amount <= 0) {
            System.out.println("Error: Invalid amount");
            return;
        }
        
        balance += amount;
        recordTransaction("DEPOSIT", amount);
        System.out.println("Funds added to wallet: $" + amount);
    }
    
    @Override
    public boolean processPayment(double amount) {
        if (amount <= 0) {
            System.out.println("Error: Invalid amount");
            return false;
        }
        
        if (amount > balance) {
            System.out.println("Error: Insufficient funds in wallet");
            return false;
        }
        
        balance -= amount;
        recordTransaction("PAYMENT", amount);
        System.out.println("Digital wallet payment processed: $" + amount);
        return true;
    }
    
    @Override
    public boolean refundPayment(double amount) {
        if (amount <= 0) {
            System.out.println("Error: Invalid amount");
            return false;
        }
        
        balance += amount;
        recordTransaction("REFUND", amount);
        System.out.println("Digital wallet refund processed: $" + amount);
        return true;
    }
    
    @Override
    public String getPaymentMethod() {
        return "Digital Wallet";
    }
    
    @Override
    public double getBalance() {
        return balance;
    }
    
    @Override
    public String generateTransactionId() {
        return "DW-" + System.currentTimeMillis() + "-" + Math.abs(new Random().nextInt(1000));
    }
    
    @Override
    public void recordTransaction(String type, double amount) {
        String transactionId = generateTransactionId();
        String timestamp = java.time.LocalDateTime.now().toString();
        String record = String.format("%s | %s | %s | $%.2f | Balance: $%.2f", 
                                     transactionId, timestamp, type, amount, balance);
        transactions.add(record);
    }
    
    @Override
    public void printTransactionHistory() {
        System.out.println("\nDigital Wallet Transaction History (" + walletId + "):");
        System.out.println("------------------------------------------------------------");
        
        if (transactions.isEmpty()) {
            System.out.println("No transactions found.");
        } else {
            for (String transaction : transactions) {
                System.out.println(transaction);
            }
        }
        
        System.out.println("------------------------------------------------------------");
        System.out.println("Current Balance: $" + balance);
    }
}

// Bank Account Payment
class BankAccountPayment implements Payable, Transactional {
    private String accountNumber;
    private String accountHolderName;
    private String bankName;
    private double balance;
    private ArrayList<String> transactions;
    
    public BankAccountPayment(String accountNumber, String accountHolderName, String bankName, double initialBalance) {
        this.accountNumber = accountNumber;
        this.accountHolderName = accountHolderName;
        this.bankName = bankName;
        this.balance = initialBalance;
        this.transactions = new ArrayList<>();
        
        if (initialBalance > 0) {
            recordTransaction("INITIAL_BALANCE", initialBalance);
        }
    }
    
    @Override
    public boolean processPayment(double amount) {
        if (amount <= 0) {
            System.out.println("Error: Invalid amount");
            return false;
        }
        
        if (amount > balance) {
            System.out.println("Error: Insufficient funds in bank account");
            return false;
        }
        
        balance -= amount;
        recordTransaction("PAYMENT", amount);
        System.out.println("Bank account payment processed: $" + amount);
        return true;
    }
    
    @Override
    public boolean refundPayment(double amount) {
        if (amount <= 0) {
            System.out.println("Error: Invalid amount");
            return false;
        }
        
        balance += amount;
        recordTransaction("REFUND", amount);
        System.out.println("Bank account refund processed: $" + amount);
        return true;
    }
    
    @Override
    public String getPaymentMethod() {
        return "Bank Account";
    }
    
    @Override
    public double getBalance() {
        return balance;
    }
    
    @Override
    public String generateTransactionId() {
        return "BA-" + System.currentTimeMillis() + "-" + Math.abs(new Random().nextInt(1000));
    }
    
    @Override
    public void recordTransaction(String type, double amount) {
        String transactionId = generateTransactionId();
        String timestamp = java.time.LocalDateTime.now().toString();
        String record = String.format("%s | %s | %s | $%.2f | Balance: $%.2f", 
                                     transactionId, timestamp, type, amount, balance);
        transactions.add(record);
    }
    
    @Override
    public void printTransactionHistory() {
        System.out.println("\nBank Account Transaction History (" + accountNumber + "):");
        System.out.println("Bank: " + bankName);
        System.out.println("------------------------------------------------------------");
        
        if (transactions.isEmpty()) {
            System.out.println("No transactions found.");
        } else {
            for (String transaction : transactions) {
                System.out.println(transaction);
            }
        }
        
        System.out.println("------------------------------------------------------------");
        System.out.println("Current Balance: $" + balance);
    }
    
    // Bank account specific method
    public void deposit(double amount) {
        if (amount <= 0) {
            System.out.println("Error: Invalid amount");
            return;
        }
        
        balance += amount;
        recordTransaction("DEPOSIT", amount);
        System.out.println("Deposit to bank account: $" + amount);
    }
}

// Payment processor class
class PaymentProcessor {
    public static boolean processPayment(Payable paymentMethod, double amount) {
        System.out.println("\nProcessing payment of $" + amount + " using " + paymentMethod.getPaymentMethod());
        return paymentMethod.processPayment(amount);
    }
    
    public static boolean refundPayment(Payable paymentMethod, double amount) {
        System.out.println("\nProcessing refund of $" + amount + " using " + paymentMethod.getPaymentMethod());
        return paymentMethod.refundPayment(amount);
    }
    
    public static void displayBalance(Payable paymentMethod) {
        System.out.println("\nCurrent balance for " + paymentMethod.getPaymentMethod() + ": $" + 
                          paymentMethod.getBalance());
    }
}

// Main class for testing
public class PaymentSystemTest {
    public static void main(String[] args) {
        // Create payment methods
        CreditCardPayment creditCard = new CreditCardPayment("1234-5678-9012-3456", "John Doe", "12/25", 123, 5000.0);
        DigitalWalletPayment digitalWallet = new DigitalWalletPayment("wallet123", "Jane Smith", 1000.0);
        BankAccountPayment bankAccount = new BankAccountPayment("987654321", "Bob Johnson", "Chase Bank", 2500.0);
        
        // Process payments
        PaymentProcessor.processPayment(creditCard, 1500.0);
        PaymentProcessor.processPayment(digitalWallet, 750.0);
        PaymentProcessor.processPayment(bankAccount, 1200.0);
        
        // Process refunds
        PaymentProcessor.refundPayment(creditCard, 200.0);
        PaymentProcessor.refundPayment(digitalWallet, 50.0);
        
        // Display balances
        PaymentProcessor.displayBalance(creditCard);
        PaymentProcessor.displayBalance(digitalWallet);
        PaymentProcessor.displayBalance(bankAccount);
        
        // Test specific methods
        System.out.println("\nAvailable credit: $" + creditCard.getAvailableCredit());
        digitalWallet.addFunds(500.0);
        bankAccount.deposit(1000.0);
        
        // Print transaction histories
        creditCard.printTransactionHistory();
        digitalWallet.printTransactionHistory();
        bankAccount.printTransactionHistory();
        
        // Demonstrate polymorphism
        System.out.println("\n===== Using Polymorphism =====");
        Payable[] paymentMethods = {creditCard, digitalWallet, bankAccount};
        
        for (Payable method : paymentMethods) {
            System.out.println("\nPayment Method: " + method.getPaymentMethod());
            System.out.println("Current Balance: $" + method.getBalance());
            
            // Process a small payment for each method
            method.processPayment(100.0);
            
            // Print transaction history if available
            if (method instanceof Transactional) {
                ((Transactional) method).printTransactionHistory();
            }
        }
    }
}
```

## Encapsulation

### Problem: User Account System

Create a user account system demonstrating proper encapsulation with private fields and public methods.

```java
import java.util.ArrayList;
import java.util.List;
import java.util.regex.Pattern;

// UserAccount class with proper encapsulation
class UserAccount {
    // Private fields - not accessible directly from outside the class
    private String userId;
    private String username;
    private String email;
    private String password;
    private String phoneNumber;
    private boolean isActive;
    private List<String> loginHistory;
    private static int nextUserId = 1001;
    
    // Constructor
    public UserAccount(String username, String email, String password, String phoneNumber) {
        // Validate inputs
        if (!isValidUsername(username)) {
            throw new IllegalArgumentException("Invalid username format");
        }
        if (!isValidEmail(email)) {
            throw new IllegalArgumentException("Invalid email format");
        }
        if (!isValidPassword(password)) {
            throw new IllegalArgumentException("Invalid password format");
        }
        if (!isValidPhoneNumber(phoneNumber)) {
            throw new IllegalArgumentException("Invalid phone number format");
        }
        
        this.userId = "USER" + nextUserId++;
        this.username = username;
        this.email = email;
        this.password = encryptPassword(password); // Store encrypted password
        this.phoneNumber = phoneNumber;
        this.isActive = true;
        this.loginHistory = new ArrayList<>();
    }
    
    // Getters - provide controlled access to private fields
    public String getUserId() {
        return userId;
    }
    
    public String getUsername() {
        return username;
    }
    
    public String getEmail() {
        return email;
    }
    
    // No getter for password - security measure
    
    public String getPhoneNumber() {
        return phoneNumber;
    }
    
    public boolean isActive() {
        return isActive;
    }
    
    public List<String> getLoginHistory() {
        // Return a copy to prevent external modification
        return new ArrayList<>(loginHistory);
    }
    
    // Setters - provide controlled access to modify private fields
    public void setUsername(String username) {
        if (!isValidUsername(username)) {
            throw new IllegalArgumentException("Invalid username format");
        }
        this.username = username;
    }
    
    public void setEmail(String email) {
        if (!isValidEmail(email)) {
            throw new IllegalArgumentException("Invalid email format");
        }
        this.email = email;
    }
    
    public void setPhoneNumber(String phoneNumber) {
        if (!isValidPhoneNumber(phoneNumber)) {
            throw new IllegalArgumentException("Invalid phone number format");
        }
        this.phoneNumber = phoneNumber;
    }
    
    public void setActive(boolean isActive) {
        this.isActive = isActive;
    }
    
    // Public methods
    public boolean changePassword(String oldPassword, String newPassword) {
        // Verify old password matches
        if (!verifyPassword(oldPassword)) {
            System.out.println("Error: Current password is incorrect");
            return false;
        }
        
        // Validate new password
        if (!isValidPassword(newPassword)) {
            System.out.println("Error: New password does not meet requirements");
            return false;
        }
        
        // Update password
        this.password = encryptPassword(newPassword);
        System.out.println("Password changed successfully");
        return true;
    }
    
    public boolean login(String passwordAttempt) {
        if (!isActive) {
            System.out.println("Error: Account is not active");
            return false;
        }
        
        if (verifyPassword(passwordAttempt)) {
            String timestamp = java.time.LocalDateTime.now().toString();
            loginHistory.add(timestamp);
            System.out.println("Login successful");
            return true;
        } else {
            System.out.println("Error: Invalid password");
            return false;
        }
    }
    
    public void deactivateAccount() {
        this.isActive = false;
        System.out.println("Account deactivated");
    }
    
    public void reactivateAccount() {
        this.isActive = true;
        System.out.println("Account reactivated");
    }
    
    public void displayAccountInfo() {
        System.out.println("User ID: " + userId);
        System.out.println("Username: " + username);
        System.out.println("Email: " + email);
        System.out.println("Phone Number: " + phoneNumber);
        System.out.println("Status: " + (isActive ? "Active" : "Inactive"));
        System.out.println("Last Login: " + (loginHistory.isEmpty() ? "Never" : loginHistory.get(loginHistory.size() - 1)));
    }
    
    // Private helper methods - encapsulated functionality
    private boolean isValidUsername(String username) {
        // Username must be 3-20 characters and contain only letters, numbers, and underscores
        return username != null && Pattern.matches("^[a-zA-Z0-9_]{3,20}$", username);
    }
    
    private boolean isValidEmail(String email) {
        // Simple email validation
        return email != null && Pattern.matches("^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$", email);
    }
    
    private boolean isValidPassword(String password) {
        // Password must be at least 8 characters with at least one uppercase, one lowercase, one digit
        return password != null && Pattern.matches("^(?=.*[a-z])(?=.*[A-Z])(?=.*\\d).{8,}$", password);
    }
    
    private boolean isValidPhoneNumber(String phoneNumber) {
        // Simple phone number validation
        return phoneNumber != null && Pattern.matches("^\\d{10}$", phoneNumber);
    }
    
    private String encryptPassword(String password) {
        // Simple "encryption" for demonstration purposes
        // In a real system, use a proper password hashing function like BCrypt
        StringBuilder encrypted = new StringBuilder();
        for (char c : password.toCharArray()) {
            encrypted.append((char) (c + 1));
        }
        return encrypted.toString();
    }
    
    private boolean verifyPassword(String passwordAttempt) {
        // Encrypt the attempt and compare with stored encrypted password
        return encryptPassword(passwordAttempt).equals(this.password);
    }
}

// UserManager class to manage multiple user accounts
class UserManager {
    private List<UserAccount> users;
    
    public UserManager() {
        this.users = new ArrayList<>();
    }
    
    public void addUser(UserAccount user) {
        // Check if username or email already exists
        for (UserAccount existingUser : users) {
            if (existingUser.getUsername().equals(user.getUsername())) {
                System.out.println("Error: Username already exists");
                return;
            }
            if (existingUser.getEmail().equals(user.getEmail())) {
                System.out.println("Error: Email already exists");
                return;
            }
        }
        
        users.add(user);
        System.out.println("User added successfully: " + user.getUsername());
    }
    
    public UserAccount findUserByUsername(String username) {
        for (UserAccount user : users) {
            if (user.getUsername().equals(username)) {
                return user;
            }
        }
        return null;
    }
    
    public UserAccount findUserByEmail(String email) {
        for (UserAccount user : users) {
            if (user.getEmail().equals(email)) {
                return user;
            }
        }
        return null;
    }
    
    public UserAccount findUserById(String userId) {
        for (UserAccount user : users) {
            if (user.getUserId().equals(userId)) {
                return user;
            }
        }
        return null;
    }
    
    public void removeUser(String userId) {
        UserAccount user = findUserById(userId);
        if (user != null) {
            users.remove(user);
            System.out.println("User removed: " + user.getUsername());
        } else {
            System.out.println("Error: User not found");
        }
    }
    
    public void displayAllUsers() {
        if (users.isEmpty()) {
            System.out.println("No users found");
            return;
        }
        
        System.out.println("\nAll Users:");
        System.out.println("----------------------------------------------------");
        for (UserAccount user : users) {
            System.out.println("ID: " + user.getUserId() + 
                               " | Username: " + user.getUsername() +
                               " | Email: " + user.getEmail() +
                               " | Status: " + (user.isActive() ? "Active" : "Inactive"));
        }
        System.out.println("----------------------------------------------------");
    }
}

// Main class for testing
public class UserAccountSystem {
    public static void main(String[] args) {
        // Create a user manager
        UserManager userManager = new UserManager();
        
        try {
            // Create users
            UserAccount user1 = new UserAccount("john_doe", "john@example.com", "Password123", "1234567890");
            UserAccount user2 = new UserAccount("jane_smith", "jane@example.com", "Secure456", "9876543210");
            
            // Add users to manager
            userManager.addUser(user1);
            userManager.addUser(user2);
            
            // Display all users
            userManager.displayAllUsers();
            
            // Test login
            System.out.println("\nTesting login:");
            user1.login("WrongPassword"); // Should fail
            user1.login("Password123"); // Should succeed
            
            // Test changing password
            System.out.println("\nTesting password change:");
            user1.changePassword("WrongPassword", "NewPass789"); // Should fail
            user1.changePassword("Password123", "NewPass789"); // Should succeed
            
            // Test login with new password
            System.out.println("\nTesting login with new password:");
            user1.login("Password123"); // Should fail
            user1.login("NewPass789"); // Should succeed
            
            // Test deactivating account
            System.out.println("\nTesting account deactivation:");
            user2.deactivateAccount();
            user2.login("Secure456"); // Should fail because account is inactive
            
            // Test reactivating account
            System.out.println("\nTesting account reactivation:");
            user2.reactivateAccount();
            user2.login("Secure456"); // Should succeed now
            
            // Display account info
            System.out.println("\nAccount Information:");
            user1.displayAccountInfo();
            
            // Test finding users
            System.out.println("\nTesting user search:");
            UserAccount foundUser = userManager.findUserByUsername("john_doe");
            if (foundUser != null) {
                System.out.println("Found user: " + foundUser.getUserId());
            }
            
            // Try to create user with invalid data
            System.out.println("\nTesting validation:");
            try {
                UserAccount invalidUser = new UserAccount("ab", "invalid-email", "weak", "123");
                System.out.println("This should not print if validation works!");
            } catch (IllegalArgumentException e) {
                System.out.println("Validation error caught: " + e.getMessage());
            }
            
        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

## Next Steps

After completing these practice problems, you should have a good understanding of object-oriented programming concepts in Java. Next, you can move on to:

1. Learn about [Java Collections Framework](/collections/README.md) to work with dynamic data structures
2. Explore [Exception Handling](/exceptions/README.md) to make your code more robust
3. Study [File I/O](/io/README.md) to work with files and streams
4. Dive into [Concurrency](/concurrency/README.md) to create multi-threaded applications