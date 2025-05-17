# Java Concurrency and Multithreading

Concurrency in Java allows multiple parts of a program to execute simultaneously. This guide covers Java's concurrency model, threads, synchronization, locks, thread pools, and concurrent collections.

## Introduction to Concurrency

Concurrency enables a program to perform multiple operations at the same time. In Java, concurrency is primarily achieved through threads.

### Benefits of Concurrency

1. **Improved Responsiveness**: Keep UI responsive while performing background operations
2. **Increased Throughput**: Process more tasks in the same amount of time
3. **Resource Utilization**: Make better use of system resources, especially in multi-core systems
4. **Simplified Program Design**: Some problems are naturally concurrent

### Challenges of Concurrency

1. **Thread Safety**: Avoiding race conditions and data corruption
2. **Deadlocks**: Preventing threads from blocking each other indefinitely
3. **Starvation**: Ensuring threads get fair access to resources
4. **Complexity**: Managing thread coordination and synchronization

## Threads in Java

A thread is the smallest unit of execution in Java. The Java Virtual Machine (JVM) allows an application to have multiple threads of execution running concurrently.

### Creating and Starting Threads

There are two main ways to create a thread in Java:

#### 1. Extending the Thread Class

```java
public class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread is running: " + Thread.currentThread().getName());
        // Thread logic here
    }
    
    public static void main(String[] args) {
        MyThread thread1 = new MyThread();
        thread1.start(); // Start the thread
    }
}
```

#### 2. Implementing the Runnable Interface

```java
public class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Thread is running: " + Thread.currentThread().getName());
        // Thread logic here
    }
    
    public static void main(String[] args) {
        Thread thread1 = new Thread(new MyRunnable());
        thread1.start(); // Start the thread
    }
}
```

### Thread States

A thread can be in one of six states:
- `NEW`: A thread that has been created but not yet started
- `RUNNABLE`: A thread that is executing or ready to execute
- `BLOCKED`: A thread that is waiting to acquire a monitor lock
- `WAITING`: A thread that is waiting indefinitely for another thread to perform an action
- `TIMED_WAITING`: A thread that is waiting for another thread for a specified period of time
- `TERMINATED`: A thread that has completed execution

```java
Thread thread = new Thread(() -> {
    // Thread logic
});

System.out.println(thread.getState()); // NEW
thread.start();
System.out.println(thread.getState()); // RUNNABLE
```

### Thread Methods

```java
Thread thread = new Thread(() -> {
    // Thread logic
});

thread.start();  // Start the thread
thread.setName("WorkerThread");  // Set thread name
thread.setPriority(Thread.MAX_PRIORITY);  // Set thread priority (1-10)
thread.join();  // Wait for thread to complete
thread.interrupt();  // Interrupt the thread
```

### Thread Sleep and Interruption

```java
public void run() {
    try {
        System.out.println("Going to sleep for 2 seconds");
        Thread.sleep(2000);
        System.out.println("Woke up!");
    } catch (InterruptedException e) {
        System.out.println("I was interrupted!");
        Thread.currentThread().interrupt(); // Re-interrupt the thread
    }
}
```

## Thread Synchronization

When multiple threads access shared resources, synchronization is required to avoid race conditions.

### The Race Condition Problem

```java
public class Counter {
    private int count = 0;
    
    // Without synchronization - leads to race condition
    public void increment() {
        count++; // This is not an atomic operation!
    }
    
    public int getCount() {
        return count;
    }
}
```

### Synchronized Methods

```java
public class Counter {
    private int count = 0;
    
    // With synchronization - thread-safe
    public synchronized void increment() {
        count++;
    }
    
    public synchronized int getCount() {
        return count;
    }
}
```

### Synchronized Blocks

```java
public class Counter {
    private int count = 0;
    private final Object lock = new Object(); // dedicated lock object
    
    public void increment() {
        synchronized(lock) { // synchronize on the lock object
            count++;
        }
    }
    
    public int getCount() {
        synchronized(lock) {
            return count;
        }
    }
}
```

### Volatile Keyword

The `volatile` keyword ensures that a variable is always read from and written to main memory, not from thread-local caches.

```java
public class StatusChecker {
    private volatile boolean running = true;
    
    public void stop() {
        running = false;
    }
    
    public void checkStatus() {
        while (running) {
            // This will see the change when another thread calls stop()
        }
    }
}
```

### Thread Local Variables

`ThreadLocal` allows each thread to have its own copy of a variable.

```java
public class UserContext {
    private static final ThreadLocal<User> userThreadLocal = new ThreadLocal<>();
    
    public static void setUser(User user) {
        userThreadLocal.set(user);
    }
    
    public static User getUser() {
        return userThreadLocal.get();
    }
    
    public static void clear() {
        userThreadLocal.remove();
    }
}
```

## Locks and Atomic Variables

Java 5 introduced the `java.util.concurrent.locks` package with more flexible locking mechanisms.

### ReentrantLock

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Counter {
    private int count = 0;
    private final Lock lock = new ReentrantLock();
    
    public void increment() {
        lock.lock();
        try {
            count++;
        } finally {
            lock.unlock(); // Always unlock in a finally block
        }
    }
    
    public int getCount() {
        lock.lock();
        try {
            return count;
        } finally {
            lock.unlock();
        }
    }
}
```

### ReadWriteLock

```java
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class Cache {
    private final ReadWriteLock rwLock = new ReentrantReadWriteLock();
    private final Map<String, Object> cache = new HashMap<>();
    
    public Object get(String key) {
        rwLock.readLock().lock(); // Multiple threads can read simultaneously
        try {
            return cache.get(key);
        } finally {
            rwLock.readLock().unlock();
        }
    }
    
    public void put(String key, Object value) {
        rwLock.writeLock().lock(); // Only one thread can write at a time
        try {
            cache.put(key, value);
        } finally {
            rwLock.writeLock().unlock();
        }
    }
}
```

### Atomic Variables

Atomic variables provide atomic operations on single values without using locks.

```java
import java.util.concurrent.atomic.AtomicInteger;

public class Counter {
    private final AtomicInteger count = new AtomicInteger(0);
    
    public void increment() {
        count.incrementAndGet(); // Atomic operation
    }
    
    public int getCount() {
        return count.get();
    }
    
    public void addValue(int value) {
        // Atomic compare-and-set loop
        int current;
        do {
            current = count.get();
        } while (!count.compareAndSet(current, current + value));
    }
}
```

## Thread Coordination

### wait(), notify(), and notifyAll()

These methods are used to coordinate threads and must be called within a synchronized block.

```java
public class MessageQueue {
    private final Queue<String> queue = new LinkedList<>();
    private final int capacity;
    
    public MessageQueue(int capacity) {
        this.capacity = capacity;
    }
    
    public synchronized void put(String message) throws InterruptedException {
        while (queue.size() == capacity) {
            wait(); // Wait until there's space in the queue
        }
        queue.add(message);
        notifyAll(); // Notify threads waiting to get a message
    }
    
    public synchronized String take() throws InterruptedException {
        while (queue.isEmpty()) {
            wait(); // Wait until there's a message in the queue
        }
        String message = queue.remove();
        notifyAll(); // Notify threads waiting to put a message
        return message;
    }
}
```

### Condition

Conditions provide a more flexible way to coordinate threads when using explicit locks.

```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class MessageQueue {
    private final Queue<String> queue = new LinkedList<>();
    private final int capacity;
    private final Lock lock = new ReentrantLock();
    private final Condition notFull = lock.newCondition();
    private final Condition notEmpty = lock.newCondition();
    
    public MessageQueue(int capacity) {
        this.capacity = capacity;
    }
    
    public void put(String message) throws InterruptedException {
        lock.lock();
        try {
            while (queue.size() == capacity) {
                notFull.await(); // Wait until there's space in the queue
            }
            queue.add(message);
            notEmpty.signalAll(); // Signal threads waiting to get a message
        } finally {
            lock.unlock();
        }
    }
    
    public String take() throws InterruptedException {
        lock.lock();
        try {
            while (queue.isEmpty()) {
                notEmpty.await(); // Wait until there's a message in the queue
            }
            String message = queue.remove();
            notFull.signalAll(); // Signal threads waiting to put a message
            return message;
        } finally {
            lock.unlock();
        }
    }
}
```

### CountDownLatch

A `CountDownLatch` allows one or more threads to wait until a set of operations in other threads completes.

```java
import java.util.concurrent.CountDownLatch;

public class WorkerManager {
    public void executeTask() throws InterruptedException {
        int workerCount = 5;
        CountDownLatch latch = new CountDownLatch(workerCount);
        
        for (int i = 0; i < workerCount; i++) {
            int workerId = i;
            new Thread(() -> {
                try {
                    // Perform some work
                    System.out.println("Worker " + workerId + " is working");
                    Thread.sleep((long) (Math.random() * 1000));
                    System.out.println("Worker " + workerId + " is done");
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                } finally {
                    latch.countDown(); // Signal that this worker is done
                }
            }).start();
        }
        
        latch.await(); // Wait for all workers to finish
        System.out.println("All workers have completed their tasks");
    }
}
```

### CyclicBarrier

A `CyclicBarrier` allows a set of threads to wait for each other to reach a common barrier point.

```java
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class ParallelProcessor {
    public void process() {
        int threadCount = 3;
        CyclicBarrier barrier = new CyclicBarrier(threadCount, () -> {
            // This runs when all threads reach the barrier
            System.out.println("All threads have reached the barrier, proceeding to next phase");
        });
        
        for (int i = 0; i < threadCount; i++) {
            int threadId = i;
            new Thread(() -> {
                try {
                    System.out.println("Thread " + threadId + " is working on phase 1");
                    Thread.sleep((long) (Math.random() * 1000));
                    
                    barrier.await(); // Wait for all threads to complete phase 1
                    
                    System.out.println("Thread " + threadId + " is working on phase 2");
                    Thread.sleep((long) (Math.random() * 1000));
                    
                    barrier.await(); // Wait for all threads to complete phase 2
                    
                    System.out.println("Thread " + threadId + " is working on phase 3");
                } catch (InterruptedException | BrokenBarrierException e) {
                    Thread.currentThread().interrupt();
                }
            }).start();
        }
    }
}
```

### Semaphore

A `Semaphore` controls access to a shared resource through a counter.

```java
import java.util.concurrent.Semaphore;

public class ResourcePool {
    private final Semaphore semaphore;
    private final List<Resource> resources = new ArrayList<>();
    
    public ResourcePool(int maxResources) {
        semaphore = new Semaphore(maxResources, true); // fair = true
        for (int i = 0; i < maxResources; i++) {
            resources.add(new Resource(i));
        }
    }
    
    public Resource acquire() throws InterruptedException {
        semaphore.acquire(); // Acquire a permit
        return getResource();
    }
    
    public void release(Resource resource) {
        if (returnResource(resource)) {
            semaphore.release(); // Release a permit
        }
    }
    
    private synchronized Resource getResource() {
        for (Resource resource : resources) {
            if (!resource.isInUse()) {
                resource.setInUse(true);
                return resource;
            }
        }
        return null; // Should never reach here if semaphore is used correctly
    }
    
    private synchronized boolean returnResource(Resource resource) {
        if (resource.isInUse()) {
            resource.setInUse(false);
            return true;
        }
        return false;
    }
    
    private static class Resource {
        private final int id;
        private boolean inUse;
        
        public Resource(int id) {
            this.id = id;
            this.inUse = false;
        }
        
        public boolean isInUse() {
            return inUse;
        }
        
        public void setInUse(boolean inUse) {
            this.inUse = inUse;
        }
        
        @Override
        public String toString() {
            return "Resource #" + id;
        }
    }
}
```

## Executors and Thread Pools

The `java.util.concurrent` package provides high-level APIs for thread management through executors and thread pools.

### ExecutorService

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class TaskManager {
    public void executeTasksWithFixedThreadPool() {
        ExecutorService executor = Executors.newFixedThreadPool(5);
        
        for (int i = 0; i < 10; i++) {
            final int taskId = i;
            executor.submit(() -> {
                System.out.println("Task " + taskId + " is running on thread " + 
                                   Thread.currentThread().getName());
                // Task logic here
                return "Result of task " + taskId;
            });
        }
        
        // Shutdown executor gracefully
        executor.shutdown();
        try {
            if (!executor.awaitTermination(1, TimeUnit.MINUTES)) {
                executor.shutdownNow();
            }
        } catch (InterruptedException e) {
            executor.shutdownNow();
            Thread.currentThread().interrupt();
        }
    }
}
```

### Thread Pool Types

```java
// Fixed thread pool - contains a fixed number of threads
ExecutorService fixedPool = Executors.newFixedThreadPool(5);

// Cached thread pool - creates new threads as needed, reuses idle threads
ExecutorService cachedPool = Executors.newCachedThreadPool();

// Single thread executor - uses a single worker thread
ExecutorService singleThreadExecutor = Executors.newSingleThreadExecutor();

// Scheduled thread pool - can schedule tasks to run after a delay or periodically
ScheduledExecutorService scheduledPool = Executors.newScheduledThreadPool(3);
```

### Scheduled Tasks

```java
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class ScheduledTaskManager {
    public void schedulePeriodicTasks() {
        ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(1);
        
        // Run once after a delay
        scheduler.schedule(() -> {
            System.out.println("Delayed task executed");
        }, 2, TimeUnit.SECONDS);
        
        // Run periodically (first execution after delay)
        scheduler.scheduleAtFixedRate(() -> {
            System.out.println("Fixed rate task executed");
        }, 1, 5, TimeUnit.SECONDS);
        
        // Run periodically (subsequent executions after completion)
        scheduler.scheduleWithFixedDelay(() -> {
            System.out.println("Fixed delay task executed");
        }, 0, 2, TimeUnit.SECONDS);
        
        // Let the scheduler run for a while before shutting down
        try {
            Thread.sleep(30000);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        } finally {
            scheduler.shutdown();
        }
    }
}
```

### Future and CompletableFuture

`Future` represents the result of an asynchronous computation.

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;
import java.util.concurrent.TimeUnit;

public class AsyncTaskManager {
    public void executeFutureTasks() throws Exception {
        ExecutorService executor = Executors.newFixedThreadPool(2);
        
        // Submit a task that returns a result
        Future<Integer> future = executor.submit(() -> {
            Thread.sleep(2000); // Simulate long computation
            return 42;
        });
        
        // Do other work while waiting for the result
        System.out.println("Doing other work...");
        
        // Get the result (blocks until available)
        Integer result = future.get(3, TimeUnit.SECONDS);
        System.out.println("Result: " + result);
        
        executor.shutdown();
    }
}
```

`CompletableFuture` extends the capabilities of `Future` with a rich set of methods for composing asynchronous tasks.

```java
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class CompletableFutureExample {
    public void demonstrateCompletableFuture() {
        ExecutorService executor = Executors.newFixedThreadPool(2);
        
        // Create and run a CompletableFuture
        CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
            return "Hello";
        }, executor);
        
        // Chain operations
        CompletableFuture<String> finalFuture = future
            .thenApply(s -> s + " World")  // Transform the result
            .thenCompose(s -> CompletableFuture.supplyAsync(() -> s + "!"))  // Chain another CF
            .thenCombine(CompletableFuture.supplyAsync(() -> " Have a nice day!"), 
                         (s1, s2) -> s1 + s2)  // Combine with another CF
            .exceptionally(ex -> "Error occurred: " + ex.getMessage());  // Handle exceptions
        
        // Add a completion handler
        finalFuture.thenAccept(result -> System.out.println("Final result: " + result));
        
        // Block and get the result
        try {
            String result = finalFuture.get();
            System.out.println("Result obtained: " + result);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            executor.shutdown();
        }
    }
}
```

## Concurrent Collections

Java provides a set of thread-safe collections in the `java.util.concurrent` package.

### ConcurrentHashMap

```java
import java.util.concurrent.ConcurrentHashMap;

public class UserCache {
    private final ConcurrentHashMap<String, User> userCache = new ConcurrentHashMap<>();
    
    public User getUser(String userId) {
        return userCache.get(userId);
    }
    
    public void addUser(String userId, User user) {
        userCache.put(userId, user);
    }
    
    public User getOrCreateUser(String userId, String name) {
        // computeIfAbsent atomically computes a value if key is absent
        return userCache.computeIfAbsent(userId, id -> new User(id, name));
    }
    
    public void processAllUsers() {
        // ForEach operations are safe and potentially parallel
        userCache.forEach((id, user) -> {
            System.out.println("Processing user: " + user.getName());
            // Process user logic
        });
    }
}
```

### ConcurrentLinkedQueue

```java
import java.util.concurrent.ConcurrentLinkedQueue;

public class MessageProcessor {
    private final ConcurrentLinkedQueue<String> messageQueue = new ConcurrentLinkedQueue<>();
    
    public void addMessage(String message) {
        messageQueue.add(message);
    }
    
    public String pollMessage() {
        return messageQueue.poll(); // Returns null if empty
    }
    
    public void processAllMessages() {
        String message;
        while ((message = messageQueue.poll()) != null) {
            System.out.println("Processing message: " + message);
            // Process message logic
        }
    }
}
```

### BlockingQueue

```java
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.TimeUnit;

public class ProducerConsumerExample {
    private final BlockingQueue<String> queue = new ArrayBlockingQueue<>(10);
    
    public void producer() throws InterruptedException {
        int count = 0;
        while (count < 20) {
            String message = "Message-" + count;
            queue.put(message); // Blocks if queue is full
            System.out.println("Produced: " + message);
            count++;
            Thread.sleep((long) (Math.random() * 500));
        }
    }
    
    public void consumer() throws InterruptedException {
        while (true) {
            String message = queue.poll(5, TimeUnit.SECONDS); // Blocks until timeout
            if (message == null) {
                System.out.println("No more messages, exiting consumer");
                break;
            }
            System.out.println("Consumed: " + message);
            Thread.sleep((long) (Math.random() * 1000));
        }
    }
    
    public void run() {
        Thread producerThread = new Thread(() -> {
            try {
                producer();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });
        
        Thread consumerThread = new Thread(() -> {
            try {
                consumer();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });
        
        producerThread.start();
        consumerThread.start();
        
        try {
            producerThread.join();
            consumerThread.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}
```

### CopyOnWriteArrayList

```java
import java.util.concurrent.CopyOnWriteArrayList;

public class EventListenerManager {
    private final CopyOnWriteArrayList<EventListener> listeners = new CopyOnWriteArrayList<>();
    
    public void addListener(EventListener listener) {
        listeners.add(listener);
    }
    
    public void removeListener(EventListener listener) {
        listeners.remove(listener);
    }
    
    public void fireEvent(Event event) {
        // Safe to iterate while other threads may be modifying the list
        for (EventListener listener : listeners) {
            listener.onEvent(event);
        }
    }
    
    public interface EventListener {
        void onEvent(Event event);
    }
    
    public static class Event {
        private final String type;
        
        public Event(String type) {
            this.type = type;
        }
        
        public String getType() {
            return type;
        }
    }
}
```

## Thread-Safe Design Patterns

### Immutable Objects

Immutable objects are inherently thread-safe because their state cannot be changed after construction.

```java
public final class ImmutablePerson {
    private final String name;
    private final int age;
    private final List<String> hobbies;
    
    public ImmutablePerson(String name, int age, List<String> hobbies) {
        this.name = name;
        this.age = age;
        // Create a defensive copy of mutable objects
        this.hobbies = new ArrayList<>(hobbies);
    }
    
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
    
    public List<String> getHobbies() {
        // Return a defensive copy to prevent modification
        return new ArrayList<>(hobbies);
    }
    
    // No setters allowed
}
```

### Thread Confinement

Thread confinement restricts access to shared data to a single thread.

```java
public class ThreadLocalExample {
    // Each thread has its own copy of userContext
    private static final ThreadLocal<UserContext> userContext = new ThreadLocal<>();
    
    public static void setUserContext(UserContext context) {
        userContext.set(context);
    }
    
    public static UserContext getUserContext() {
        return userContext.get();
    }
    
    public static void processRequest() {
        UserContext context = getUserContext();
        if (context != null) {
            // Process the request in the context of the current user
            System.out.println("Processing request for user: " + context.getUsername());
        }
    }
    
    public static void clearContext() {
        userContext.remove(); // Important to prevent memory leaks
    }
    
    public static class UserContext {
        private final String username;
        
        public UserContext(String username) {
            this.username = username;
        }
        
        public String getUsername() {
            return username;
        }
    }
}
```

### Producer-Consumer Pattern

```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

public class ProducerConsumerPattern {
    private static class Task {
        private final int id;
        
        public Task(int id) {
            this.id = id;
        }
        
        public int getId() {
            return id;
        }
        
        @Override
        public String toString() {
            return "Task #" + id;
        }
    }
    
    private static class Producer implements Runnable {
        private final BlockingQueue<Task> queue;
        private final int count;
        
        public Producer(BlockingQueue<Task> queue, int count) {
            this.queue = queue;
            this.count = count;
        }
        
        @Override
        public void run() {
            try {
                for (int i = 0; i < count; i++) {
                    Task task = new Task(i);
                    queue.put(task);
                    System.out.println("Produced " + task);
                    Thread.sleep((long) (Math.random() * 100));
                }
                
                // Signal end of production with poison pill
                queue.put(new Task(-1));
                System.out.println("Producer finished");
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
    
    private static class Consumer implements Runnable {
        private final BlockingQueue<Task> queue;
        
        public Consumer(BlockingQueue<Task> queue) {
            this.queue = queue;
        }
        
        @Override
        public void run() {
            try {
                while (true) {
                    Task task = queue.take();
                    
                    // Check for poison pill
                    if (task.getId() == -1) {
                        System.out.println("Consumer received end signal");
                        // Put it back for other consumers
                        queue.put(task);
                        break;
                    }
                    
                    System.out.println("Consumed " + task);
                    Thread.sleep((long) (Math.random() * 200));
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
    
    public static void main(String[] args) {
        BlockingQueue<Task> queue = new LinkedBlockingQueue<>(5); // Bounded queue
        
        // Create and start producer
        Thread producerThread = new Thread(new Producer(queue, 10));
        producerThread.start();
        
        // Create and start consumers
        Thread consumer1 = new Thread(new Consumer(queue));
        Thread consumer2 = new Thread(new Consumer(queue));
        consumer1.start();
        consumer2.start();
        
        // Wait for all threads to complete
        try {
            producerThread.join();
            consumer1.join();
            consumer2.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        
        System.out.println("All threads have finished");
    }
}
```

## Common Concurrency Issues

### Deadlock

A deadlock occurs when two or more threads are blocked forever, each waiting for the other to release a lock.

```java
public class DeadlockExample {
    private final Object lock1 = new Object();
    private final Object lock2 = new Object();
    
    public void method1() {
        synchronized (lock1) {
            System.out.println("Thread 1: Holding lock 1...");
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
            System.out.println("Thread 1: Waiting for lock 2...");
            synchronized (lock2) {
                System.out.println("Thread 1: Holding lock 1 & 2...");
            }
        }
    }
    
    public void method2() {
        synchronized (lock2) {
            System.out.println("Thread 2: Holding lock 2...");
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
            System.out.println("Thread 2: Waiting for lock 1...");
            synchronized (lock1) {
                System.out.println("Thread 2: Holding lock 1 & 2...");
            }
        }
    }
    
    // To avoid deadlock, ensure consistent lock ordering:
    public void safeMethod1() {
        synchronized (lock1) {
            synchronized (lock2) {
                // Safe code here
            }
        }
    }
    
    public void safeMethod2() {
        synchronized (lock1) {
            synchronized (lock2) {
                // Safe code here
            }
        }
    }
}
```

### Livelock

A livelock occurs when threads keep changing their state in response to another thread's action, without making any real progress.

```java
public class LivelockExample {
    static class Spoon {
        private Person owner;
        
        public Spoon(Person owner) {
            this.owner = owner;
        }
        
        public Person getOwner() {
            return owner;
        }
        
        public void setOwner(Person owner) {
            this.owner = owner;
        }
        
        public synchronized void use() {
            System.out.println(owner.getName() + " is eating!");
        }
    }
    
    static class Person {
        private String name;
        private boolean isHungry;
        
        public Person(String name) {
            this.name = name;
            this.isHungry = true;
        }
        
        public String getName() {
            return name;
        }
        
        public boolean isHungry() {
            return isHungry;
        }
        
        public void eatWith(Spoon spoon, Person spouse) {
            while (isHungry) {
                // Don't have the spoon, so check if spouse is using it
                if (spoon.getOwner() != this) {
                    try {
                        Thread.sleep(1);
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                        continue;
                    }
                    continue;
                }
                
                // If spouse is hungry, insist upon passing the spoon
                if (spouse.isHungry) {
                    System.out.println(name + ": You eat first " + spouse.getName());
                    spoon.setOwner(spouse);
                    continue;
                }
                
                // Spouse wasn't hungry, so finally eat
                spoon.use();
                isHungry = false;
                System.out.println(name + ": I am no longer hungry!");
                spoon.setOwner(spouse);
            }
        }
    }
    
    public static void main(String[] args) {
        final Person husband = new Person("Bob");
        final Person wife = new Person("Alice");
        
        final Spoon spoon = new Spoon(husband);
        
        new Thread(() -> husband.eatWith(spoon, wife)).start();
        new Thread(() -> wife.eatWith(spoon, husband)).start();
    }
}
```

### Starvation

Starvation occurs when a thread is denied access to resources it needs to progress.

```java
public class StarvationExample {
    public static void main(String[] args) {
        final Object lock = new Object();
        
        // Start greedy threads that hold the lock for long periods
        for (int i = 0; i < 5; i++) {
            final int id = i;
            new Thread(() -> {
                while (true) {
                    synchronized (lock) {
                        System.out.println("Greedy thread " + id + " got the lock");
                        try {
                            Thread.sleep(1000); // Hold lock for a long time
                        } catch (InterruptedException e) {
                            Thread.currentThread().interrupt();
                            break;
                        }
                    }
                    
                    try {
                        Thread.sleep(10); // Small pause before trying again
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                        break;
                    }
                }
            }).start();
        }
        
        // Start a polite thread that gets starved
        new Thread(() -> {
            while (true) {
                synchronized (lock) {
                    System.out.println("Polite thread finally got the lock!");
                    // Just release it immediately
                }
                
                try {
                    Thread.sleep(100); // Polite waiting time
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                    break;
                }
            }
        }).start();
    }
}
```

## Best Practices for Concurrent Programming

1. **Minimize Shared Mutable State**: Use immutable objects when possible. When state must be shared and mutable, ensure proper synchronization.

2. **Use Higher-Level Concurrency Utilities**: Prefer `java.util.concurrent` utilities over low-level synchronization.

3. **Avoid Blocking Operations in Critical Sections**: Keep synchronized blocks small and focused.

4. **Prefer Concurrent Collections**: Use thread-safe collections like `ConcurrentHashMap` instead of synchronized wrappers.

5. **Document Thread Safety**: Clearly document the thread safety guarantees of your classes.

6. **Follow Consistent Locking Order**: To prevent deadlocks, always acquire locks in a consistent order.

7. **Consider Thread-Local Variables**: Use `ThreadLocal` for per-thread state to avoid synchronization.

8. **Prefer Atomic Variables Over Locks**: For single variables, atomic classes provide better performance.

9. **Use Thread Pools**: Use executor services instead of creating threads directly.

10. **Proper Exception Handling**: Always catch and handle `InterruptedException` by either rethrowing or restoring the interrupted status.

## Testing Concurrent Code

### Using TimUnit for Testing Timing

```java
import java.util.concurrent.TimeUnit;

// Wait with timeout
boolean completed = latch.await(5, TimeUnit.SECONDS);
```

### Testing with JUnit and Awaitility

```java
import org.junit.Test;
import java.util.concurrent.CountDownLatch;
import java.util.concurrent.TimeUnit;
import static org.junit.Assert.*;

public class ConcurrentTest {
    @Test
    public void testConcurrentExecution() throws InterruptedException {
        final CountDownLatch latch = new CountDownLatch(1);
        final AtomicBoolean executed = new AtomicBoolean(false);
        
        new Thread(() -> {
            try {
                Thread.sleep(500);
                executed.set(true);
                latch.countDown();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }).start();
        
        boolean completed = latch.await(2, TimeUnit.SECONDS);
        assertTrue("Task did not complete in time", completed);
        assertTrue("Task did not execute correctly", executed.get());
    }
}
```

Using Awaitility:
```java
import static org.awaitility.Awaitility.*;
import static java.util.concurrent.TimeUnit.*;

@Test
public void testAsyncOperation() {
    // Start async operation
    asyncService.performOperation();
    
    // Wait until condition is met with timeout
    await().atMost(5, SECONDS).until(() -> asyncService.isOperationComplete());
    
    // Assert final state
    assertTrue(asyncService.getResult() > 0);
}
```

## Practice Exercises

1. Implement a thread-safe cache with timeout capabilities.
2. Create a bounded buffer with multiple producers and consumers.
3. Design a simple thread pool implementation.
4. Implement a concurrent web crawler that respects site rate limits.
5. Create a thread-safe object pool with size limits and timeout.

## Next Steps

- Learn about [Advanced Java Concurrency Features](/concurrency/advanced.md)
- Explore [Reactive Programming with Java](/reactive-programming/README.md)
- Study [Parallel Streams and Functional Programming](/functional-programming/README.md)