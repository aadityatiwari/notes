[Mutual exclusion](https://en.wikipedia.org/wiki/Mutual_exclusion)
-------------

The  **mutual exclusion** is a property of [concurrency control](https://en.wikipedia.org/wiki/Concurrency_control), which is instituted for the purpose of preventing [race conditions](https://en.wikipedia.org/wiki/Race_condition)

It is the requirement that one [thread of execution](https://en.wikipedia.org/wiki/Thread_(computing)) never enter its [critical section](https://en.wikipedia.org/wiki/Critical_section) at the same time that another [concurrent](https://en.wikipedia.org/wiki/Concurrent_computing) thread of execution enters its own critical section.

> <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/2f/Mutual_exclusion_example_with_linked_list.png/640px-Mutual_exclusion_example_with_linked_list.png" alt="https://upload.wikimedia.org/wikipedia/commons/thumb/2/2f/Mutual_exclusion_example_with_linked_list.png/640px-Mutual_exclusion_example_with_linked_list.png" width="411" height="291" />
> 
> Two nodes, *i* and *i* + 1, being removed simultaneously results in node *i* + 1 not being removed.

A simple example of why mutual exclusion is important in practice can be visualized using a [singly linked list](https://en.wikipedia.org/wiki/Singly_linked_list) of four items, where the second and third are to be removed. The removal of a node that sits between 2 other nodes is performed by changing the next pointer of the previous node to point to the next node (in other words, if node i is being removed, then the next pointer of node i − 1 is changed to point to node i + 1, thereby removing from the linked list any reference to node i).

When such a linked list is being shared between multiple threads of execution, two threads of execution may attempt to remove two different nodes simultaneously, one thread of execution changing the next pointer of node i − 1 to point to node i + 1, while another thread of execution changes the next pointer of node i to point to node i + 2. Although both removal operations complete successfully, the desired state of the linked list is not achieved: node i + 1 remains in the list, because the next pointer of node i − 1 points to node i + 1.

This problem (called a race condition) can be avoided by using the requirement of mutual exclusion to ensure that simultaneous updates to the same part of the list cannot occur.

The problem which mutual exclusion addresses is a problem of resource sharing: how can a software system control multiple processes' access to a shared resource, when each process needs exclusive control of that resource while doing its work?

A successful solution to this problem must have at least these two properties:

-   It must implement *mutual exclusion*: only one process can be in the critical section at a time.
-   It must be free of [*deadlocks*](https://en.wikipedia.org/wiki/Deadlock): if processes are trying to enter the critical section, one of them must eventually be able to do so successfully, provided no process stays in the critical section permanently.
>
> <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d2/State_graph.png/522px-State_graph.png" alt="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d2/State_graph.png/522px-State_graph.png" width="375" height="195" />
> 

If a process wishes to enter the critical section, it must first execute the trying section and wait until it acquires access to the critical section. After the process has executed its critical section and is finished with the shared resources, it needs to execute the exit section to release them for other processes' use. The process then returns to its non-critical section.

The **reentrant mutex** (**recursive mutex**, **recursive lock**) is particular type of [mutual exclusion](https://en.wikipedia.org/wiki/Mutual_exclusion) (mutex) device that may be locked multiple times by the same [process/thread](https://en.wikipedia.org/wiki/Thread_(computing)), without causing a [deadlock](https://en.wikipedia.org/wiki/Deadlock).

While any attempt to perform the "lock" operation on an ordinary mutex (lock) would either fail or block when the mutex is already locked, on a recursive mutex this operation will succeed [if and only if](https://en.wikipedia.org/wiki/If_and_only_if) the locking thread is the one that already holds the lock.

Typically, a recursive mutex tracks the number of times it has been locked, and requires equally many unlock operations to be performed before other threads may lock it.

```
var m : Mutex  // A non-recursive mutex, initially unlocked.

function lock_and_call(i : Integer)
    m.lock()
    callback(i)
    m.unlock()

function callback(i : Integer)
    if i > 0
        lock_and_call(i - 1)

```

Recursive mutexes solve the problem of [non-reentrancy](https://en.wikipedia.org/wiki/Reentrancy_(computing)) with regular mutexes: if a function that takes a lock and executes a callback is itself called by the callback, [deadlock](https://en.wikipedia.org/wiki/Deadlock) ensues.

Replacing the mutex with a recursive one solves the problem of deadlock, which otherwise could occur by waiting for itself.

The [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) language's native synchronization mechanism, [monitor](https://en.wikipedia.org/wiki/Monitor_(synchronization)), uses recursive locks. Syntactically, a lock is a block of code with the 'synchronized' keyword preceding it and any [Object](https://en.wikipedia.org/wiki/Object_(computer_science)) reference in parentheses that will be used as the mutex.

Inside the synchronized block, the given object can be used as a condition variable by doing a wait(), notify(), or notifyAll() on it. Thus all Objects in Java are both recursive mutexes and [condition variables](https://en.wikipedia.org/wiki/Condition_variable).

*Example:*

1.  *Thread A calls function F which acquires a reentrant lock for itself before proceeding*

2.  *Thread B calls function F which attempts to acquire a reentrant lock for itself but cannot due to one already outstanding, resulting in either a block (it waits), or a timeout if requested*

3.  *Thread A's F calls itself recursively. It already owns the lock, so it will not block itself (no deadlock). This is the central idea of a reentrant mutex, and is what makes it different from a regular lock.*

4.  *Thread B's F is still waiting, or has caught the timeout and worked around it*

5.  *Thread A's F finishes and releases its lock(s)*

6.  *Thread B's F can now acquire a reentrant lock and proceed if it was still waiting*
