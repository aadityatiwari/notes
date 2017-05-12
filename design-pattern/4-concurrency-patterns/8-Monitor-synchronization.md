[Monitor (synchronization)](https://en.wikipedia.org/wiki/Monitor_(synchronization))
------------------

A **monitor** is a synchronization construct that allows [threads](https://en.wikipedia.org/wiki/Thread_(computing)) to have both [mutual exclusion](https://en.wikipedia.org/wiki/Mutual_exclusion) and the ability to wait (block) for a certain condition to become true.

Monitors also have a mechanism for signaling other threads that their condition has been met. 

A monitor consists of a [mutex (lock)](https://en.wikipedia.org/wiki/Lock_(computer_science)) object and **condition variables**.

A **condition variable** is basically a container of threads that are waiting for a certain condition.

Monitors provide a mechanism for threads to temporarily give up exclusive access in order to wait for some condition to be met, before regaining exclusive access and resuming their task.

**Monitor** is a **thread-safe** [class](https://en.wikipedia.org/wiki/Class_(computer_science)), [object](https://en.wikipedia.org/wiki/Object_(computer_science)), or [module](https://en.wikipedia.org/wiki/Module_(programming)) that uses wrapped [mutual exclusion](https://en.wikipedia.org/wiki/Mutual_exclusion) in order to safely allow access to a method or variable by more than one [thread](https://en.wikipedia.org/wiki/Thread_(computer_science)). 

The defining characteristic of a monitor is that its methods are executed with [mutual exclusion](https://en.wikipedia.org/wiki/Mutual_exclusion)

By using one or more condition variables it can also provide the ability for threads to wait on a certain condition

"monitor" can be referred as a "thread-safe object/class/module"

```java
class Account {
  private lock myLock

  private int balance := 0
  invariant balance >= 0

  public method boolean withdraw(int amount)
     precondition amount >= 0
  {
    myLock.acquire()
    try {
      if balance < amount {
        return false
      } else {
        balance := balance - amount
        return true
      }
    } finally {
      myLock.release()
    }
  }

  public method deposit(int amount)
     precondition amount >= 0
  {
    myLock.acquire()
    try {
      balance := balance + amount
    } finally {
      myLock.release()
    }
  }
}
```

While a thread is executing a method of a thread-safe object, it is said to *occupy* the object, by holding its [mutex (lock)](https://en.wikipedia.org/wiki/Lock_(computer_science)).

Thread-safe objects are implemented to enforce that *at each point in time; at most one thread may occupy the object*.

The lock, which is initially unlocked, is locked at the start of each public method, and is unlocked at each return from each public method.

An example of classic concurrency problem of the **bounded producer/consumer,** using naïve approach to achieve synchronization using **“spin-waiting”**, in which a mutex is used to protect critical section and busy-waiting is still used, with the lock being acquired and released in between each busy-wait check.


```java
global RingBuffer queue; // A thread-unsafe ring-buffer of tasks.
global Lock queueLock; // A mutex for the ring-buffer of tasks.

// Method representing each producer thread's behavior:
public method producer(){
    while(true){
        task myTask=...; // Producer makes some new task to be added.

        queueLock.acquire(); // Acquire lock for initial busy-wait check.
        while(queue.isFull()){ // Busy-wait until the queue is non-full.
            queueLock.release();
            // Drop the lock temporarily to allow a chance for other threads
            // needing queueLock to run so that a consumer might take a task.
            queueLock.acquire(); // Re-acquire the lock for the next call to "queue.isFull()".
        }

        queue.enqueue(myTask); // Add the task to the queue.
        queueLock.release(); // Drop the queue lock until we need it again to add the next task.
    }
}

// Method representing each consumer thread's behavior:
public method consumer(){
    while(true){
        queueLock.acquire(); // Acquire lock for initial busy-wait check.
        while (queue.isEmpty()){ // Busy-wait until the queue is non-empty.
            queueLock.release();
            // Drop the lock temporarily to allow a chance for other threads
            // needing queueLock to run so that a producer might add a task.
            queueLock.acquire(); // Re-acquire the lock for the next call to "queue.isEmpty()".
        }
        myTask=queue.dequeue(); // Take a task off of the queue.
        queueLock.release(); // Drop the queue lock until we need it again to take off the next task.
        doStuff(myTask); // Go off and do something with the task.
    }
}
```

This method assures that an inconsistent state does not occur, but wastes CPU resources due to the unnecessary busy-waiting.

What is needed is a way to make producer threads block until the queue is non-full, and a way to make consumer threads block until the queue is non-empty.


**Condition variables**
-------------------

For many applications, mutual exclusion is not enough. Threads attempting an operation may need to wait until some condition *P* holds true.

A busy waiting loop "**while not( P ) do skip**" will not work, as mutual exclusion will prevent any other thread from entering the monitor to make the condition true.

What is needed is a way to signal the thread when the condition P is true (or *could* be true).

A condition variable is a queue of threads, associated with a monitor, on which a thread may wait for some condition to become true.

Each condition variable *c* is associated with an [assertion](https://en.wikipedia.org/wiki/Assertion_(computing)) *P<sub>c</sub>*. While a thread is waiting on a condition variable, that thread is not considered to occupy the monitor, and so **other threads** may enter the monitor to change the monitor's state.

In most types of monitors, **these other threads** may **signal the condition variable** *c* to indicate that assertion *P<sub>c</sub>* is true in the current state.

There are two main operations on condition variables:

-   **wait** c, m: c is condition variable and m is a mutex(lock) associated with the monitor. fundamental contract, of the "wait" operation, is to do the following steps atomically:
> 
> 1.  release the mutex *m*,
> 2.  move this thread from the "ready queue" to *c*'s "wait-queue" (a.k.a. "sleep-queue") of threads, and
> 3.  sleep this thread. (Context is synchronously yielded to another thread.)
> 
		*The atomicity of the operations within step 1 is important to avoid race conditions that would be caused by a preemptive thread switch in-between them. One failure mode that could occur if these were not atomic is a missed wakeup, in which the thread could be on c's sleep-queue and have released the mutex, but a preemptive thread switch occurred before the thread went to sleep, and another thread called a signal/notify operation (see below) on c moving the first thread back out of c's queue. As soon as the first thread in question is switched back to, its program counter will be at step 1c, and it will sleep and be unable to be woken up again, violating the invariant that it should have been on c's sleep-queue when it slept. *

-   **signal** c**:** Also known as notify c, is called by a thread to indicate that the assertion Pc is true. Depending on the type and implementation of the monitor, this moves one or more threads from c's sleep-queue to the "ready queue" or another queues for it to be executed. It is usually considered a best practice to perform the "signal"/"notify" operation before releasing mutex *m* that is associated with *c.*

-   The **broadcast** c, also known as **notifyAll** c, is a similar operation that wakes up all threads in c's wait-queue. This empties the wait-queue. Generally, when more than one predicate condition is associated with the same condition variable, the application will require **broadcast** instead of **signal** because a thread waiting for the wrong condition might be woken up and then immediately go back to sleep without waking up a thread waiting for the correct condition that just became true. Otherwise, if the predicate condition is one-to-one with the condition variable associated with it, then **signal** may be more efficient than **broadcast**.

As a design rule, multiple condition variables can be associated with the same mutex, but not vice versa.

In the producer-consumer example, The **"producer"** threads will want to wait on a monitor using **lock m and a condition variable c\_{full}** which blocks until the queue is non-full. The **"consumer"** threads will want to wait on a different monitor using the **same mutex m but a different condition variable c\_{empty}** which blocks until the queue is non-empty.

