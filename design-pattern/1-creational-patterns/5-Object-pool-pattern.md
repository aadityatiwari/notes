[Object pool pattern](https://en.wikipedia.org/wiki/Object_pool_pattern)
-----------------

Object pool pattern uses a set of initialized objects kept ready to use a "pool" rather than allocating and destroying them on demand.

A client of the pool will request an object from the pool and perform operations on the returned object. When the client has finished, it returns the object to the pool rather than destroying it.

Object pooling can offer a significant performance boost in situations where the cost of initializing a class instance is high and the rate of instantiation and destruction of a class is high – in this case objects can frequently be reused, and each reuse saves a significant amount of time.

These benefits are mostly true for objects that are expensive with respect to time, such as database connections, socket connections, threads and large graphic objects like fonts or bitmaps.

Object pools complicate object lifetime, as objects obtained from and returned to a pool are not actually created or destroyed at this time, and thus require care in implementation.

Some publications do not recommend using object pooling with Java, especially for objects that only use memory and hold no external resources.

Opponents usually say that object allocation is relatively fast in modern languages with garbage collectors;

While the operator **new** needs only ten instructions, the classic **new - delete** pair found in pooling designs requires hundreds of them as it does more complex work.

Most garbage collectors **scan "live" object references**, and not the memory that these objects use for their content. This means that any number of "dead" objects without references can be discarded with little cost.

In contrast, keeping a large number of "live" but unused objects increases the duration of garbage collection.

Java supports [thread pooling](https://en.wikipedia.org/wiki/Thread_pool_pattern) via **java.util.concurrent.ExecutorService** and other related classes.
