[Java objects memory usage](http://www.javamex.com/tutorials/memory/object_memory_usage.shtml)
-----------

The **heap memory used by a Java object in Hotspot** consists of:
-   an **object header**, consisting of a few bytes of "housekeeping" information;
-   memory for **primitive fields**, according to their size;
-   memory for **reference fields** (4 bytes each);
-   **padding**: potentially a few "wasted" unused bytes after the object data, to make every object start at an address that is a convenient multiple of bytes and reduce the number of bits required to represent a pointer to an object.

Instances of an object on the Java heap don't just take up memory for their actual fields. Inevitably, they also require some "housekeeping" information, such as recording an object's class, ID and status flags such as whether the object is currently reachable, currently synchronization-locked etc.

In Hotspot **a normal object requires 8 bytes of "housekeeping" space** and **arrays require 12 bytes** (the same as a normal object, **plus 4 bytes for the array length**).

In Hotspot, every object occupies a number of bytes that is a **multiple of 8**. If the number of bytes required by an object for its header and fields is not a multiple 8, then you round up to the next multiple of 8.

**A bare Object takes up 8 bytes.** An instance of a class with a single boolean field takes up 16 bytes: 8 bytes of header, 1 byte for the boolean and 7 bytes of "padding" to make the size up to a multiple of 8.

An instance with eight boolean fields will also take up 16 bytes: 8 for the header, 8 for the booleans; since this is already a multiple of 8, no padding is needed.

For example, let's consider a 10x10 int array. Firstly, the "outer" array has its 12-byte object header followed by space for the 10 elements. Those elements are object references to the 10 arrays making up the rows. That comes to 12+4\*10=52 bytes, which must then be rounded up to the next multiple of 8, giving 56. Then, each of the 10 rows has its own 12-byte object header, 4\*10=40 bytes for the actual row of ints, and again, 4 bytes of padding to bring the total for that row to a multiple of 8. So in total, that gives 11\*56=616 bytes. That's a bit bigger than if you'd just counted on 10\*10\*4=400 bytes for the hundred "raw" ints themselves.

If you have 32-bit references, you can address up to 2^32 or 4 GB of memory (practically you get less though, more like 3.5 GB). If you have 64-bit references, you can address 2^64, which is terrabytes of memory. However, with 64-bit references, everything tends to slow down and take more space. This is due to the overhead of 32-bit processors dealing with 64-bits, and on all processors more GC cycles due to less space and more garbage collection.

So, the creators took a middle ground and decided on **35-bit references**, which allow up to 2^35 or 32 GB of memory and take up less space so to have the same performance benefits of 32-bit references. This is done by taking a 32-bit reference and shifting it left 3 bits when reading, and shifting it right 3 bits when storing references. That means all objects must be aligned on 2^3 boundaries (8 bytes). These are called ***compressed ordinary object pointers* or compressed oops**. (*-XX:+UsedCompressedOops* flag)

Min. String memory usage of a java String in hotspot Java 6 VM is as follows:

-   Minimum String memory usage (bytes) = **8 \* (int) (((*(no. of chars)* \* 2) + 45) / 8)**
-   In other way,
    * multiply the number of characters of the String by two;
    * add 38;
    * if the result is not a multiple of 8, round up to the next multiple of 8;
    * result is generally the minimum number of bytes taken up on the heap by the String.

To understand the above calculation, we need to start by looking at the fields on a String object. A String contains the following:

-   a char array— thus a separate object— containing the actual characters;
-   an integer offset into the array at which the string starts;
-   the length of the string;
-   another int for the cached calculation of the hash code.

Even if the string contains no characters, it will require 4 bytes for the char array reference, plus 3\*4=12 bytes for the three int fields, plus 8 bytes of object header. This gives 24 bytes (which is a multiple of 8 so no "padding" bytes are needed so far). Then, the (empty) char array will require a further 12 bytes (arrays have an extra 4 bytes to store their length), plus in this case 4 bytes of padding to bring the memory used by the char array object up to a multiple of 16. So in total, **an empty string uses 40 bytes.**

If the string contains, say, 17 characters, then the String object itself still requires 24 bytes. But now the char array requires 12 bytes of header plus 17\*2=34 bytes for the seventeen chars. Since 12+34=46 isn't a multiple of 8, we also need to round up to the next multiple of 8 (48). So overall, our 17-character String will use up 48+24 = 72 bytes. 

When we create a substring of an existing String, the newly created substring is a new String object but which points back to the same char array as the parent (but with different offset and length)

**Canonicalisation** is the technique of ensuring that there is only one actual object with a given unique content, then every time we need an object with that content, we create a reference to the single object.

The running JVM actually contains a **string pool**, which it uses for any literal string hardcoded in classes and other special purposes such as class and method names etc**. You can ask the VM to pool any given string by calling the intern() method on that string**.

The principal *problem* with this approach is that **there is no way to de-intern a String** once it has been interned. Thus, using intern() is liable to cause a **memory leak** if you are not careful. We also have no real way to query the current status of the JVM's internal string pool and find out how full it is— the first we'll know is probably when we get an OutOfMemoryError calling intern() 

In general, you should **never call intern() on user-generated strings**, as your application will be susceptible to an attack whereby the user deliberately sends a large number of different strings to your application and fills up the memory with pooled strings that will never be removed.

**String interning** is a method of storing only one copy of each distinct [string](https://en.wikipedia.org/wiki/String_(computer_science)) value, which must be [immutable](https://en.wikipedia.org/wiki/Immutable_object)

The single copy of each string is called its 'intern' and is typically looked up by a method of the string class, for example [String.intern--](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#intern--) in Java. All compile-time constant strings in Java are automatically interned using this method.

Objects other than strings can be interned. For example, in Java, when primitive values are [boxed](https://en.wikipedia.org/wiki/Object_type_(object-oriented_programming)#Boxing) into a [wrapper object](https://en.wikipedia.org/wiki/Primitive_wrapper_class), certain values (any boolean, any byte, any char from 0 to 127, and any shortor int between −128 and 127) are interned, and any two boxing conversions of one of these values are guaranteed to result in the same object.
