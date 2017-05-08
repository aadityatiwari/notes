[Asynchronous method invocation](https://en.wikipedia.org/wiki/Asynchronous_method_invocation)
------------------

**Asynchronous method invocation** (**AMI**), also known as **asynchronous method calls** or the **asynchronous pattern** is a [design pattern](https://en.wikipedia.org/wiki/Software_design_pattern) in which the call site is not [blocked](https://en.wikipedia.org/wiki/Blocking_(computing)) while waiting for the called code to finish. Instead, the calling thread is notified when the reply arrives. Polling for a reply is an undesired option.

In most programming languages a called method is executed synchronously, i.e. in the [thread of execution](https://en.wikipedia.org/wiki/Thread_(computer_science)) from which it is invoked. If the method needs a long time to completion, e.g. because it is loading data over the internet, the calling thread is blocked until the method has finished. When this is not desired, it is possible to start a "worker thread" and invoke the method from there.

AMI solves this problem in that it augments a potentially long-running ("synchronous") object method with an "asynchronous" variant that returns immediately, along with additional methods that make it easy to receive notification of completion, or to wait for completion at a later time.

One common use of AMI is in the [active object](https://en.wikipedia.org/wiki/Active_object) design pattern.

Alternatives are synchronous method invocation and [future objects](https://en.wikipedia.org/wiki/Futures_and_promises).

An example for an application that may make use of AMI is a web browser that needs to display a web page even before all images are loaded.

FutureTask class in [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) use [events](https://en.wikipedia.org/wiki/Event_(synchronization_primitive)) to solve the same problem. This pattern is a variant of AMI whose implementation carries more overhead but useful for objects representing s/w components.
