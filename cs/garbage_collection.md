
Garbage collection (computer science)
-------------------------------------

-   Garbage collection (GC) is a form of automatic [memory management](https://en.wikipedia.org/wiki/Memory_management).  The *garbage collector*, or just *collector*, attempts to reclaim [*garbage*](https://en.wikipedia.org/wiki/Garbage_(computer_science)), or memory occupied by [objects](https://en.wikipedia.org/wiki/Object_(computer_science)) that are no longer in use by the [program](https://en.wikipedia.org/wiki/Application_software). 

-   The basic principles of garbage collection are to find data objects in a program that cannot be accessed in the future, and to reclaim the resources used by those objects.

-   Garbage collection frees the programmer from manually dealing with memory de-allocation. As a result, certain categories of bugs are eliminated or substantially reduced: *[Dangling pointer](https://en.wikipedia.org/wiki/Dangling_pointer) bugs, Double free bugs, Memory leaks.*

-   Garbage collection has certain disadvantages, including consuming additional resources, performance impacts, possible stalls in program execution, and incompatibility with manual resource management.

-   A study shows that GC needs five times the memory to compensate for the overhead of consuming computing resources in deciding which memory to free and to perform as fast as explicit memory management.

-   The impact on performance was also given by Apple as a reason for not adopting garbage collection in [iOS](https://en.wikipedia.org/wiki/IOS) despite being the most desired feature.

-   The moment when the garbage is actually collected can be unpredictable, resulting in stalls (pauses to shift/free memory) scattered throughout a session. Unpredictable stalls can be unacceptable in [real-time environments](https://en.wikipedia.org/wiki/Real-time_computing), in [transaction processing](https://en.wikipedia.org/wiki/Transaction_processing), or in interactive programs. Incremental, concurrent, and real-time garbage collectors address these problems, with varying trade-offs.

-   [**Tracing garbage collection**](https://en.wikipedia.org/wiki/Tracing_garbage_collection) is the most common type of garbage collection, so much so that "garbage collection" often refers to tracing garbage collection, rather than other methods such as **[reference counting](https://en.wikipedia.org/wiki/Reference_counting). **

-   The overall strategy in **tracing garbage collection** mechanism consists of determining which objects should be garbage collected by tracing which objects are reachable by a chain of references from certain root objects, and considering the rest as garbage and collecting them. However, there are a large number of algorithms used in implementation, with widely varying complexity and performance characteristics.

-   **Reference counting** is a form of garbage collection whereby each object has a count of the number of references to it. Garbage is identified by having a reference count of zero.

-   Disadvantages to reference counting: Cycles, space overhead, speed overhead (increment/decrement), requires atomicity especially in multithreaded environment.

-   [Escape analysis](https://en.wikipedia.org/wiki/Escape_analysis) can be used to convert [heap allocations](https://en.wikipedia.org/wiki/Heap_allocation) to [stack allocations](https://en.wikipedia.org/wiki/Stack_allocation), thus reducing the amount of work needed to be done by the garbage collector. This is done using a compile-time analysis to determine whether an object allocated within a function is not accessible outside of it (i.e. escape) to other functions or threads. In such a case the object may be allocated directly on the thread stack and released when the function returns, reducing its potential garbage collection overhead.

-   Generational garbage collection schemes are based on the empirical observation that most objects die young.

-   In generational garbage collection two or more allocation regions (generations) are kept, which are kept separate based on object's age. New objects are created in the "young" generation that is regularly collected, and when a generation is full, the objects that are still referenced from older regions are copied into the next oldest generation. Occasionally a full scan is performed.

-   [Lisp](https://en.wikipedia.org/wiki/Lisp_(programming_language)) is the first [functional programming language](https://en.wikipedia.org/wiki/Functional_programming_language) and the first language to introduce garbage collection.

-   Lisp is the second-oldest [high-level programming language](https://en.wikipedia.org/wiki/High-level_programming_language) in widespread use today. Only [Fortran](https://en.wikipedia.org/wiki/Fortran) is older, by one year
