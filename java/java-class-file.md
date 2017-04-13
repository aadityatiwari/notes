
Java class file
---------------

-   A **Java class file** is a [file](https://en.wikipedia.org/wiki/Computer_file) (with the .class [filename extension](https://en.wikipedia.org/wiki/Filename_extension)) containing [Java bytecode](https://en.wikipedia.org/wiki/Java_bytecode) that can be executed on the [Java Virtual Machine (JVM)](https://en.wikipedia.org/wiki/Java_Virtual_Machine).

-   There are 10 basic sections to the Java Class File structure:

- >  **Magic Number**: 0xCAFEBABE
 >
- >   **Version of Class File Format**: the minor and major versions of the class file

- >   **Constant Pool**: Pool of constants for the class

- >   **Access Flags**: for example whether the class is abstract, static, etc.

- >   **This Class**: The name of the current class

- >   [**Super Class**](https://en.wikipedia.org/wiki/Superclass_(computer_science)): The name of the super class

- >  [**Interfaces**](https://en.wikipedia.org/wiki/Interface_(object-oriented_programming)): Any interfaces in the class

- >   **Fields**: Any fields in the class

- >   [**Methods**](https://en.wikipedia.org/wiki/Method_(computing)): Any methods in the class

- >   **Attributes**: Any attributes of the class (for example the name of the source file, etc.)

-   The constant pool table is where most of the literal constant values are stored. This includes values such as numbers of all sorts, strings, identifier names, references to classes and methods, and type descriptors. All indexes, or references, to specific constants in the constant pool table are given by 16-bit (type u2) numbers, where index value 1 refers to the first constant in the table (index value 0 is invalid).
