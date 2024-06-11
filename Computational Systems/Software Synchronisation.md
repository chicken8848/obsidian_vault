# Multiple Producer Multiple Consumer
Multiple Producer-Multiple Consumer (MPMC) refers to a synchronization problem where multiple producers and consumers share a shared resource, such as a buffer or queue. In this scenario:

⦁ Producers: These are entities that generate data and add it to the shared resource.
⦁ Consumers: These are entities that consume data from the shared resource.

The challenge arises when there is more than one producer adding data to the shared resource simultaneously, while also having multiple consumers removing data from the same resource. This can lead to issues such as:

1. Data corruption or loss due to concurrent access and modification.
2. Starvation: One consumer might not get a chance to consume data because another consumer is holding onto it for too long.

Producer program:
```c
void send (char c) {
	wait(space); // check, try reduce space "resource"
	wait(mutex_p);
	// critical section start
	buf[in] = c;
	in = (in + 1) % N;
	// critical section end
	signal(mutex_p);
	signal(chars); // increment char resource so consumer can run rcv()
}
```
Consumer program:
```c
char rcv(){
	char c;
	wait(chars);
	wait(mutex_c);
	// critical section start
	c = buf[out];
	out = (out+1)%N;
	// critical section end
	signal(mutex_c);
	signal(space);
}
```

# Race Condition
A **race condition** is a situation where two or more threads (or processes) are competing for shared resources in such a way that the outcome depends on the order in which they access these resources. In other words, it's when multiple threads try to modify shared data simultaneously, and the result of their actions depends on who gets there first.

# Semaphores
A **semaphore** is a synchronization mechanism used in operating systems and computer programming. It's a variable or abstract data type that can be either 0 (zero) or greater than zero. Usually an `int`.

There are two types of semaphores:
1. Binary Semaphore: Also known as a "mutex" (short for "mutual exclusion"), this type of semaphore allows either 0 or 1 process to access the shared resource.
2. Counting Semaphore (or "counting mutex"): This type of semaphore allows up to a certain number of processes to access the shared resource simultaneously.

## Bounded Waiting
To achieve bounded waiting with semaphores, you can use counting semaphores that allow a certain number of processes or threads to be in the critical section at any given time. This way, even if multiple processes are trying to access the same resource, only a limited number will actually get in and wait for their turn:
1. A process or thread tries to acquire the semaphore.
2. If there is already another process or thread in the critical section (i.e., holding onto the semaphore), it waits until one of them releases the semaphore.
3. The maximum number of processes or threads that can be in the critical section at any given time is bounded, ensuring that no single process will have to wait indefinitely.
```c
wait(semaphore *S)
{  S->value--;
   if (S->value < 0)
   {
       add this process to S->list; // this will call block()
   }
}
```

```c
signal(semaphore *S)
{
   S->value++;
   if (S->value <= 0)
   {
       remove a process P from S->list;
       wakeup(P);
   }
}
```
# Mutex vs Spinlock
In a nutshell, both mutex (short for "mutual exclusion") and spin lock are synchronization primitives used in concurrent programming to ensure exclusive access to shared resources. However, they differ in their implementation, behavior, and use cases:

## Mutex Lock:

A mutex lock is a type of locking mechanism that allows only one thread to access a critical section of code at a time. When a thread acquires a mutex lock, it blocks until the lock is released by another thread.

Key characteristics:

⦁ Blocking: If multiple threads try to acquire the same mutex lock simultaneously, one will block and wait for the others to release their locks.
⦁ Non-spinning: The acquiring thread does not continuously poll or spin (i.e., waste CPU cycles) while waiting for the lock. Instead, it yields control back to the scheduler.

In C++, the standard library provides two types of mutexes: `std::mutex` and `std::timed_mutex`. The main difference between them is how they handle blocking:

1. `std::mutex`: This type of mutex does not have a timeout, which means that if a thread tries to acquire the lock but it's already held by another thread, the calling thread will block indefinitely until the lock becomes available.
2. `std::timed_mutex`: This type of mutex has a timeout, which allows you to specify how long the calling thread should wait before giving up and returning an error.

Here are some key methods for working with `std::mutex`:

⦁   `lock()`: Attempts to acquire the lock. If another thread already holds the lock, this method will block until the lock becomes available.
⦁   `unlock()`: Releases the lock held by the calling thread.
⦁   `try_lock()`: Tries to acquire the lock without blocking. Returns `true` if successful and `false` otherwise.

Here's an example of how you might use a mutex in C++:
```c
#include <iostream>
#include <mutex>

std::mutex mtx;

void printSomething() {
    std::lock_guard<std::mutex> lock(mtx);
    for (int i = 0; i < 10; ++i) {
        std::cout << "Hello from thread " << std::this_thread::get_id() << "! \n";
        // Simulate some work
        std::this_thread::sleep_for(std::chrono::milliseconds(100));
    }
}

int main() {
    for (int i = 0; i < 5; ++i) {
        std::thread t(printSomething);
        t.join();
    }

    return 0;
}
```

In this example, the `printSomething` function is designed to be thread-safe by using a lock guard `std::lock_guard` to acquire and release the mutex. This ensures that only one thread can execute the code inside the lock guard at any given time.

When you run this program, each thread will print `"Hello from thread X!"` 10 times, with some delay between prints. The mutex ensures that no two threads try to access the shared output stream simultaneously, preventing potential issues like garbled or missing output.

## Spin Lock:

A spin lock is a type of locking mechanism that allows one thread to access a critical section of code at a time, but with some key differences:

⦁ Non-blocking: When multiple threads try to acquire the same spin lock simultaneously, they will continuously poll or "spin" until the lock becomes available.
⦁ Fast path: Spin locks are designed for low-latency scenarios where the cost of context switching is high. They allow threads to quickly check and re-check if the lock has been released.

Key differences:

1. Blocking vs. Non-Blocking: Mutex locks block, while spin locks do not.
2. Polling vs. Yielding: Spin locks continuously poll (spin) until the lock becomes available, whereas mutex locks yield control back to the scheduler.
3. Use Cases:
   ⦁ Use mutex when you need to ensure exclusive access to a shared resource and don't care about latency or performance overhead.
   ⦁ Use spin lock when you need low-latency synchronization (e.g., in real-time systems, embedded systems, or high-performance computing) and can tolerate the additional CPU usage.

In summary:

⦁ Mutex locks are suitable for most concurrency control scenarios where blocking is acceptable.
⦁ Spin locks are better suited for situations where low latency and non-blocking behavior are crucial.

# Condition Variables
Condition variables allow a process or thread to wait for completion of a given **event** on a particular object (some shared state, data structure, anything). It is used to **communicate** between processes or threads when certain conditions become `true`.

The “event” is the _change_ in state of some condition that thread is interested in. Until that is satisfied, the process waits to be awakened later by a signalling process/thread (that actually **changes** the condition).
```c
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
```

```c
pthread_cond_t p_cond_x = PTHREAD_COND_INITIALIZER;
```

```c
pthread_mutex_lock(&mutex);
// CRITICAL SECTION
// ...
cond_x = true; // your own boolean
pthread_cond_signal(&p_cond_x);
pthread_mutex_unlock(&mutex);
```

```c
pthread_mutex_lock(&mutex);
// always wait in a while loop
while (cond_x == false){
   pthread_cond_wait(&p_cond_x, &mutex);  // yields mutex, sleeping, when woken, lock is acquired automatically
}
// CRITICAL SECTION, can only be executed iff cond_x == true
// ...
pthread_mutex_unlock(&mutex);
```

# Synchronisation in Java
In Java, a `synchronized` method or block of code is used to ensure that only one thread can execute it at any given time. This is achieved by acquiring an exclusive lock on an object before executing the `synchronized` code.

When a thread tries to enter a synchronized method or block, it must first acquire the lock associated with the object on which the method or block is declared. If another thread already holds this lock, then the current thread will be blocked until the lock becomes available.

Here are some key points about synchronized methods in Java:

1. Synchronization: The primary purpose of a synchronized method is to ensure that only one thread can execute it at any given time.
2. Locking: When a thread enters a synchronized method, it must acquire the lock associated with the object on which the method is declared.
3. Blocking: If another thread already holds this lock, then the current thread will be blocked until the lock becomes available.
4. Object-level locking: In Java, each object has its own lock, and a `synchronized` method or block only acquires the lock associated with that specific object.

To illustrate this concept, let's consider an example:

Suppose we have two threads, A and B, which are trying to access a shared resource (e.g., a bank account) using a `synchronized` method:
```java
public class BankAccount {
    private int balance = 0;

    public synchronized void deposit(int amount) {
        balance += amount;
    }

    public synchronized void withdraw(int amount) {
        if (balance >= amount) {
            balance -= amount;
        } else {
            throw new InsufficientFundsException();
        }
    }
}
```

In this example, the deposit and withdraw methods are declared as `synchronized`, which means that only one thread can execute these methods at any given time. If another thread tries to access these methods while they're already being executed by another thread, it will be blocked until the lock becomes available.

The benefits of using `synchronized` methods include:

1. Thread safety: By ensuring that only one thread can execute a method or block of code at any given time, we can prevent data inconsistencies and ensure that shared resources are accessed safely.
2. Preventing deadlocks: Synchronization helps to prevent deadlocks by ensuring that threads do not get stuck in an infinite loop of waiting for each other.

However, it's essential to note that excessive use of synchronized methods can lead to performance issues due to the overhead of acquiring and releasing locks. Therefore, it's crucial to carefully consider the synchronization requirements for your application and use alternative concurrency mechanisms (e.g., Java 5's AtomicInteger) when possible.