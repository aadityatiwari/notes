[Java Virtual Machine](https://en.wikipedia.org/wiki/Java_virtual_machine)
-----------------

A **Java virtual machine** (**JVM**) is [an abstract computing machine](https://en.wikipedia.org/wiki/Virtual_machine#Process_virtual) that enables a computer to run a [Java](https://en.wikipedia.org/wiki/Java_(software_platform)) program. There are three notions of the JVM specification, implementation and instance. 

An instance of a JVM is an implementation running in a [process](https://en.wikipedia.org/wiki/Process_(computing)) that executes a computer program compiled into [Java bytecode](https://en.wikipedia.org/wiki/Java_bytecode).

**Java Runtime Environment** (**JRE**) is a software package that contains what is required to run a Java program. It includes a Java Virtual Machine implementation together with an implementation of the [Java Class Library](https://en.wikipedia.org/wiki/Java_Class_Library).

[**Java Development Kit**](https://en.wikipedia.org/wiki/Java_Development_Kit) (**JDK**) is a superset of a JRE and contains tools for Java programmers, e.g. a [javac](https://en.wikipedia.org/wiki/Javac) compiler. 

<img src="https://upload.wikimedia.org/wikipedia/commons/d/dd/JvmSpec7.png" alt="https://upload.wikimedia.org/wikipedia/commons/d/dd/JvmSpec7.png" width="702" height="425" />

**Sun's [JRE](https://en.wikipedia.org/wiki/JRE) features two virtual machines, one called *Client* and the other *Server***. The Client version is tuned for quick loading. It makes use of interpretation. The Server version loads more slowly, putting more effort into producing highly optimized [JIT compilations](https://en.wikipedia.org/wiki/Just-in-time_compilation) that yield higher performance. Both VMs compile only often-run methods, using a configurable invocation-count threshold to decide which methods to compile.

**HotSpot became the default Sun JVM in Java 1.3.**

Tiered compilation, an option introduced in Java 7, uses both the client and server compilers in tandem to provide faster startup time than the server compiler, but similar or better peak performance. **Tiered compilation is the default for the server VM since Java 8.**

A class loader implementation must recognize class files. There are two types of class loader: **bootstrap class loader** and **user defined class loader.** Every JVM implementation must have a bootstrap class loader, capable of loading trusted classes. 

The **class loader performs three basic activities** in this strict order:

1.  ***Loading***: finds and imports the binary data for a type

2.  ***Linking***: performs verification, preparation, and (optionally) resolution

- >   **Verification**: ensures the correctness of the imported type

- >  **Preparation**: allocates memory for class variables and initializing the memory to default values

- >  **Resolution**: transforms symbolic references from the type into direct references.

3.  ***Initialization***: invokes Java code that initializes class variables to their proper starting values.

The JVM has [instructions](https://en.wikipedia.org/wiki/Instruction_(computer_science)) for the following groups of tasks:

- >  [Load and store](https://en.wikipedia.org/wiki/Load/store_architecture)

- >  [Arithmetic](https://en.wikipedia.org/wiki/Arithmetic)

- >   [Type conversion](https://en.wikipedia.org/wiki/Type_conversion)

- >   [Object creation and manipulation](https://en.wikipedia.org/wiki/Dynamic_memory_allocation)

- >  [Operand stack management (push / pop)](https://en.wikipedia.org/wiki/Stack_(abstract_data_type))

- >   [Control transfer (branching)](https://en.wikipedia.org/wiki/Branch_(computer_science))

- >   [Method invocation and return](https://en.wikipedia.org/wiki/Subroutine)

- >  [Throwing exceptions](https://en.wikipedia.org/wiki/Exception_handling)

- >   [Monitor-based concurrency](https://en.wikipedia.org/wiki/Monitor_(synchronization))


A class file contains JVM instructions ([Java byte code](https://en.wikipedia.org/wiki/Java_byte_code)) and a symbol table, as well as other ancillary information. The class file format is the hardware and OS-independent binary format used to represent compiled classes and interfaces.

There are several JVM languages other than Java, both old languages ported to JVM and completely new languages. For e.g: JRuby, Jython, Clojure, Groovy, Scala, etc.

Bytecode verifier: The JVM *verifies* all bytecode before it is executed. This verification consists primarily of three types of checks:

- >   Branches are always to valid locations

- >   Data is always initialized and references are always type-safe

- >  Access to *private* or *package private* data and methods is rigidly controlled


Every [hardware architecture](https://en.wikipedia.org/wiki/Hardware_architecture) needs a different Java bytecode [interpreter](https://en.wikipedia.org/wiki/Interpreter_(computing)). When Java bytecode is executed by an interpreter, the execution will always be slower than the execution of the same program compiled into native machine language. This problem is mitigated by [just-in-time (JIT) compilers](https://en.wikipedia.org/wiki/Just-in-time_compilation) for executing Java bytecode.

JIT compiler may translate Java bytecode into native machine language while executing the program. The translated parts of the program can then be executed much more quickly than they could be interpreted. This technique gets applied to those parts of a program frequently executed. This way a JIT compiler can significantly speed up the overall execution time.

To speed-up code execution, Oracle’s JVM HotSpot relies on just-in-time compilation. To speed-up object allocation and garbage collection, HotSpot uses generational heap.

In HotSpot the heap is divided into *generations*:

- >   The ***young generation*** stores short-lived [objects](https://en.wikipedia.org/wiki/Object_(computer_science)) that are created and immediately [garbage collected](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)).

- >   Objects that persist longer are moved to the ***old generation*** (**also** **called** the ***tenured generation***). This memory is subdivided into (two) Survivors spaces where the objects that survived the first and next garbage collections are stored.

- >  The permanent generation (or permgen) was used for class definitions and associated metadata prior to Java 8.

- >  Permanent generation was not part of the heap. The permanent generation was removed from Java 8.


List of Java virtual machines =&gt; [List\_of\_Java\_virtual\_machines](https://en.wikipedia.org/wiki/List_of_Java_virtual_machines)

Apart from the [Java language](https://en.wikipedia.org/wiki/Java_(programming_language)) itself, the most common or well-known JVM languages are:

- >   [**Clojure**](https://en.wikipedia.org/wiki/Clojure), a [functional](https://en.wikipedia.org/wiki/Functional_programming) [Lisp](https://en.wikipedia.org/wiki/Lisp_(programming_language)) dialect

- >   [**Apache** **Groovy**](https://en.wikipedia.org/wiki/Groovy_(programming_language)), a dynamic programming and [scripting language](https://en.wikipedia.org/wiki/Scripting_language)

- >  [**Scala**](https://en.wikipedia.org/wiki/Scala_(programming_language)), a statically-typed [object-oriented](https://en.wikipedia.org/wiki/Object-oriented_programming) and [functional programming](https://en.wikipedia.org/wiki/Functional_programming) language

- >   [**Kotlin**](https://en.wikipedia.org/wiki/Kotlin_(programming_language)), a [statically-typed](https://en.wikipedia.org/wiki/Static_type_checking) language

- >   [**JRuby**](https://en.wikipedia.org/wiki/JRuby), an implementation of [Ruby](https://en.wikipedia.org/wiki/Ruby_(programming_language))

- >   [**Jython**](https://en.wikipedia.org/wiki/Jython), an implementation of [Python](https://en.wikipedia.org/wiki/Python_(programming_language))
