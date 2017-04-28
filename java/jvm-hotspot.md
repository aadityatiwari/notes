[HotSpot JVM](https://en.wikipedia.org/wiki/HotSpot)
---------------

Sun's [JRE](https://en.wikipedia.org/wiki/JRE) features two virtual machines, one called *Client* and the other *Server*. 

The Client version is tuned for quick loading. It makes use of interpretation. 

The Server version loads more slowly, putting more effort into producing highly optimized [JIT compilations](https://en.wikipedia.org/wiki/Just-in-time_compilation) that yield higher performance.

Both VMs compile only often-run methods, using a configurable invocation-count threshold to decide which methods to compile.

Starting from Java 8, Tiered compilation is the default for the server VM as it provides faster startup time.

The HotSpot Java virtual machine is written in [C++](https://en.wikipedia.org/wiki/C%2B%2B)

Hotspot provides:

> A class loader
>
> A bytecode interpreter
>
> Client and Server virtual machines, optimized for their respective uses
>
> Several garbage collectors
>
> A set of supporting runtime libraries


Porting HotSpot is difficult because the code, while primarily written in [C++](https://en.wikipedia.org/wiki/C%2B%2B), contains a lot of [assembly language](https://en.wikipedia.org/wiki/Assembly_language). To remedy this, the [IcedTea](https://en.wikipedia.org/wiki/IcedTea) project has developed a generic port of the HotSpot [interpreter](https://en.wikipedia.org/wiki/Interpreter_(computing))  called *zero-assembler Hotspot* (or *zero*), with almost no assembly code.
