[Java Bytecode](https://en.wikipedia.org/wiki/Java_bytecode)
---------

-   Java bytecode is the instruction set of the Java virtual machine.
-   Each bytecode is composed of one, or in some cases two bytes that represent the instruction (opcode), along with zero or more bytes for passing parameters.
-   Of the 255 possible byte-long opcodes, as of 2015, 198 are in use (~78%), 54 are reserved for future use (~21%), and 3 instructions (~1%) are set aside as permanently unimplemented
-   Instructions fall into a number of broad groups:
    * Load and store (e.g. aload\_0, istore)
    * Arithmetic and logic (e.g. ladd, fcmpl)
    * Type conversion (e.g. i2b, d2i)
    * Object creation and manipulation (new, putfield)
    * Operand stack management (e.g. swap, dup2)
    * Control transfer (e.g. ifeq, goto)
    * Method invocation and return (e.g. invokespecial, areturn)
    * There are also a few instructions for a number of more specialized tasks such as exception throwing, synchronization, etc.

-   If executing Java bytecode in a Java virtual machine is not desirable, a developer can also compile Java source code or Java bytecode directly to native machine code with tools such as the GNU Compiler for Java. Some processors can execute Java bytecode natively. Such processors are known as Java processors.

-   A **Java processor** is the implementation of the [Java virtual machine](https://en.wikipedia.org/wiki/Java_virtual_machine) (JVM) in hardware. In other words, the [Java bytecode](https://en.wikipedia.org/wiki/Java_bytecode) that makes up the instruction set of the abstract machine becomes the instruction set of a concrete machine.

-   Example: Java code and it’s byte code

>	*Java code:*
```java
 outer:
 for (int i = 2; i &lt; 1000; i++) {
     for (int j = 2; j &lt; i; j++) {
        if(i % j == 0)
            continue outer;
        }
    System.out.println (i);
}
```

>	*Byte code:*

```assembly
 0: iconst\_2
 1: istore\_1
 2: iload\_1
 3: sipush 1000
 6: if\_icmpge 44
 9: iconst\_2
 10: istore\_2
 11: iload\_2
 12: iload\_1
 13: if\_icmpge 31
 16: iload\_1
 17: iload\_2
 18: irem
 19: ifne 25
 22: goto 38
 25: iinc 2, 1
 28: goto 11
 31: getstatic \#84; // Field java/lang/System.out:Ljava/io/PrintStream;
 34: iload\_1
 35: invokevirtual \#85; // Method java/io/PrintStream.println:(I)V
 38: iinc 1, 1
 41: goto 2
 44: return
```
