# Java I/O (Input/Output)

Java provides a rich set of classes and interfaces to handle input and output operations. This guide covers both traditional I/O and the newer NIO (New I/O) APIs, focusing on file operations, streams, readers, writers, and more.

## Java I/O Package Structure

Java I/O functionality is primarily contained in the following packages:

- `java.io`: Traditional I/O with streams, readers, and writers
- `java.nio`: New I/O with buffers, channels, and selectors
- `java.nio.file`: File system operations with Path interface and Files utility class
- `java.nio.channels`: Channel interfaces and implementations
- `java.nio.charset`: Character set encodings and decoders

## File Handling

### File Class (java.io.File)

The `File` class represents a file or directory path.

```java
// Creating File objects
File file1 = new File("data.txt");
File file2 = new File("/home/user/documents/report.pdf");
File directory = new File("src/main/resources");

// Checking properties
boolean exists = file1.exists();
boolean isFile = file1.isFile();
boolean isDirectory = directory.isDirectory();
long length = file1.length();
long lastModified = file1.lastModified();

// File operations
boolean created = file1.createNewFile(); // Creates a new file if it doesn't exist
boolean renamed = file1.renameTo(new File("newData.txt"));
boolean deleted = file1.delete();

// Directory operations
boolean dirCreated = directory.mkdir(); // Creates a single directory
boolean dirsCreated = directory.mkdirs(); // Creates directory including any necessary parents
String[] contents = directory.list(); // Lists filenames in the directory
File[] files = directory.listFiles(); // Lists files in the directory
```

### Path and Files (java.nio.file)

The `Path` interface and `Files` utility class provide more powerful file operations.

```java
// Creating Path objects
Path path1 = Paths.get("data.txt");
Path path2 = Paths.get("/home", "user", "documents", "report.pdf");
Path directory = Paths.get("src/main/resources");

// Getting information
boolean exists = Files.exists(path1);
boolean isRegularFile = Files.isRegularFile(path1);
boolean isDirectory = Files.isDirectory(directory);
long size = Files.size(path1);
FileTime lastModified = Files.getLastModifiedTime(path1);

// File attributes
BasicFileAttributes attrs = Files.readAttributes(path1, BasicFileAttributes.class);
long creationTime = attrs.creationTime().toMillis();
boolean isSymbolicLink = attrs.isSymbolicLink();

// File operations
Files.createFile(path1); // Creates a new file
Files.move(path1, path2); // Moves or renames a file
Files.delete(path1); // Deletes a file
Files.copy(path1, path2); // Copies a file
Files.createDirectories(directory); // Creates directories and parent directories

// Directory operations
try (DirectoryStream<Path> stream = Files.newDirectoryStream(directory)) {
    for (Path file: stream) {
        System.out.println(file.getFileName());
    }
}

// Walking a file tree
Files.walkFileTree(directory, new SimpleFileVisitor<Path>() {
    @Override
    public FileVisitResult visitFile(Path file, BasicFileAttributes attrs) {
        System.out.println(file);
        return FileVisitResult.CONTINUE;
    }
});
```

## Byte Streams

Byte streams handle I/O of raw binary data (8-bit bytes).

### Input Streams

#### FileInputStream

Reads bytes from a file.

```java
try (FileInputStream fis = new FileInputStream("input.dat")) {
    int byteData;
    while ((byteData = fis.read()) != -1) {
        // Process each byte
        System.out.print(byteData + " ");
    }
}
```

#### BufferedInputStream

Adds buffering to an input stream for more efficient reading.

```java
try (BufferedInputStream bis = new BufferedInputStream(
        new FileInputStream("input.dat"))) {
    byte[] buffer = new byte[1024];
    int bytesRead;
    while ((bytesRead = bis.read(buffer)) != -1) {
        // Process bytes in buffer
        for (int i = 0; i < bytesRead; i++) {
            System.out.print(buffer[i] + " ");
        }
    }
}
```

#### DataInputStream

Reads primitive Java data types from an underlying input stream.

```java
try (DataInputStream dis = new DataInputStream(
        new FileInputStream("data.bin"))) {
    int intValue = dis.readInt();
    double doubleValue = dis.readDouble();
    boolean boolValue = dis.readBoolean();
}
```

#### ObjectInputStream

Deserializes Java objects from a stream.

```java
try (ObjectInputStream ois = new ObjectInputStream(
        new FileInputStream("objects.ser"))) {
    Object obj1 = ois.readObject();
    Object obj2 = ois.readObject();
    // Cast to appropriate types
    Person person = (Person) obj1;
}
```

### Output Streams

#### FileOutputStream

Writes bytes to a file.

```java
try (FileOutputStream fos = new FileOutputStream("output.dat")) {
    byte[] data = {65, 66, 67, 68, 69}; // ABCDE in ASCII
    fos.write(data);
    // Alternatively, write one byte at a time
    fos.write(70); // F in ASCII
}
```

#### BufferedOutputStream

Adds buffering to an output stream for more efficient writing.

```java
try (BufferedOutputStream bos = new BufferedOutputStream(
        new FileOutputStream("output.dat"))) {
    byte[] data = new byte[1024];
    // Fill data array
    bos.write(data);
    // No need to call flush() - try-with-resources will close the stream
}
```

#### DataOutputStream

Writes primitive Java data types to an underlying output stream.

```java
try (DataOutputStream dos = new DataOutputStream(
        new FileOutputStream("data.bin"))) {
    dos.writeInt(42);
    dos.writeDouble(3.14159);
    dos.writeBoolean(true);
}
```

#### ObjectOutputStream

Serializes Java objects to a stream.

```java
try (ObjectOutputStream oos = new ObjectOutputStream(
        new FileOutputStream("objects.ser"))) {
    Person person = new Person("John", 30);
    oos.writeObject(person);
}
```

## Character Streams

Character streams handle I/O of character data, automatically handling character encoding.

### Readers

#### FileReader

Reads characters from a file.

```java
try (FileReader fr = new FileReader("text.txt")) {
    int charData;
    while ((charData = fr.read()) != -1) {
        // Process each character
        System.out.print((char) charData);
    }
}
```

#### BufferedReader

Adds buffering to a reader for more efficient reading and provides readLine() method.

```java
try (BufferedReader br = new BufferedReader(
        new FileReader("text.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        // Process each line
        System.out.println(line);
    }
}
```

#### InputStreamReader

Bridges from byte streams to character streams by using a specified character encoding.

```java
try (InputStreamReader isr = new InputStreamReader(
        new FileInputStream("text.txt"), StandardCharsets.UTF_8)) {
    char[] buffer = new char[1024];
    int charsRead;
    while ((charsRead = isr.read(buffer)) != -1) {
        // Process characters in buffer
        System.out.print(new String(buffer, 0, charsRead));
    }
}
```

### Writers

#### FileWriter

Writes characters to a file.

```java
try (FileWriter fw = new FileWriter("output.txt")) {
    fw.write("Hello, World!");
    fw.write(System.lineSeparator());
    fw.write("Another line of text.");
}
```

#### BufferedWriter

Adds buffering to a writer for more efficient writing and provides newLine() method.

```java
try (BufferedWriter bw = new BufferedWriter(
        new FileWriter("output.txt"))) {
    bw.write("Hello, World!");
    bw.newLine();
    bw.write("Another line of text.");
}
```

#### OutputStreamWriter

Bridges from character streams to byte streams by using a specified character encoding.

```java
try (OutputStreamWriter osw = new OutputStreamWriter(
        new FileOutputStream("output.txt"), StandardCharsets.UTF_8)) {
    osw.write("Hello, World with UTF-8 encoding!");
}
```

#### PrintWriter

Provides convenient methods for printing formatted text.

```java
try (PrintWriter pw = new PrintWriter(
        new FileWriter("output.txt"))) {
    pw.println("Hello, World!");
    pw.printf("Name: %s, Age: %d%n", "John", 30);
}
```

## NIO (New I/O)

NIO provides more powerful and flexible I/O operations with buffers, channels, and selectors.

### Buffers

Buffers are containers for data being transferred. The most commonly used buffer types are:

- `ByteBuffer`
- `CharBuffer`
- `IntBuffer`
- `FloatBuffer`
- `DoubleBuffer`

```java
// Creating buffers
ByteBuffer buffer1 = ByteBuffer.allocate(1024); // Heap buffer
ByteBuffer buffer2 = ByteBuffer.allocateDirect(1024); // Direct buffer
ByteBuffer buffer3 = ByteBuffer.wrap(new byte[1024]); // Wraps an existing array

// Using a buffer
buffer1.put((byte) 65); // Put a byte
buffer1.putInt(42); // Put an int
buffer1.putFloat(3.14f); // Put a float

// Preparing to read
buffer1.flip(); // Flip from write mode to read mode

// Reading from a buffer
byte byteValue = buffer1.get(); // Get a byte
int intValue = buffer1.getInt(); // Get an int
float floatValue = buffer1.getFloat(); // Get a float

// Resetting the buffer
buffer1.clear(); // Reset for writing from scratch
// or
buffer1.compact(); // Move remaining data to the beginning and prepare for writing
```

### Channels

Channels represent open connections to entities such as files, sockets, or pipes that are capable of performing I/O operations.

#### FileChannel

```java
// Reading from a file using a channel
try (RandomAccessFile file = new RandomAccessFile("data.bin", "r");
     FileChannel channel = file.getChannel()) {
    
    ByteBuffer buffer = ByteBuffer.allocate(1024);
    int bytesRead = channel.read(buffer);
    
    buffer.flip();
    while (buffer.hasRemaining()) {
        System.out.print(buffer.get() + " ");
    }
}

// Writing to a file using a channel
try (RandomAccessFile file = new RandomAccessFile("output.bin", "rw");
     FileChannel channel = file.getChannel()) {
    
    ByteBuffer buffer = ByteBuffer.allocate(1024);
    // Fill buffer with data
    buffer.putInt(42);
    buffer.putInt(7);
    
    buffer.flip();
    channel.write(buffer);
}

// Using transferTo/transferFrom for efficient file copying
try (FileChannel sourceChannel = FileChannel.open(Paths.get("source.dat"), StandardOpenOption.READ);
     FileChannel targetChannel = FileChannel.open(Paths.get("target.dat"),
             StandardOpenOption.CREATE, StandardOpenOption.WRITE)) {
    
    sourceChannel.transferTo(0, sourceChannel.size(), targetChannel);
    // Or equivalently:
    // targetChannel.transferFrom(sourceChannel, 0, sourceChannel.size());
}
```

### Memory-Mapped Files

Memory-mapped files allow you to map a file directly into memory for extremely fast access.

```java
try (RandomAccessFile file = new RandomAccessFile("data.bin", "rw");
     FileChannel channel = file.getChannel()) {
    
    MappedByteBuffer buffer = channel.map(
            FileChannel.MapMode.READ_WRITE, 0, channel.size());
    
    // Direct access to file data as if it were in memory
    buffer.putInt(0, 100); // Write an int at position 0
    int value = buffer.getInt(4); // Read an int from position 4
}
```

## File Locking

File locks allow you to coordinate access to a file between multiple processes.

```java
try (RandomAccessFile file = new RandomAccessFile("shared.dat", "rw");
     FileChannel channel = file.getChannel()) {
    
    // Exclusive lock (for writing)
    FileLock lock = channel.lock();
    try {
        // Perform operations on the file
        file.writeInt(42);
    } finally {
        lock.release();
    }
    
    // Shared lock (for reading)
    FileLock sharedLock = channel.lock(0, Long.MAX_VALUE, true);
    try {
        // Read from the file
        int value = file.readInt();
    } finally {
        sharedLock.release();
    }
}
```

## Character Sets and Encodings

Java NIO provides support for different character sets and encodings.

```java
// Get available character sets
SortedMap<String, Charset> charsets = Charset.availableCharsets();
for (String name : charsets.keySet()) {
    System.out.println(name);
}

// Convert between different encodings
String text = "Hello, World!";
Charset utf8 = StandardCharsets.UTF_8;
Charset utf16 = StandardCharsets.UTF_16;

// String to bytes
byte[] utf8Bytes = text.getBytes(utf8);
byte[] utf16Bytes = text.getBytes(utf16);

// Bytes to string
String utf8Text = new String(utf8Bytes, utf8);
String utf16Text = new String(utf16Bytes, utf16);
```

## Common I/O Tasks

### Reading Text Files

```java
// Using BufferedReader (Traditional I/O)
try (BufferedReader reader = new BufferedReader(new FileReader("file.txt"))) {
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
}

// Using Files utility (NIO)
List<String> lines = Files.readAllLines(Paths.get("file.txt"));
for (String line : lines) {
    System.out.println(line);
}

// For large files, use a Stream (Java 8+)
try (Stream<String> stream = Files.lines(Paths.get("file.txt"))) {
    stream.forEach(System.out::println);
}
```

### Writing Text Files

```java
// Using BufferedWriter (Traditional I/O)
try (BufferedWriter writer = new BufferedWriter(new FileWriter("file.txt"))) {
    writer.write("Line 1");
    writer.newLine();
    writer.write("Line 2");
}

// Using Files utility (NIO)
List<String> lines = Arrays.asList("Line 1", "Line 2");
Files.write(Paths.get("file.txt"), lines);

// Appending to a file
Files.write(Paths.get("file.txt"), 
        Arrays.asList("Line 3", "Line 4"), 
        StandardOpenOption.APPEND);
```

### Reading Binary Files

```java
// Read all bytes
byte[] bytes = Files.readAllBytes(Paths.get("data.bin"));

// For large files, use a BufferedInputStream
try (BufferedInputStream bis = new BufferedInputStream(
        new FileInputStream("data.bin"))) {
    byte[] buffer = new byte[8192];
    int bytesRead;
    while ((bytesRead = bis.read(buffer)) != -1) {
        // Process bytes in buffer
    }
}

// Using NIO
try (FileChannel channel = FileChannel.open(Paths.get("data.bin"), 
        StandardOpenOption.READ)) {
    ByteBuffer buffer = ByteBuffer.allocate(8192);
    while (channel.read(buffer) > 0) {
        buffer.flip();
        // Process bytes in buffer
        buffer.clear();
    }
}
```

### Writing Binary Files

```java
// Write all bytes at once
byte[] bytes = {1, 2, 3, 4, 5};
Files.write(Paths.get("data.bin"), bytes);

// Using BufferedOutputStream
try (BufferedOutputStream bos = new BufferedOutputStream(
        new FileOutputStream("data.bin"))) {
    byte[] data = new byte[8192];
    // Fill data array
    bos.write(data);
}

// Using NIO
try (FileChannel channel = FileChannel.open(Paths.get("data.bin"),
        StandardOpenOption.CREATE, StandardOpenOption.WRITE)) {
    ByteBuffer buffer = ByteBuffer.allocate(8192);
    // Fill buffer with data
    buffer.flip();
    channel.write(buffer);
}
```

### File Copy

```java
// Simple copy with Files utility
Files.copy(Paths.get("source.txt"), Paths.get("target.txt"),
        StandardCopyOption.REPLACE_EXISTING);

// Manual copy with streams
try (InputStream in = new FileInputStream("source.txt");
     OutputStream out = new FileOutputStream("target.txt")) {
    byte[] buffer = new byte[8192];
    int bytesRead;
    while ((bytesRead = in.read(buffer)) != -1) {
        out.write(buffer, 0, bytesRead);
    }
}

// Efficient copy with NIO channels
try (FileChannel sourceChannel = FileChannel.open(Paths.get("source.txt"), StandardOpenOption.READ);
     FileChannel targetChannel = FileChannel.open(Paths.get("target.txt"),
             StandardOpenOption.CREATE, StandardOpenOption.WRITE)) {
    sourceChannel.transferTo(0, sourceChannel.size(), targetChannel);
}
```

## Best Practices

1. **Always close resources**: Use try-with-resources or finally blocks to ensure resources are properly closed.

2. **Use buffered streams**: Always wrap file streams with buffered streams for performance.

3. **Choose appropriate buffer sizes**: For manual buffering, choose appropriate buffer sizes (usually a multiple of the underlying disk block size, like 4KB, 8KB, or 16KB).

4. **Use character streams for text**: Use character streams (Reader/Writer) for text data to properly handle character encodings.

5. **Specify character encodings explicitly**: Always specify the character encoding explicitly to avoid platform-dependent behavior.

6. **Use NIO for advanced needs**: Use NIO for advanced needs like non-blocking I/O, memory-mapped files, or file locking.

7. **Handle exceptions properly**: Catch and handle IOException properly, either by logging or by providing meaningful error messages.

8. **Use try-with-resources**: Use try-with-resources for automatic resource management.

## Practice Exercises

1. Create a program that counts the number of characters, words, and lines in a text file.
2. Implement a simple file encryption/decryption program.
3. Write a program that reads a CSV file and converts it to a JSON file.
4. Create a file copying utility that shows progress and speed statistics.
5. Implement a simple text editor that can open, edit, and save files.

## Next Steps

- Learn about [Java NIO.2](/nio2/README.md) for advanced file operations
- Explore [Java Serialization](/serialization/README.md) for object persistence
- Study [Java Networking](/networking/README.md) for network I/O