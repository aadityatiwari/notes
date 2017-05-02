[Façade Pattern](https://en.wikipedia.org/wiki/Facade_pattern)
--------------

The **facade pattern** (also spelled *façade*) is a [software design pattern](https://en.wikipedia.org/wiki/Software_design_pattern) commonly used with [object-oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming). The name is by analogy to an architectural [façade](https://en.wikipedia.org/wiki/Facade).

A facade is an object that provides a simplified interface to a larger body of code, such as a [class library](https://en.wikipedia.org/wiki/Class_library). 

A façade can:
-   make a [software library](https://en.wikipedia.org/wiki/Software_library) easier to use, understand and test, since the facade has convenient methods for common tasks,
-   make the library more readable, for the same reason,
-   reduce [dependencies](https://en.wikipedia.org/wiki/Coupling_(computer_programming)) of outside code on the inner workings of a library, since most code uses the facade, thus allowing more flexibility in developing the system,
-   wrap a poorly designed collection of [APIs](https://en.wikipedia.org/wiki/Application_programming_interface) with a single well-designed API.

The Facade design pattern is often used when a system is very complex or difficult to understand because the system has a large number of interdependent classes or its source code is unavailable. 

This pattern hides the complexities of the larger system and provides a simpler interface to the client.

It typically involves a single wrapper class that contains a set of members required by client. These members access the system on behalf of the facade client and hide the implementation details.

A Facade is used when an easier or simpler interface to an underlying object is desired.

> <img src="https://upload.wikimedia.org/wikipedia/en/5/57/Example_of_Facade_design_pattern_in_UML.png" alt="Example of Facade design pattern in UML.png" width="576" height="384" />

>  The facade class abstracts Packages 1, 2, and 3 from the rest of the application.

*Example:*
---------

```java
/* Complex parts */

class CPU {
    public void freeze() { ... }
    public void jump(long position) { ... }
    public void execute() { ... }
}

class HardDrive {
    public byte[] read(long lba, int size) { ... }
}

class Memory {
    public void load(long position, byte[] data) { ... }
}

/* Facade */
class ComputerFacade {
    private CPU processor;
    private Memory ram;
    private HardDrive hd;

    public ComputerFacade() {
        this.processor = new CPU();
        this.ram = new Memory();
        this.hd = new HardDrive();
    }

    public void start() {
        processor.freeze();
        ram.load(BOOT_ADDRESS, hd.read(BOOT_SECTOR, SECTOR_SIZE));
        processor.jump(BOOT_ADDRESS);
        processor.execute();
    }
}

/* Client */

class You {
    public static void main(String[] args) {
        ComputerFacade computer = new ComputerFacade();
        computer.start();
    }
}
```
