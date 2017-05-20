[Thread pool](https://en.wikipedia.org/wiki/Thread_pool)
------------

A **thread pool** is a [software design pattern](https://en.wikipedia.org/wiki/Software_design_pattern) for achieving [concurrency](https://en.wikipedia.org/wiki/Concurrency_(computer_science)) of execution in a computer program.

Often also called a **replicated workers** or **worker-crew model**, a thread pool maintains multiple [threads](https://en.wikipedia.org/wiki/Thread_(computer_science)) waiting for [tasks](https://en.wikipedia.org/wiki/Task_(computers)) to be allocated for [concurrent](https://en.wikipedia.org/wiki/Concurrent_computing) execution by the supervising program.

By maintaining a pool of threads, the model increases performance and avoids latency in execution due to frequent creation and destruction of threads for short-lived tasks.

The number of available threads is tuned to the computing resources available to the program, such as [parallel](https://en.wikipedia.org/wiki/Parallel_computing) [processors](https://en.wikipedia.org/wiki/Central_processing_unit), [cores](https://en.wikipedia.org/wiki/Multi-core_processor), [memory](https://en.wikipedia.org/wiki/Computer_memory), and network sockets.

A common method of [scheduling](https://en.wikipedia.org/wiki/Scheduling_(computing)) tasks for thread execution is a [synchronized](https://en.wikipedia.org/wiki/Synchronization_(computer_science)) [queue](https://en.wikipedia.org/wiki/Queue_(data_structure)), known as a [task queue](https://en.wikipedia.org/wiki/Task_queue). 

The threads in the pool remove waiting tasks from the queue, and place them into a completed task queue after completion of execution.

The size of a thread pool is the number of threads kept in reserve for executing tasks. It is usually a tunable parameter of the application, adjusted to optimize program performance.

The primary benefit of a thread pool over creating a new thread for each task is that thread creation and destruction overhead is restricted to the initial creation of the pool, which may result in better [performance](https://en.wikipedia.org/wiki/Performance_tuning) and better system [stability](https://en.wikipedia.org/wiki/Stability_Model).

> <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/0c/Thread_pool.svg/400px-Thread_pool.svg.png" alt="https://upload.wikimedia.org/wikipedia/commons/thumb/0/0c/Thread_pool.svg/400px-Thread_pool.svg.png" width="400" height="207" />
>
> *A sample thread pool (green boxes) with waiting tasks (blue) and completed tasks (yellow)*


- Creating too many threads wastes resources and costs time creating the unused threads.
- Destroying too many threads requires more time later when creating them again.
- Creating threads too slowly might result in poor client performance (long wait times).
- Destroying threads too slowly may starve other processes of resources.

