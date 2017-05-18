[Readers-writer lock](https://en.wikipedia.org/wiki/Readers-writer%20lock)
-------------

A **readers–writer** (**RW**) or **shared-exclusive** lock (also known as a **multiple readers/single-writer** lock or **multi-reader** lock) is a [synchronization](https://en.wikipedia.org/wiki/Synchronization_(computer_science)) primitive that solves one of the [readers–writers problems](https://en.wikipedia.org/wiki/Readers%E2%80%93writers_problem).

RW lock allows [concurrent](https://en.wikipedia.org/wiki/Concurrency_control) access for read-only operations, while write operations require exclusive access.

This means that multiple threads can read the data in parallel but an exclusive [lock](https://en.wikipedia.org/wiki/Lock_(computer_science)) is needed for writing or modifying data.

A common use might be to control access to a data structure in memory that cannot be updated [atomically](https://en.wikipedia.org/wiki/Atomicity_(programming)) and is invalid (and should not be read by another thread) until the update is complete.

Readers–writer locks are usually constructed on top of [mutexes](https://en.wikipedia.org/wiki/Mutex) and [condition variables](https://en.wikipedia.org/wiki/Condition_variable), or on top of [semaphores](https://en.wikipedia.org/wiki/Semaphore_(programming)).

RW locks can be designed with different priority policies for reader vs. writer access. 

The lock can either be designed to always give priority to readers (***read-preferring***), to always give priority to writers (***write-preferring***) or be ***unspecified*** with regards to priority. These policies lead to different tradeoffs with regards to [concurrency](https://en.wikipedia.org/wiki/Concurrency_(computer_science)) and [starvation](https://en.wikipedia.org/wiki/Resource_starvation).

> 
> ***Read-preferring RW locks*** allow for maximum concurrency, but can lead to write-starvation if contention is high. This is because writer threads will not be able to acquire the lock as long as at least one reading thread holds it.
> ***Write-preferring RW locks*** avoid the problem of writer starvation by preventing any *new* readers from acquiring the lock if there is a writer queued and waiting for the lock; the writer will acquire the lock as soon as all readers which were already holding the lock have completed.
> ***Unspecified priority RW locks*** does not provide any guarantees with regards read vs. write access.
> 

**Pseudocode using two mutexes and a single integer counter**. 
-------------

One mutex, *r*, protects *b* and is only used by readers; the other, *g* (for "global") ensures mutual exclusion of writers.
```
Begin Read
•	Lock r.
•	Increment b.
•	If b = 1, lock g.
•	Unlock r.

End Read
•	Lock r.
•	Decrement b.
•	If b = 0, unlock g.
•	Unlock r.

Begin Write
•	Lock g.

End Write 
•	Unlock g.

```

**Pseudocode using a condition variable and a mutex**
------------

Input: mutex *m*, condition variable *c*, integer *r* (number of readers waiting), flag *w* (writer waiting).

The lock-for-read operation in this setup is:
```
•	Lock m (blocking).
•	While w:
•	wait c, m
•	Increment r.
•	Unlock m.
```

The lock-for-write operation:
```
•	Lock m (blocking).
•	While (w or r > 0):
•	wait c, m
•	Set w to true.
•	Unlock m.
```

Each of lock-for-read and lock-for-write has its own inverse operation. Releasing a read lock is done by decrementing *r* and signaling *c* if *r* has become zero (both while holding *m*), so that one of the threads waiting on *c* can wake up and lock the R/W lock. Releasing the write lock means setting *w* to false and broadcasting on *c* (again while holding *m*).

ReadWriteLock interface and the ReentrantReadWriteLock locks in [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) version 5 or above

The [read-copy-update](https://en.wikipedia.org/wiki/Read-copy-update) (RCU) algorithm is one solution to the readers–writers problem.

RCU is [wait-free](https://en.wikipedia.org/wiki/Lock-free_and_wait-free_algorithms) for readers. The [Linux kernel](https://en.wikipedia.org/wiki/Linux_kernel) implements a special solution for few writers called [seqlock](https://en.wikipedia.org/wiki/Seqlock).
