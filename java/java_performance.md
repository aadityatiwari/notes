
[Java performance](https://en.wikipedia.org/wiki/Java_performance)
------------------------------------------------------------------

-   Early JVMs always interpreted [Java bytecodes](https://en.wikipedia.org/wiki/Java_bytecode). This had a large performance penalty of between a factor 10 and 20 for Java versus C in average applications. To combat this, a just-in-time (JIT) compiler was introduced into Java 1.1. Due to the high cost of compiling, an added system called [HotSpot](https://en.wikipedia.org/wiki/HotSpot) was introduced in Java 1.2 and was made the default in Java 1.3.

-   Using this framework, the [Java virtual machine](https://en.wikipedia.org/wiki/Java_virtual_machine) continually analyses program performance for ***hot spots* **which are executed frequently or repeatedly. These are then targeted for [optimizing](https://en.wikipedia.org/wiki/Optimization_(computer_science)), leading to high performance execution with a minimum of overhead for less performance-critical code.

-   **Adaptive optimizing** is a method in computer science that performs [dynamic recompilation](https://en.wikipedia.org/wiki/Dynamic_recompilation) of parts of a program based on the current execution profile.

-   **Compressed Oops** allows Java 5.0+ to address up to 32 GB of heap with 32-bit references. Java does not support access to individual bytes, only objects which are 8-byte aligned by default. Because of this, the lowest 3 bits of a heap reference will always be 0.

-   By lowering the resolution of 32-bit references to 8 byte blocks, the addressable space can be increased from 4GB to 32 GB.

-   This significantly reduces memory use compared to using 64-bit references as Java uses references much more than some languages like C++. Java 8 supports larger alignments such as 16-byte alignment to support up to 64 GB with 32-bit references.

-   Bytecode verification is performed lazily i.e. classes bytecodes are only loaded and verified when the specific class is loaded and prepared for use, and not at the beginning of the program.

-   As the Java library does not know which methods will be used by more than one thread, the standard library **lock [blocks](https://en.wikipedia.org/wiki/Block_(programming)) when needed** in a multithreaded environment.

-   When [JRE](https://en.wikipedia.org/wiki/Java_Runtime_Environment) is installed, the installer loads a set of classes from the system [JAR](https://en.wikipedia.org/wiki/JAR_(file_format)) file (the JAR file holding entire Java class library, called rt.jar) into a private internal representation, and dumps that representation to a file, called a "shared archive". During subsequent JVM invocations, this shared archive is [memory-mapped](https://en.wikipedia.org/wiki/Memory-mapped_file) in, saving the cost of loading those classes and allowing much of the JVM's [metadata](https://en.wikipedia.org/wiki/Metadata#Program_metadata) for these classes to be shared among multiple JVM processes.
