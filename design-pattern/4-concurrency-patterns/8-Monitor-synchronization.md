Monitor (synchronization)
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
---------------

For many applications, mutual exclusion is not enough. Threads attempting an operation may need to wait until some condition *P* holds true.

A busy waiting loop "**while not( P ) do skip**" will not work, as mutual exclusion will prevent any other thread from entering the monitor to make the condition true.

What is needed is a way to signal the thread when the condition P is true (or *could* be true).

A condition variable is a queue of threads, associated with a monitor, on which a thread may wait for some condition to become true.

Each condition variable *c* is associated with an [assertion](https://en.wikipedia.org/wiki/Assertion_(computing)) *P<sub>c</sub>*. While a thread is waiting on a condition variable, that thread is not considered to occupy the monitor, and so **other threads** may enter the monitor to change the monitor's state.

In most types of monitors, **these other threads** may **signal the condition variable** *c* to indicate that assertion *P<sub>c</sub>* is true in the current state.

There are three main operations on condition variables:

-   **wait** c, m: c is condition variable and m is a mutex(lock) associated with the monitor. fundamental contract, of the "wait" operation, is to do the following steps atomically:
> 
> 1.  release the mutex *m*,
> 2.  move this thread from the "ready queue" to *c*'s "wait-queue" (a.k.a. "sleep-queue") of threads, and
> 3.  sleep this thread. (Context is synchronously yielded to another thread.)
> 
> *The atomicity of the operations within step 1 is important to avoid race conditions that would be caused by a preemptive thread switch in-between them. One failure mode that could occur if these were not atomic is a missed wakeup, in which the thread could be on c's sleep-queue and have released the mutex, but a preemptive thread switch occurred before the thread went to sleep, and another thread called a signal/notify operation (see below) on c moving the first thread back out of c's queue. As soon as the first thread in question is switched back to, its program counter will be at step 1c, and it will sleep and be unable to be woken up again, violating the invariant that it should have been on c's sleep-queue when it slept.*
> 

-   **signal** c: Also known as notify c, is called by a thread to indicate that the assertion Pc is true. Depending on the type and implementation of the monitor, this moves one or more threads from c's sleep-queue to the "ready queue" or another queues for it to be executed. It is usually considered a best practice to perform the "signal"/"notify" operation before releasing mutex *m* that is associated with *c.*

-   The **broadcast** c, also known as **notifyAll** c, is a similar operation that wakes up all threads in c's wait-queue. This empties the wait-queue. Generally, when more than one predicate condition is associated with the same condition variable, the application will require **broadcast** instead of **signal** because a thread waiting for the wrong condition might be woken up and then immediately go back to sleep without waking up a thread waiting for the correct condition that just became true. Otherwise, if the predicate condition is one-to-one with the condition variable associated with it, then **signal** may be more efficient than **broadcast**.

As a design rule, multiple condition variables can be associated with the same mutex, but not vice versa.

In the producer-consumer example, The **"producer"** threads will want to wait on a monitor using **lock m and a condition variable c\_{full}** which blocks until the queue is non-full. The **"consumer"** threads will want to wait on a different monitor using the **same mutex m but a different condition variable c\_{empty}** which blocks until the queue is non-empty.


**Monitor usage:**
------------

```
acquire(m); // Acquire this monitor's lock.
while (!p) { // While the condition/predicate/assertion that we are waiting for is not true...
	wait(m, cv); // Wait on this monitor's lock and condition variable.
}

// ... Critical section of code goes here ...

signal(cv2); //-- OR -- notifyAll(cv2); cv2 might be the same as cv or different.
release(m); // Release this monitor's lock.

```

To be more precise, this is the same pseudocode but with more verbose comments to better explain what is going on:

```java

// ... (previous code)
// About to enter the monitor.
// Acquire the advisory mutex (lock) associated with the concurrent data that is shared between threads, 
// to ensure that no two threads can be preemptively interleaved or run simultaneously on different cores
// while executing in critical sections that read or write this same concurrent data.
// If another thread is holding this mutex, then this thread will be sleeped (blocked) and placed on
// m's sleep queue.  (Mutex "m" shall not be a spin-lock.)
acquire(m);
// Now, we are holding the lock and can check the condition for the first time.

// The first time we execute the while loop condition after the above "acquire", we are asking,
// "Does the condition/predicate/assertion we are waiting for happen to already be true?"

while ( ! p() ) // "p" is any expression (e.g. variable or function-call) that checks the condition
				// and evaluates to boolean.  This itself is a critical section, so you *MUST*
				// be holding the lock when executing this "while" loop condition!
				
// If this is not the first time the "while" condition is being checked, then we are asking the question,
// "Now that another thread using this monitor has notified me and woken me up and I have been
// context-switched back to, did the condition/predicate/assertion we are waiting on stay true between
// the time that I was woken up and the time that I
// re-acquired the lock inside the "wait" call in the last iteration of this loop,
// or did some other thread cause the condition to become false again in the meantime
// thus making this a spurious wakeup?

{
	// If this is the first iteration of the loop, then the answer is "no" -- the condition is not ready yet.
	// Otherwise, the answer is: the latter.  This was a spurious wakeup, some other thread occurred first
	// and caused the condition to become false again, and we must wait again.

	wait(m, cv);
		// Temporarily prevent any other thread on any core from doing operations on m or cv.
		// release(m) 	// Atomically release lock "m" so other code using this concurrent data
		// 				// can operate, move this thread to cv's wait-queue so that it will be notified
		//				// sometime when the condition becomes true, and sleep this thread.
		//				// Re-enable other threads and cores to do operations on m and cv.
		//
		// Context switch occurs on this core.
		//
		// At some future time, the condition we are waiting for becomes true,
		// and another thread using this monitor (m, cv) does either a signal/notify
		// that happens to wake this thread up, or a notifyAll that wakes us up, meaning
		// that we have been taken out of cv's wait-queue.
		//
		// During this time, other threads may be switched to that caused the condition to become
		// false again, or the condition may toggle one or more times, or it may happen to
		// stay true.
		//
		// This thread is switched back to on some core.
		//
		// acquire(m)	// Lock "m" is re-acquired.
		
	// End this loop iteration and re-check the "while" loop condition to make sure the predicate is
	// still true.
	
}

// The condition we are waiting for is true!
// We are still holding the lock, either from before entering the monitor or from the
// last execution of "wait".

// Critical section of code goes here, which has a precondition that our predicate
// must be true.
// This code might make cv's condition false, and/or make other condition variables'
// predicates true.

// Call signal/notify or notifyAll, depending on which condition variables' predicates
// (who share mutex m) have been made true or may have been made true, and the monitor semantic type
// being used.

for (cv_x in cvs_to_notify){
	notify(cv_x); //-- OR -- notifyAll(cv_x);
}
// One or more threads have been woken up but will block as soon as they try
// to acquire m.

// Release the mutex so that notified thread(s) and others can enter
// their critical sections.
release(m);
```

**Solving the bounded producer/consumer problem**
-----------

The classic solution is to use **two monitors**, comprising **two condition variables** sharing **one lock** on the queue.

```java
global volatile RingBuffer queue; // A thread-unsafe ring-buffer of tasks.
global Lock queueLock;  // A mutex for the ring-buffer of tasks.  (Not a spin-lock.)
global CV queueEmptyCV; // A condition variable for consumer threads waiting for the queue to become non-empty. Its associated lock is "queueLock".
global CV queueFullCV; // A condition variable for producer threads waiting for the queue to become non-full. Its associated lock is also "queueLock".

// Method representing each producer thread's behavior:
public method producer(){
    while(true){
        task myTask=...; // Producer makes some new task to be added.

        queueLock.acquire(); // Acquire lock for initial predicate check.
        while(queue.isFull()){ // Check if the queue is non-full.
            // Make the threading system atomically release queueLock,
            // enqueue this thread onto queueFullCV, and sleep this thread.
            wait(queueLock, queueFullCV);
            // Then, "wait" automatically re-acquires "queueLock" for re-checking
            // the predicate condition.
        }
        
        // Critical section that requires the queue to be non-full.
        // N.B.: We are holding queueLock.
        queue.enqueue(myTask); // Add the task to the queue.

        // Now the queue is guaranteed to be non-empty, so signal a consumer thread
        // or all consumer threads that might be blocked waiting for the queue to be non-empty:
        signal(queueEmptyCV); -- OR -- notifyAll(queueEmptyCV);
        
        // End of critical sections related to the queue.
        queueLock.release(); // Drop the queue lock until we need it again to add the next task.
    }
}

// Method representing each consumer thread's behavior:
public method consumer(){
    while(true){

        queueLock.acquire(); // Acquire lock for initial predicate check.
        while (queue.isEmpty()){ // Check if the queue is non-empty.
            // Make the threading system atomically release queueLock,
            // enqueue this thread onto queueEmptyCV, and sleep this thread.
            wait(queueLock, queueEmptyCV);
            // Then, "wait" automatically re-acquires "queueLock" for re-checking
            // the predicate condition.
        }
        // Critical section that requires the queue to be non-empty.
        // N.B.: We are holding queueLock.
        myTask=queue.dequeue(); // Take a task off of the queue.
        // Now the queue is guaranteed to be non-full, so signal a producer thread
        // or all producer threads that might be blocked waiting for the queue to be non-full:
        signal(queueFullCV); -- OR -- notifyAll(queueFullCV);

        // End of critical sections related to the queue.
        queueLock.release(); // Drop the queue lock until we need it again to take off the next task.

        doStuff(myTask); // Go off and do something with the task.
    }
}
```

A variant of this solution could use a single condition variable for both producers and consumers, perhaps named "queueFullOrEmptyCV" or "queueSizeChangedCV".  However, doing this would require using *notifyAll* in all the threads using the condition variable and cannot use a regular *signal*. This is because the regular *signal* might wake up a thread of the wrong type whose condition has not yet been met, and that thread would go back to sleep without a thread of the correct type getting signalled.

For example, a producer might make the queue full and wake up another producer instead of a consumer, and the woken producer would go back to sleep. In the complementary case, a consumer might make the queue empty and wake up another consumer instead of a producer, and the consumer would go back to sleep. Using *notifyAll* ensures that some thread of the right type will proceed as expected by the problem statement.

*Here is the **variant** using only **one condition variable** and **notifyAll**:*

```java
global volatile RingBuffer queue; // A thread-unsafe ring-buffer of tasks.
global Lock queueLock; // A mutex for the ring-buffer of tasks.  (Not a spin-lock.)

global CV queueFullOrEmptyCV; // A single condition variable for when the queue is not ready for
// any  thread -- i.e., for producer threads waiting for the queue to become non-full 
// and consumer threads waiting for the queue to become non-empty.
// Its associated lock is "queueLock". 
// Not safe to use regular "signal" because it is associated with
// multiple predicate conditions (assertions).

// Method representing each producer thread's behavior:
public method producer(){
    while(true){
        task myTask=...; // Producer makes some new task to be added.

        queueLock.acquire(); 	// Acquire lock for initial predicate check.
        while(queue.isFull()){ 	// Check if the queue is non-full.
            			// Make the threading system atomically release queueLock,
            			// enqueue this thread onto the CV, and sleep this thread.
            wait(queueLock, queueFullOrEmptyCV);
            // Then, "wait" automatically re-acquires "queueLock" for re-checking
            // the predicate condition.
        }
        
        // Critical section that requires the queue to be non-full.
        // N.B.: We are holding queueLock.
        queue.enqueue(myTask); // Add the task to the queue.

        // Now the queue is guaranteed to be non-empty, so signal all blocked threads
        // so that a consumer thread will take a task:
        notifyAll(queueFullOrEmptyCV); // Do not use "signal" (as it might wake up another producer instead).
        
        // End of critical sections related to the queue.
        queueLock.release(); // Drop the queue lock until we need it again to add the next task.
    }
}

// Method representing each consumer thread's behavior:
public method consumer(){
    while(true){

        queueLock.acquire(); // Acquire lock for initial predicate check.
        while (queue.isEmpty()){ 	// Check if the queue is non-empty.
           			 // Make the threading system atomically release queueLock,
           			 // enqueue this thread onto the CV, and sleep this thread.
            wait(queueLock, queueFullOrEmptyCV);
            // Then, "wait" automatically re-acquires "queueLock" for re-checking
            // the predicate condition.
        }
        // Critical section that requires the queue to be non-full.
        // N.B.: We are holding queueLock.
        myTask=queue.dequeue(); // Take a task off of the queue.

        // Now the queue is guaranteed to be non-full, so signal all blocked threads
        // so that a producer thread will take a task:
        notifyAll(queueFullOrEmptyCV); // Do not use "signal" (as it might wake up another consumer instead).

        // End of critical sections related to the queue.
        queueLock.release(); // Drop the queue lock until we need it again to take off the next task.

        doStuff(myTask); // Go off and do something with the task.
    }
}
```

Locks and condition variables are higher-level abstractions over **synchronization primitives** provided by hardware support that provides [atomicity](https://en.wikipedia.org/wiki/Atomic_operation).

On a uniprocessor, disabling and enabling interrupts is a way to implement monitors by preventing context switches during the critical sections of the locks and condition variables, but this is not enough on a multiprocessor.

On a multiprocessor, usually special atomic **read-modify-write** instructions on the memory such as **test-and-set**, [**compare-and-swap**](https://en.wikipedia.org/wiki/Compare-and-swap), etc. are used, depending on what the [ISA](https://en.wikipedia.org/wiki/Instruction_set) provides.

*Sample Mesa-monitor implementation with Test-and-Set*
-----------

```java
// Basic parts of threading system:
// Assume "ThreadQueue" supports random access.
public volatile ThreadQueue readyQueue; // Thread-unsafe queue of ready threads.  Elements are (Thread*).
public volatile global Thread* currentThread; // Assume this variable is per-core.  (Others are shared.)

// Implements a spin-lock on just the synchronized state of the threading system itself.
// This is used with test-and-set as the synchronization primitive.
public volatile global bool threadingSystemBusy=false;

// Context-switch interrupt service routine (ISR):
// On the current CPU core, preemptively switch to another thread.
public method contextSwitchISR(){
    if (testAndSet(threadingSystemBusy)){
        return; // Can't switch context right now.
    }

    // Ensure this interrupt can't happen again which would foul up the context switch:
    systemCall_disableInterrupts();

    // Get all of the registers of the currently-running process.
    // For Program Counter (PC), we will need the instruction location of
    // the "resume" label below.  Getting the register values is platform-dependent and may involve
    // reading the current stack frame, JMP/CALL instructions, etc.  (The details are beyond this scope.)
    currentThread->registers = getAllRegisters(); // Store the registers in the "currentThread" object in memory.
    currentThread->registers.PC = resume; // Set the next PC to the "resume" label below in this method.

    readyQueue.enqueue(currentThread); // Put this thread back onto the ready queue for later execution.
    
    Thread* otherThread=readyQueue.dequeue(); // Remove and get the next thread to run from the ready queue.
    
    currentThread=otherThread; // Replace the global current-thread pointer value so it is ready for the next thread.

    // Restore the registers from currentThread/otherThread, including a jump to the stored PC of the other thread
    // (at "resume" below).  Again, the details of how this is done are beyond this scope.
    restoreRegisters(otherThread.registers);

    // *** Now running "otherThread" (which is now "currentThread")!  The original thread is now "sleeping". ***

    resume: // This is where another contextSwitch() call needs to set PC to when switching context back here.

    // Return to where otherThread left off.

    threadingSystemBusy=false; // Must be an atomic assignment.
    systemCall_enableInterrupts(); // Turn pre-emptive switching back on on this core.

}

// Thread sleep method:
// On current CPU core, a synchronous context switch to another thread without putting
// the current thread on the ready queue.
// Must be holding "threadingSystemBusy" and disabled interrupts so that this method
// doesn't get interrupted by the thread-switching timer which would call contextSwitchISR().
// After returning from this method, must clear "threadingSystemBusy".
public method threadSleep(){
    // Get all of the registers of the currently-running process.
    // For Program Counter (PC), we will need the instruction location of
    // the "resume" label below.  Getting the register values is platform-dependent and may involve
    // reading the current stack frame, JMP/CALL instructions, etc.  (The details are beyond this scope.)
    currentThread->registers = getAllRegisters(); // Store the registers in the "currentThread" object in memory.
    currentThread->registers.PC = resume; // Set the next PC to the "resume" label below in this method.

    // Unlike contextSwitchISR(), we will not place currentThread back into readyQueue.
    // Instead, it has already been placed onto a mutex's or condition variable's queue.
    
    Thread* otherThread=readyQueue.dequeue(); // Remove and get the next thread to run from the ready queue.
    
    currentThread=otherThread; // Replace the global current-thread pointer value so it is ready for the next thread.

    // Restore the registers from currentThread/otherThread, including a jump to the stored PC of the other thread
    // (at "resume" below).  Again, the details of how this is done are beyond this scope.
    restoreRegisters(otherThread.registers);

    // *** Now running "otherThread" (which is now "currentThread")!  The original thread is now "sleeping". ***

    resume: // This is where another contextSwitch() call needs to set PC to when switching context back here.

    // Return to where otherThread left off.
    
}

public method wait(Mutex m, ConditionVariable c){
    // Internal spin-lock while other threads on any core are accessing this object's
    // "held" and "threadQueue", or "readyQueue".
    while (testAndSet(threadingSystemBusy)){}
    // N.B.: "threadingSystemBusy" is now true.
    
    // System call to disable interrupts on this core so that threadSleep() doesn't get interrupted by
    // the thread-switching timer on this core which would call contextSwitchISR().
    // Done outside threadSleep() for more efficiency so that this thread will be sleeped
    // right after going on the condition-variable queue.
    systemCall_disableInterrupts();
 
    assert m.held; // (Specifically, this thread must be the one holding it.)
    
    m.release();
    c.waitingThreads.enqueue(currentThread);
    
    threadSleep();
    
    // Thread sleeps ... Thread gets woken up from a signal/broadcast.
    
    threadingSystemBusy=false; // Must be an atomic assignment.
    systemCall_enableInterrupts(); // Turn pre-emptive switching back on on this core.
    
    // Mesa style:
    // Context switches may now occur here, making the client caller's predicate false.
    
    m.acquire();
    
}

public method signal(ConditionVariable c){

    // Internal spin-lock while other threads on any core are accessing this object's
    // "held" and "threadQueue", or "readyQueue".
    while (testAndSet(threadingSystemBusy)){}
    // N.B.: "threadingSystemBusy" is now true.
    
    // System call to disable interrupts on this core so that threadSleep() doesn't get interrupted by
    // the thread-switching timer on this core which would call contextSwitchISR().
    // Done outside threadSleep() for more efficiency so that this thread will be sleeped
    // right after going on the condition-variable queue.
    systemCall_disableInterrupts();
    
    if (!c.waitingThreads.isEmpty()){
        wokenThread=c.waitingThreads.dequeue();
        readyQueue.enqueue(wokenThread);
    }
    
    threadingSystemBusy=false; // Must be an atomic assignment.
    systemCall_enableInterrupts(); // Turn pre-emptive switching back on on this core.
    
    // Mesa style:
    // The woken thread is not given any priority.
    
}

public method broadcast(ConditionVariable c){

    // Internal spin-lock while other threads on any core are accessing this object's
    // "held" and "threadQueue", or "readyQueue".
    while (testAndSet(threadingSystemBusy)){}
    // N.B.: "threadingSystemBusy" is now true.
    
    // System call to disable interrupts on this core so that threadSleep() doesn't get interrupted by
    // the thread-switching timer on this core which would call contextSwitchISR().
    // Done outside threadSleep() for more efficiency so that this thread will be sleeped
    // right after going on the condition-variable queue.
    systemCall_disableInterrupts();
    
    while (!c.waitingThreads.isEmpty()){
        wokenThread=c.waitingThreads.dequeue();
        readyQueue.enqueue(wokenThread);
    }
    
    threadingSystemBusy=false; // Must be an atomic assignment.
    systemCall_enableInterrupts(); // Turn pre-emptive switching back on on this core.
    
    // Mesa style:
    // The woken threads are not given any priority.
    
}

class Mutex {
    protected volatile bool held=false;
    private volatile ThreadQueue blockingThreads; // Thread-unsafe queue of blocked threads.  Elements are (Thread*).
    
    public method acquire(){
        // Internal spin-lock while other threads on any core are accessing this object's
        // "held" and "threadQueue", or "readyQueue".
        while (testAndSet(threadingSystemBusy)){}
        // N.B.: "threadingSystemBusy" is now true.
        
        // System call to disable interrupts on this core so that threadSleep() doesn't get interrupted by
        // the thread-switching timer on this core which would call contextSwitchISR().
        // Done outside threadSleep() for more efficiency so that this thread will be sleeped
        // right after going on the lock queue.
        systemCall_disableInterrupts();

        assert !blockingThreads.contains(currentThread);

        if (held){
            // Put "currentThread" on this lock's queue so that it will be
            // considered "sleeping" on this lock.
            // Note that "currentThread" still needs to be handled by threadSleep().
            readyQueue.remove(currentThread);
            blockingThreads.enqueue(currentThread);
            threadSleep();
            
            // Now we are woken up, which must be because "held" became false.
            assert !held;
            assert !blockingThreads.contains(currentThread);
        }
        
        held=true;
        
        threadingSystemBusy=false; // Must be an atomic assignment.
        systemCall_enableInterrupts(); // Turn pre-emptive switching back on on this core.

    }        
        
    public method release(){
        // Internal spin-lock while other threads on any core are accessing this object's
        // "held" and "threadQueue", or "readyQueue".
        while (testAndSet(threadingSystemBusy)){}
        // N.B.: "threadingSystemBusy" is now true.
        
        // System call to disable interrupts on this core for efficiency.
        systemCall_disableInterrupts();
        
        assert held; // (Release should only be performed while the lock is held.)

        held=false;
        
        if (!blockingThreads.isEmpty()){
            Thread* unblockedThread=blockingThreads.dequeue();
            readyQueue.enqueue(unblockedThread);
        }
        
        threadingSystemBusy=false; // Must be an atomic assignment.
        systemCall_enableInterrupts(); // Turn pre-emptive switching back on on this core.
        
    }
}

struct ConditionVariable {
    volatile ThreadQueue waitingThreads;
}

```

*An example of Semaphore:*

```
class Semaphore
{
  private volatile int s := 0
  invariant s >= 0
  private ConditionVariable sIsPositive /* associated with s > 0 */
  private Mutex myLock /* Lock on "s" */

  public method P()
  {
    myLock.acquire()
    while s = 0:
      wait(myLock, sIsPositive)
    assert s > 0
    s := s - 1
    myLock.release()
  }

  public method V()
  {
    myLock.acquire()
    s := s + 1
    assert s > 0
    signal sIsPositive
    myLock.release()
  }
}
```

When a **signal** happens on a condition variable that at least one other thread is waiting on, there are at least two threads that could then occupy the monitor: the thread that signals and any one of the threads that is waiting.


