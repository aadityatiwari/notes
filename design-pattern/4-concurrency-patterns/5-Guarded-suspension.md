[Guarded suspension](https://en.wikipedia.org/wiki/Guarded_suspension)
-----------------

**Guarded suspension** is a [software design pattern](https://en.wikipedia.org/wiki/Design_pattern_(computer_science)) for managing operations that require both a [lock](https://en.wikipedia.org/wiki/Lock_(computer_science)) to be acquired and a [precondition](https://en.wikipedia.org/wiki/Precondition) to be satisfied before the operation can be executed.

The guarded suspension pattern is typically applied to method calls in object-oriented programs, and involves suspending the method call, and the calling thread, until the precondition (acting as a [guard](https://en.wikipedia.org/wiki/Guard_(computing))) is satisfied.

Because it is [blocking](https://en.wikipedia.org/wiki/Blocking_(computing)), the guarded suspension pattern is generally only used when the developer knows that a method call will be **suspended for a finite and reasonable period of time**. 

If a method call is suspended for too long, then the overall program will slow down or stop, waiting for the precondition to be satisfied. If the developer knows that the method call suspension will be indefinite or for an unacceptably long period, then the [balking pattern](https://en.wikipedia.org/wiki/Balking_pattern) may be preferred.

In Java, Object class provides the wait() and notify() methods to assist with guarded suspension.

```java

public class Example {
    synchronized void guardedMethod() {
        while (!preCondition()) {
            try {
                // Continue to wait
                wait();
                // …
            } catch (InterruptedException e) {
                // …
            }
        }
        // Actual task implementation
    }
    synchronized void alterObjectStateMethod() {
        // Change the object state
        // …
        // Inform waiting threads
        notify();
    }
}

```

An example of an actual implementation would be a queue object with a get method that has a guard to detect when there are no items in the queue. Once the "put" method notifies the other methods (for example, a get() method), then get() method can exit its guarded state and proceed with a call. Once the queue is empty, then get() method will enter a guarded state once again.
