[Lock](https://en.wikipedia.org/wiki/Lock_(computer_science))
---------------

A **lock** or **mutex** (from [mutual exclusion](https://en.wikipedia.org/wiki/Mutual_exclusion)) is a [synchronization](https://en.wikipedia.org/wiki/Synchronization_(computer_science)) mechanism for enforcing limits on access to a resource in an environment where there are many [threads of execution](https://en.wikipedia.org/wiki/Thread_(computer_science)).

A lock is designed to enforce a [mutual exclusion](https://en.wikipedia.org/wiki/Mutual_exclusion) [concurrency control](https://en.wikipedia.org/wiki/Concurrency_control) policy.

Generally, locks are *advisory locks*, where each thread cooperates by acquiring the lock before accessing the corresponding data.

Some systems also implement *mandatory locks*, where attempting unauthorized access to a locked resource will force an [exception](https://en.wikipedia.org/wiki/Exception_handling) in the entity attempting to make the access.

The simplest type of lock is a binary [semaphore](https://en.wikipedia.org/wiki/Semaphore_(programming)). It provides exclusive access to the locked data. Other schemes also provide shared access for reading data. Other widely implemented access modes are exclusive, intend-to-exclude and intend-to-upgrade.

Another way to classify locks is by what happens when the [lock strategy](https://en.wikipedia.org/w/index.php?title=Lock_strategy&action=edit&redlink=1) prevents progress of a thread.

Most locking designs [block](https://en.wikipedia.org/wiki/Blocking_(computing)) the [execution](https://en.wikipedia.org/wiki/Execution_(computers)) of the [thread](https://en.wikipedia.org/wiki/Thread_(computer_science)) requesting the lock until it is allowed to access the locked resource.

With a [spinlock](https://en.wikipedia.org/wiki/Spinlock), the thread simply waits ("spins") until the lock becomes available. This is efficient if threads are blocked for a short time, because it avoids the overhead of operating system process re-scheduling. It is inefficient if the lock is held for a long time, or if the progress of the thread that is holding the lock depends on preemption of the locked thread.

Locks typically require hardware support for efficient implementation. This support usually takes the form of one or more [atomic](https://en.wikipedia.org/wiki/Atomic_(computer_science)) instructions such as "[test-and-set](https://en.wikipedia.org/wiki/Test-and-set)", "[fetch-and-add](https://en.wikipedia.org/wiki/Fetch-and-add)" or "[compare-and-swap](https://en.wikipedia.org/wiki/Compare-and-swap)". These instructions allow a single process to test if the lock is free, and if free, acquire the lock in a single atomic operation.

[Dekker's](https://en.wikipedia.org/wiki/Dekker%27s_algorithm) or [Peterson's algorithm](https://en.wikipedia.org/wiki/Peterson%27s_algorithm) are possible substitutes if atomic locking operations are not available.

Careless use of locks can result in [deadlock](https://en.wikipedia.org/wiki/Deadlock) or [livelock](https://en.wikipedia.org/wiki/Livelock).

Three basic concepts about locks:
-   ***lock overhead***: the extra resources for using locks, like the memory space allocated for locks, the CPU time to initialize and destroy locks, and the time for acquiring or releasing locks. The more locks a program uses, the more overhead associated with the usage;

-   ***lock [contention](https://en.wikipedia.org/wiki/Resource_contention)***: this occurs whenever one process or thread attempts to acquire a lock held by another process or thread. The more fine-grained the available locks, the less likely one process/thread will request a lock held by the other. (For example, locking a row rather than the entire table, or locking a cell rather than the entire row.);

-   [***deadlock***](https://en.wikipedia.org/wiki/Deadlock): the situation when each of at least two tasks is waiting for a lock that the other task holds. Unless something is done, the two tasks will wait forever.

There is a tradeoff between decreasing lock overhead and decreasing lock contention when choosing the number of locks in synchronization.

An example of a banking application: Design a class Account that allows multiple concurrent clients to deposit or withdraw money to an account; and give an algorithm to transfer money from one account to another. 

*Example*
-----------

```
class Account:
    member balance : Integer
    member mutex : Lock
    method deposit(n : Integer)
           mutex.lock()
           balance ← balance + n
           mutex.unlock()
    method withdraw(n : Integer)
           deposit(−n)

----------------------------------

function transfer(from : Account, to : Account, amount : integer)
    if from < to    // arbitrary ordering on the locks
        from.lock()
        to.lock()
    else
        to.lock()
        from.lock()
    from.withdraw(amount)
    to.deposit(amount)
    from.unlock()
    to.unlock()

```
