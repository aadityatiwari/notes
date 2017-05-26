[Synchronization](https://en.wikipedia.org/wiki/Synchronization_(computer_science))
--------------

**Synchronization** refers to one of two distinct but related concepts:
-   synchronization of [processes](https://en.wikipedia.org/wiki/Process_(computer_science))
-   synchronization of [data](https://en.wikipedia.org/wiki/Dataset)

*Process synchronization* refers to the idea that multiple processes are to join up or [handshake](https://en.wikipedia.org/wiki/Handshaking) at a certain point, in order to reach an agreement or commit to a certain sequence of action.

[*Data synchronization*](https://en.wikipedia.org/wiki/Data_synchronization) refers to the idea of keeping multiple copies of a dataset in coherence with one another, or to maintain [data integrity](https://en.wikipedia.org/wiki/Data_integrity).

Process synchronization primitives are commonly used to implement data synchronization.

The need for synchronization does not arise merely in multi-processor systems but for any kind of concurrent processes; even in single processor systems. e.g.: [*Forks and Joins*](https://en.wikipedia.org/wiki/Fork-join_model), [*Producer-Consumer*](https://en.wikipedia.org/wiki/Producer%E2%80%93consumer_problem), exclusive use resources.

**Process synchronization**
----------------

Thread synchronization is defined as a mechanism which ensures that two or more concurrent [processes](https://en.wikipedia.org/wiki/Process_(computer_science)) or [threads](https://en.wikipedia.org/wiki/Thread_(computer_science)) do not simultaneously execute some particular program segment known as [critical section](https://en.wikipedia.org/wiki/Mutual_exclusion). 

If proper synchronization techniques are not applied, it may cause a [race condition](https://en.wikipedia.org/wiki/Race_condition#Software) where the values of variables may be unpredictable and vary depending on the timings of [context switches](https://en.wikipedia.org/wiki/Context_switch) of the processes or threads.

Other than mutual exclusion, synchronization also deals with deadlock, starvation, priority inversion, and busy waiting.

In [Java](https://en.wikipedia.org/wiki/Java_(programming_language)), to prevent thread interference and memory consistency errors, blocks of code are wrapped into **synchronized** *(lock\_object)* sections. This forces any thread to acquire the said lock object before it can execute the block. The lock is automatically released when thread leaves the block or enters the waiting state within the block. 

In addition to mutual exclusion and memory consistency, Java *synchronized* blocks enable signaling, sending events from those threads, which have acquired the lock and execute the code block to those which are waiting for the lock within the block. 

Java synchronized sections combine functionality of mutexes and events. Such primitive is known as [synchronization monitor](https://en.wikipedia.org/wiki/Monitor_(synchronization)).

Any object is fine to be used as a lock/monitor in Java. The declaring object is implicitly implied as lock object when the whole method is marked with *synchronized*.

Synchronization can be implemented using one of following: spinlock, barriers, or semaphores

-   [**Spinlock**](https://en.wikipedia.org/wiki/Spinlock): Before accessing any shared resource, check flag. IF flag not set(reset) then set it and continue execution of thread, ELSE IF already set then keep spinning in a loop until flag resets.

-   [**Barriers**](https://en.wikipedia.org/wiki/Barrier_(computer_science)): Based on concept of implementing wait cycles.  A barrier for a group of threads or processes in the source code means any thread/process must stop at this point and cannot proceed until all other threads/processes reach this barrier. Experiments show that 34% of the total execution time is spent in waiting for other slower threads.

```
The barrier synchronization wait function for i<sup>th</sup> thread can be represented as:

(Wbarrier)i = f ((Tbarrier)i, (Rthread)i) ,
where **Wbarrier** is the wait time for a thread, 
**Tbarrier** is the number of threads has arrived, 
and **Rthread** is the arrival rate of threads

```

-   [**Semaphores**](https://en.wikipedia.org/wiki/Semaphore_(programming)): Semaphores are signaling mechanisms which can allow one or more threads/processors to access a section. A Semaphore has a flag which has a certain fixed value associated with it and each time a thread wishes to access the section, it decrements the flag. Similarly, when the thread leaves the section, the flag is incremented. If the flag is zero, the thread cannot access the section and gets blocked if it chooses to wait.


**Data synchronization**
-------------
-   Refers to the need to keep multiple copies of a set of data coherent with one another or to maintain [data integrity](https://en.wikipedia.org/wiki/Data_integrity)

-   For example, database replication is used to keep multiple copies of data synchronized with database servers that store data in different locations.

> <img src="https://upload.wikimedia.org/wikipedia/commons/d/dc/Data_Synchronization.png" alt="https://upload.wikimedia.org/wikipedia/commons/d/dc/Data_Synchronization.png" width="325" height="254" />

*Other Examples include:*

-   [File synchronization](https://en.wikipedia.org/wiki/File_synchronization), such as syncing a hand-held MP3 player to a desktop computer;

-   [Cluster file systems](https://en.wikipedia.org/wiki/Cluster_file_system), which are [file systems](https://en.wikipedia.org/wiki/File_system) that maintain data or indexes in a coherent fashion across a whole [computing cluster](https://en.wikipedia.org/wiki/Computing_cluster);

-   [Cache coherency](https://en.wikipedia.org/wiki/Cache_coherency), maintaining multiple copies of data in sync across multiple [caches](https://en.wikipedia.org/wiki/Cache_(computing));

-   [RAID](https://en.wikipedia.org/wiki/RAID), where data is written in a redundant fashion across multiple disks, so that the loss of any one disk does not lead to a loss of data;

-   [Database replication](https://en.wikipedia.org/wiki/Database_replication), where copies of data on a [database](https://en.wikipedia.org/wiki/Database) are kept in sync, despite possible large geographical separation;

-   [Journaling](https://en.wikipedia.org/wiki/Journaling_file_system), a technique used by many modern file systems to make sure that file metadata are updated on a disk in a coherent, consistent manner.

Some of the challenges which user may face in data synchronization:
-   data formats complexity;
-   real-timeliness;
-   data security;
-   data quality;
-   performance.
