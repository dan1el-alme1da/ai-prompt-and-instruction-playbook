
-----

## Introduction to Virtual Threads in Java

**Virtual Threads** are a new feature introduced by **Project Loom**, which became stable in **Java 21**. They are threads that are much **lighter** than traditional ones, created and managed by the **JVM** (Java Virtual Machine).

The core idea is simple: you can create **thousands of virtual threads** (while respecting your system's resources, of course) without worrying about freezing your application, because they **do not map 1:1** to operating system (OS) threads.

-----

## Key Differences: Virtual Thread vs. Traditional (Platform) Thread

| Characteristic | Platform Thread (Traditional) | Virtual Thread |
| :--- | :--- | :--- |
| **Mapping** | A "wrapper" around a **real OS thread** (1:1 mapping). | Scheduled by the **JVM** on top of a small **pool of platform threads**. |
| **Resource Use** | Creating many consumes a lot of **memory and resources**. | Allows you to have **100,000 virtual threads** running on top of just a few native threads, without losing performance in **I/O** scenarios. |

### What Happens When a Virtual Thread Blocks?

This is a critical point: when a virtual thread performs **blocking I/O**, the **JVM suspends that virtual thread** and **releases the underlying native thread** so that another virtual thread can use it.

In summary, you can have **thousands of threads** waiting for an **I/O** response without tying up **OS resources**. This greatly reduces the complexity of dealing with concurrency.

-----

## Usage and Application

**Virtual Threads do not replace** traditional threads. They are designed to solve a specific problem: **concurrency in I/O tasks** (such as HTTP calls, file reading, and database queries).

  * **Recommended for:** I/O-bound tasks, which spend most of their time waiting.
  * **Less Effective for:** Heavy **CPU-bound** tasks (intensive calculations, compression, cryptography), where using virtual threads does not offer a significant difference in performance.

-----

## Implementation and Code Examples

### 1\. How to Create a Virtual Thread in Java

It's quite simple, using the new API `Thread.ofVirtual()`:

```java
Thread.startVirtualThread(() -> {
    System.out.println("Running in a virtual thread!");
});
```

### 2\. Example with ExecutorService

The most practical way to use Virtual Threads with an **ExecutorService** is to use the new executor:

```java
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
executor.submit(() -> System.out.println("Executing on the executor!"));
}
```

This executor creates a **virtual thread for every submitted task**. It is recommended for **web servers** or **queue processors** that handle a high volume of concurrent I/O-bound work.