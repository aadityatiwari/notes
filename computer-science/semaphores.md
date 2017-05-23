[Semaphores](https://en.wikipedia.org/wiki/Semaphore_(programming))
------------

A **semaphore** is a [variable](https://en.wikipedia.org/wiki/Variable_(programming)) or [abstract data type](https://en.wikipedia.org/wiki/Abstract_data_type) used to control access to a common resource by multiple [processes](https://en.wikipedia.org/wiki/Process_(computing)) in a [concurrent](https://en.wikipedia.org/wiki/Concurrent_computing) system such as a [multiprogramming](https://en.wikipedia.org/wiki/Computer_multitasking) operating system.

A trivial semaphore is a plain variable that is changed (for example, incremented or decremented, or toggled) depending on programmer-defined conditions. The variable is then used as a condition to control access to some system resource.

A useful way to think of a semaphore as used in the real-world systems is as a record of how many units of a particular resource are available, coupled with operations to adjust that record *safely* (i.e. to avoid [race conditions](https://en.wikipedia.org/wiki/Race_condition)) as units are required or become free, and, if necessary, wait until a unit of the resource becomes available. 

Semaphores are a useful tool in the prevention of race conditions; however, their use is by no means a guarantee that a program is free from these problems. 

Semaphores which allow an arbitrary resource count are called **counting semaphores**, while semaphores which are restricted to the values 0 and 1 (or locked/unlocked, unavailable/available) are called **binary semaphores** and are used to implement [locks](https://en.wikipedia.org/wiki/Lock_(computer_science)).

The semaphore concept was invented by Dutch computer scientist [Edsger Dijkstra](https://en.wikipedia.org/wiki/Edsger_Dijkstra) in 1963

When used to control access to a [pool](https://en.wikipedia.org/wiki/Pool_(computer_science)) of resources, a semaphore tracks only *how many* resources are free; it does not keep track of *which* of the resources are free.

The Fairness and safety are likely to be compromised if even a single process acts incorrectly. This includes:

>- requesting a resource and forgetting to release it;
>- releasing a resource that was never requested;
>- holding a resource for a long time without needing it;
>- Using a resource without requesting it first (or after releasing it).

Even if all processes follow these rules, *multi-resource [deadlock](https://en.wikipedia.org/wiki/Deadlock)* may still occur when there are different resources managed by different semaphores and when processes need to use more than one resource at a time, as illustrated by the [dining philosophers problem](https://en.wikipedia.org/wiki/Dining_philosophers_problem).

Counting semaphores are equipped with two operations, historically denoted as P and V. Operation V [increments](https://en.wikipedia.org/wiki/Increment_operator) the semaphore *S*, and operation P [decrements](https://en.wikipedia.org/wiki/Decrement_operator) it.

The canonical names V and P come from the initials of [Dutch](https://en.wikipedia.org/wiki/Dutch_language) words. V as *verhogen* ("increase") and P as *prolaag* ("try to reduce"). In English, *V* and *P* operations are called, respectively, *up* and *down*. In software engineering practice, they are often called ***signal* **and ***wait***, or ***release*** and ***acquire*** (which the standard [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) library uses), or ***vacate*** and ***procure***.

The value of the semaphore *S* is the number of units of the resource that are currently available. The value of S can only be changed using the V and P operations.

- {**wait (P):** decrement S by 1}
- {**signal (V):** increment S by 1}

```
function V(semaphore S, integer I):
    [S ← S + I]

function P(semaphore S, integer I):
    repeat:
        [if S ≥ I:
        S ← S − I
        break]
```

To avoid [starvation](https://en.wikipedia.org/wiki/Resource_starvation), a semaphore has an associated [queue](https://en.wikipedia.org/wiki/Queue_(data_structure)) of processes (usually with [FIFO](https://en.wikipedia.org/wiki/FIFO_(computing_and_electronics))  semantics).

If a process performs a P operation on a semaphore that has the value zero, the process is added to the semaphore's queue and its execution is suspended.

When another process increments the semaphore by performing a V operation, and there are processes on the queue, one of them is removed from the queue and resumes execution.

When processes have different priorities the queue may be ordered by priority, so that the highest priority process is taken from the queue first.

Atomicity may be achieved by using a machine instruction that is able to [read, modify and write](https://en.wikipedia.org/wiki/Read-modify-write) the semaphore in a single operation. 

In the [producer–consumer problem](https://en.wikipedia.org/wiki/Producer%E2%80%93consumer_problem), one process (the producer) generates data items and another process (the consumer) receives and uses them.

They communicate using a queue of maximum size N and are subject to the following conditions:

>
>- **Consumer** must **wait** for the producer to produce something **if the queue is empty**;
>- **Producer** must **wait** for the consumer to consume something **if the queue is full.**
>

The semaphore solution to the **producer–consumer problem** tracks the state of the queue **with two semaphores:** 

>
>- **emptyCount**, the number of empty places in the queue, and
>- **fullCount**, the number of elements in the queue.
>

*Producer will decrease emptyCount and increase fullCount*

*Consumer will decrease fullCount and increase emptyCount*

The binary semaphore useQueue ensures that the integrity of the state of the queue itself is not compromised

The emptyCount is initially *N*, fullCount is initially 0, and useQueue is initially 1.

The producer does the following repeatedly:
```
produce:
    P(emptyCount)
    P(useQueue)
    putItemIntoQueue(item)
    V(useQueue)
    V(fullCount)
```

The consumer does the following repeatedly:
```
consume:
    P(fullCount)
    P(useQueue)
    item ← getItemFromQueue()
    V(useQueue)
    V(emptyCount)
```

(emptyCount + fullCount ≤ *N*) always holds, with equality if and only if no producers or consumers are executing their critical sections.

**Semaphores vs. mutexes: **
-----------

-   A [mutex](https://en.wikipedia.org/wiki/Mutex) is essentially the same thing as a binary semaphore and sometimes uses the same basic implementation.
-   The differences between them are in how they are used.
-   **While a binary semaphore may be used as a mutex, a mutex is a more specific use-case, in that only the thread that locked the mutex is supposed to unlock it.**
-   This constraint makes it possible to implement some additional features in mutexes:

Since only the thread that locked the mutex is supposed to unlock it, a mutex may store the id of thread that locked it and verify the same thread unlocks it.

Mutexes may provide [priority inversion](https://en.wikipedia.org/wiki/Priority_inversion) safety. If the mutex knows who locked it and is supposed to unlock it, it is possible to promote the priority of that thread whenever a higher-priority task starts waiting on the mutex.

Mutexes may provide deletion safety, where the thread holding the mutex cannot be accidentally deleted.

Alternately, if the thread holding the mutex is deleted (perhaps due to an unrecoverable error), the mutex can be automatically released.

A mutex may be recursive: a thread is allowed to lock it multiple times without causing a deadlock.
