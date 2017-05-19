[Thread-local_storage](https://en.wikipedia.org/wiki/Thread-local_storage)
--------------

**Thread-local storage** (**TLS**) is a computer programming method that uses [static](https://en.wikipedia.org/wiki/Static_memory_allocation) or global [memory](https://en.wikipedia.org/wiki/Computer_storage) local to a [thread](https://en.wikipedia.org/wiki/Thread_(computing)).

TLS is used in some places where ordinary, single-threaded programs would use [global variables](https://en.wikipedia.org/wiki/Global_variables) but where this would be inappropriate in multithreaded cases. 

Use case: To avoid a [race condition](https://en.wikipedia.org/wiki/Race_condition), every access to this global variable would have to be protected by a mutex. Alternatively, each thread might accumulate into a thread-local variable (that, by definition, cannot be read from or written to from other threads, implying that there can be no race conditions). Threads then only have to synchronize a final accumulation from their own thread-local variable into a single, truly global variable.

In [Java](https://en.wikipedia.org/wiki/Java_(programming_language)), thread-local variables are implemented by the [ThreadLocal](https://docs.oracle.com/javase/8/docs/api/java/lang/ThreadLocal.html) [class](https://en.wikipedia.org/wiki/Class_(computer_science)) object. 

ThreadLocal holds variable of type T, which is accessible via get/set methods. 

```
 private static final ThreadLocal<Integer> myThreadLocalInteger = new ThreadLocal<Integer>();
```

Each Thread object stores a (non-thread-safe) map of ThreadLocal objects to their values (as opposed to each ThreadLocal having a map of Thread objects to values and incurring a performance overhead).

Thread-local variables differ from their normal counterparts in that each thread that accesses one (via its get or set method) has its own, independently initialized copy of the variable.

ThreadLocal instances are typically private static fields in classes that wish to associate state with a thread (e.g., a user ID or Transaction ID).