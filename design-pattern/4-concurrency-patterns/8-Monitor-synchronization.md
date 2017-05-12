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