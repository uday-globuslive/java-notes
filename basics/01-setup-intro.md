# Java Setup and Introduction

## Installing Java Development Kit (JDK)

### For Windows:
1. Download the latest JDK from [Oracle's website](https://www.oracle.com/java/technologies/downloads/) or use OpenJDK
2. Run the installer and follow the instructions
3. Set the JAVA_HOME environment variable:
   - Right-click on 'This PC' and select 'Properties'
   - Click on 'Advanced system settings'
   - Click on 'Environment Variables'
   - Under System Variables, click 'New'
   - Variable name: JAVA_HOME
   - Variable value: Path to your JDK installation (e.g., C:\Program Files\Java\jdk-17)
4. Add Java to PATH:
   - In the same Environment Variables window
   - Find the 'Path' variable under System Variables and click 'Edit'
   - Click 'New' and add '%JAVA_HOME%\bin'
   - Click 'OK' on all windows

### For macOS:
1. Install Homebrew if you don't have it already:
   ```
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
2. Install OpenJDK:
   ```
   brew install openjdk@17
   ```
3. Add JAVA_HOME to your profile:
   ```
   echo 'export JAVA_HOME=$(/usr/libexec/java_home)' >> ~/.zshrc
   source ~/.zshrc
   ```

### For Linux:
1. Install OpenJDK:
   ```
   sudo apt update
   sudo apt install openjdk-17-jdk
   ```
2. Set JAVA_HOME in .bashrc:
   ```
   echo 'export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64' >> ~/.bashrc
   echo 'export PATH=$PATH:$JAVA_HOME/bin' >> ~/.bashrc
   source ~/.bashrc
   ```

### Verify Installation:
Open a terminal or command prompt and run:
```
java -version
javac -version
```
Both commands should display the installed Java version.

## Setting Up an IDE

### IntelliJ IDEA (Recommended):
1. Download IntelliJ IDEA Community Edition from [JetBrains website](https://www.jetbrains.com/idea/download/)
2. Install and launch it
3. During setup, ensure Java JDK is correctly configured

### Eclipse:
1. Download Eclipse IDE for Java Developers from [Eclipse website](https://www.eclipse.org/downloads/)
2. Install and launch it
3. Set the JDK path during setup if prompted

### VS Code:
1. Download VS Code from [Visual Studio Code website](https://code.visualstudio.com/)
2. Install the "Extension Pack for Java" from the Extensions marketplace
3. Configure Java settings if necessary

## Hello World Program

Create a new file called `HelloWorld.java` with the following content:

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

### Compiling and Running from Command Line:
```
javac HelloWorld.java
java HelloWorld
```

### Running from IDE:
- In IntelliJ IDEA or Eclipse, right-click the file and select "Run"
- In VS Code, click the "Run" button above the main method

## Understanding the Hello World Program

- `public class HelloWorld`: Defines a class named HelloWorld
- `public static void main(String[] args)`: The main method, entry point of every Java application
- `System.out.println()`: Outputs text to the console

## Java Basics Overview

Java is:
- **Object-Oriented**: Organized around objects rather than actions
- **Platform-Independent**: "Write once, run anywhere" (WORA)
- **Strongly Typed**: Variables must be declared before use
- **Automatic Memory Management**: Includes garbage collection

## Java Program Structure

- **Package Declaration**: Optional, at the top of the file
- **Import Statements**: For using classes from other packages
- **Class Declaration**: Contains fields and methods
- **Main Method**: Entry point for application execution

## Next Steps

Now that you have Java set up, proceed to the [Variables and Data Types](/basics/02-variables-datatypes.md) section to start learning Java programming basics.