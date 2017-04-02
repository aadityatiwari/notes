
[Java Platform](https://en.wikipedia.org/wiki/Java_(software_platform))
-----------------------------------------------------------------------

-   Writing in the [Java programming language](https://en.wikipedia.org/wiki/Java_(programming_language)) is the primary way to produce code that will be deployed as [byte code](https://en.wikipedia.org/wiki/Java_byte_code) in a [Java Virtual Machine](https://en.wikipedia.org/wiki/Java_Virtual_Machine) (JVM)

-   Byte code [compilers](https://en.wikipedia.org/wiki/Compiler) are also available for other languages, including [Ada](https://en.wikipedia.org/wiki/Ada_(programming_language)), [JavaScript](https://en.wikipedia.org/wiki/JavaScript), [Python](https://en.wikipedia.org/wiki/Python_(programming_language)), and [Ruby](https://en.wikipedia.org/wiki/Ruby_(programming_language)). In addition, several languages have been designed to run natively on the JVM, including [Scala](https://en.wikipedia.org/wiki/Scala_(programming_language)), [Clojure](https://en.wikipedia.org/wiki/Clojure) and [Apache Groovy](https://en.wikipedia.org/wiki/Groovy_(programming_language)).

-   The Java platform is a suite of programs that facilitate developing and running programs written in the [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) programming language. 

-   A Java platform includes an execution engine (called a [virtual machine](https://en.wikipedia.org/wiki/Virtual_machine)), a compiler and a set of [libraries](https://en.wikipedia.org/wiki/Library_(computing)).

-   The essential components in the platform are the Java language compiler, the libraries, and the runtime environment in which Java intermediate bytecode executes according to the rules laid out in the virtual machine specification.

-   There is a JIT (Just In Time) compiler within the JVM. The JIT compiler translates the Java bytecode into native processor instructions at run-time and caches the native code in memory during execution.

-   The use of bytecode as an intermediate language permits Java programs to run on any platform that has a virtual machine available.

-   Although Java programs are [cross-platform](https://en.wikipedia.org/wiki/Cross-platform) or platform independent, the code of the Java Virtual Machines (JVM) that executes these programs is not. Every supported operating platform has its own JVM.

-   As the Java platform is not dependent on any specific operating system, applications cannot rely on any of the pre-existing OS libraries. Instead, the Java platform provides a comprehensive set of its own standard class libraries containing many of the same reusable functions commonly found in modern operating systems. Most of the system library is also written in Java.

-   Java class libraries serve three purposes:

<!-- -->

-   Provides standard set of functions to perform common tasks.

-   Provides abstract interface to hardware and OS related tasks like network and file access. The java.net and java.io libraries implement an abstraction layer in native OS code, and then provide a standard interface for the Java applications to perform those tasks.

-   Class libraries gracefully handle the absent components when some underlying platform does not support all of the features a Java application expects.

<!-- -->

-   Other languages/interpreters which runs on JVM are:

    -   BeanShell – A lightweight scripting language for Java

    -   Clojure – A dialect of the Lisp programming language

    -   Groovy – A dynamic language with features similar to those of Python, Ruby, Perl, and Smalltalk

    -   JRuby – A Ruby interpreter

    -   Jython – A Python interpreter

    -   Kotlin – An industrial programming language for JVM with full Java interoperability

    -   Rhino – A JavaScript interpreter

    -   Scala – A multi-paradigm programming language designed as a "better Java"

    -   Gosu – A general-purpose JVM-based programming language released under the Apache License 2.0

-   Google's [Android](https://en.wikipedia.org/wiki/Android_(operating_system)) operating system uses the Java language, not its class libraries; therefore the Android platform cannot be called Java. Android executes the code on the [ART VM](https://en.wikipedia.org/wiki/Android_Runtime) (formerly the [Dalvik VM](https://en.wikipedia.org/wiki/Dalvik_(software)) up to Android 4.4.4) instead of the Java VM.

-   Java methods cannot be bigger than 64KB. Java lacks native [unsigned integer](https://en.wikipedia.org/wiki/Integer_(computer_science)) types.

-   Java arrays cannot have more than 2<sup>31</sup>−1 (about 2.1 billion) elements. Arrays must be indexed by int values... An attempt to access an array component with a long index value results in a compile-time error.
