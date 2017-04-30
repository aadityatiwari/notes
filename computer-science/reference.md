[Reference (computer science)](https://en.wikipedia.org/wiki/Reference_(computer_science))
------------

The reference is said to **refer** to the datum, and accessing the datum is called [**dereferencing**](https://en.wikipedia.org/wiki/Dereference_operator) the reference.


Typically, for references to data stored in memory on a given system, a reference is implemented as the [physical address](https://en.wikipedia.org/wiki/Physical_address) of where the data is stored in memory or in the storage device. For this reason, a reference is often erroneously confused with a [*pointer*](https://en.wikipedia.org/wiki/Pointer_(computer_programming)) or [*address*](https://en.wikipedia.org/wiki/Memory_address), and is said to "point to" the data.


However a reference may also be implemented in other ways, such as the offset (difference) between the datum's address and some fixed "base" address, as an [index](https://en.wikipedia.org/wiki/Array_index) into an [array](https://en.wikipedia.org/wiki/Array_data_structure), or more abstractly as a [handle](https://en.wikipedia.org/wiki/Handle_(computing)). More broadly, in networking, references may be *network* addresses, such as [URLs](https://en.wikipedia.org/wiki/URL).


References can cause significant complexity in a program, partially due to the possibility of [dangling](https://en.wikipedia.org/wiki/Dangling_reference) and [wild references](https://en.wikipedia.org/wiki/Wild_reference) and partially because the [topology](https://en.wikipedia.org/wiki/Topology) of data with references is a [directed graph](https://en.wikipedia.org/wiki/Directed_graph), whose analysis can be quite complicated.


Many object oriented languages make extensive use of references. They may use references to access and [assign](https://en.wikipedia.org/wiki/Assignment_(computer_science)#Assignment_in_object_oriented_languages) objects. References are also used in function/[method](https://en.wikipedia.org/wiki/Method_(computer_programming)) calls or message passing, and [reference counts](https://en.wikipedia.org/wiki/Reference_counting) are frequently used to perform [garbage collection](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)) of unused objects.