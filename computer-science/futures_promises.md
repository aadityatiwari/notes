[Futures and promises](https://en.wikipedia.org/wiki/Futures_and_promises)
--------------------------------------------------------------------------

**Future**, **promise**, **delay**, and **deferred** refer to constructs used for [synchronizing](https://en.wikipedia.org/wiki/Synchronization_(computer_science)) program [execution](https://en.wikipedia.org/wiki/Execution_(computing)) 

They describe an object that acts as a proxy for a result that is initially unknown.

A future is a *read-only* placeholder view of a variable, while a promise is a writable, [single assignment](https://en.wikipedia.org/wiki/Single_assignment) container which sets the value of the future.

A future may be defined without specifying which specific promise will set its value, and different possible promises may set the value of a given future, though this can be done only once for a given future.

In other cases a future and a promise are created together and associated with each other: the future is the value, the promise is the function that sets the value – essentially the return value (future) of an asynchronous function (promise). 

Setting the value of a future is also called *resolving*, *fulfilling*, or *binding* it.

Futures and promises originated in [functional programming](https://en.wikipedia.org/wiki/Functional_programming) and related paradigms (such as [logic programming](https://en.wikipedia.org/wiki/Logic_programming)) to decouple a value (a future) from how it was computed (a promise), allowing the computation to be done more flexibly, notably by parallelizing it.

Later, it found use in [distributed computing](https://en.wikipedia.org/wiki/Distributed_computing), in reducing the latency from communication round trips. Later still, it gained more use by allowing writing asynchronous programs in [direct style](https://en.wikipedia.org/wiki/Direct_style), rather than in [continuation-passing style](https://en.wikipedia.org/wiki/Continuation-passing_style).

Use of futures may be *implicit* (any use of the future automatically obtains its value, as if it were an ordinary [reference](https://en.wikipedia.org/wiki/Reference_(programming))) or *explicit* (the user must call a function to obtain the value, such as the get method of [java.util.concurrent.Future](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Future.html) or [java.util.concurrent.CompletableFuture](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html) in [Java](https://en.wikipedia.org/wiki/Java_(programming_language))).

Obtaining the value of an explicit future can be called *stinging* or *forcing*.
