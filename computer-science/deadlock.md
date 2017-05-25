[Deadlock](https://en.wikipedia.org/wiki/Deadlock)
------------

A **deadlock** is a state in which each member of a group of actions is waiting for some other member to release a lock.

> <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/28/Process_deadlock.svg/451px-Process_deadlock.svg.png" alt="https://upload.wikimedia.org/wikipedia/commons/thumb/2/28/Process_deadlock.svg/451px-Process_deadlock.svg.png" width="499" height="230" />
> 
> Both processes need resources to continue execution. *P1* requires additional resource *R1* and is in possession of resource *R2*, *P2* requires additional resource *R2* and is in possession of *R1*; neither process can continue.
> 

In an [operating system](https://en.wikipedia.org/wiki/Operating_system), a deadlock occurs when a [process](https://en.wikipedia.org/wiki/Process_(computing)) or [thread](https://en.wikipedia.org/wiki/Thread_(computing)) enters a waiting [state](https://en.wikipedia.org/wiki/Process_state) because a requested [system resource](https://en.wikipedia.org/wiki/System_resource) is held by another waiting process, which in turn is waiting for another resource held by another waiting process. If a process is unable to change its state indefinitely because the resources requested by it are being used by another waiting process, then the system is said to be in a deadlock.

In a [communications system](https://en.wikipedia.org/wiki/Communications_system), deadlocks occur mainly due to lost or corrupt signals rather than resource contention.

A deadlock situation can arise if and only if all of the following conditions hold simultaneously in a system known as the ***Coffman conditions*.**

-   ***[Mutual exclusion](https://en.wikipedia.org/wiki/Mutual_exclusion):*** The resources involved must be unshareable; otherwise, the processes would not be prevented from using the resource when necessary. Only one process can use the resource at any given instant of time.
-   ***Hold and wait* or *resource holding:*** a process is currently holding at least one resource and requesting additional resources which are being held by other processes.
-   ***No [preemption](https://en.wikipedia.org/wiki/Preemption_(computing)):*** a resource can be released only voluntarily by the process holding it.
-   ***Circular wait:*** a process must be waiting for a resource which is being held by another process, which in turn is waiting for the first process to release the resource. In general, there is a [set](https://en.wikipedia.org/wiki/Set_(mathematics)) of waiting processes, *P* = {*P*<sub>1</sub>, *P*<sub>2</sub>, …, *P<sub>N</sub>*}, such that *P*<sub>1</sub> is waiting for a resource held by *P*<sub>2</sub>, *P*<sub>2</sub> is waiting for a resource held by *P*<sub>3</sub> and so on until *P<sub>N</sub>* is waiting for a resource held by *P*<sub>1</sub>.

A deadlock can be resolved by breaking the symmetry of the locks or of the locking mechanism.

Two processes competing for two resources in opposite order.
> 
> (A) A single process goes through.
> (B) The later process has to wait.
> (C) A deadlock occurs when the first process locks the first resource at the same time as the second process locks the second resource.
> (D) The deadlock can be resolved by cancelling and restarting the first process.
> 

**Deadlock** prevention works by preventing one of the four Coffman conditions from occurring.

A **livelock** is similar to a deadlock, except that the states of the processes involved in the livelock constantly change with regard to one another, none progressing. A livelock is a special case of [resource starvation](https://en.wikipedia.org/wiki/Resource_starvation).

A **spinlock** is a [lock](https://en.wikipedia.org/wiki/Lock_(computer_science)) which causes a [thread](https://en.wikipedia.org/wiki/Thread_(computer_science)) trying to acquire it to simply wait in a loop ("spin") while repeatedly checking if the lock is available. Since the thread remains active but is not performing a useful task, the use of such a lock is a kind of [busy waiting](https://en.wikipedia.org/wiki/Busy_waiting). 

Once acquired, spinlocks will usually be held until they are explicitly released, although in some implementations they may be automatically released if the thread being waited on (that which holds the lock) blocks, or "goes to sleep".

Because spinlocks avoid overhead from [operating system](https://en.wikipedia.org/wiki/Operating_system) [process rescheduling](https://en.wikipedia.org/wiki/Scheduling_(computing)) or [context switching](https://en.wikipedia.org/wiki/Context_switch), spinlocks are efficient if [threads](https://en.wikipedia.org/wiki/Thread_(computer_science)) are likely to be blocked for only short periods.
